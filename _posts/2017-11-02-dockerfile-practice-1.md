---
layout: post
title: dockerfile 实践
category: tech
tags: docker
---
![](https://cdn.kelu.org/blog/tags/docker.jpg)

看过了这么多的教程，我仍然写不好一个 Dockerfile 文件233333333。主要原因还是没有自己动手实践，把自己的项目一行一行敲出来做成一个镜像。

在这篇文档中，我将在我 GitHub 上的项目 —— KeluLinuxKit 作为基础，对照其中的命令移植到 Dockerfile 里来，其中涉及的命令行的知识就不做解读了，不明白的童鞋还请自己谷歌或者查看文档。

# 准备

先看一下我原生的脚本：
	
	install_openresty(){
	    cd $DOWNLOAD
	    aptitude -y install libreadline-dev libpcre3-dev libssl-dev libpq-dev
	    wget https://openresty.org/download/ngx_openresty-1.9.7.1.tar.gz
	    tar -xzvf ngx_openresty-1.9.7.1.tar.gz
	    cd ngx_openresty-1.9.7.1/
	    ./configure --prefix=/usr/share/openresty --with-pcre-jit --with-http_postgres_module --with-http_iconv_module --with-http_stub_status_module
	    make && make install
	
	    mkdir /var/local/nginx
	    mkdir /var/local/log/nginx
	    cp -R $NGINX_HOME /var/local
	    cd /var/local/nginx
	    mkdir conf/vhost
	
	    cp $RESOURCE/nginx/* /var/local/nginx/
	}

这部分命令行主要做的是 openresty 的下载、编译然后拷贝现有的配置。

# 什么是 dockerfile

接触过容器的朋友应该都知道，`Dockerfile`是由一系列命令和参数构成的脚本，使用 Dockerfile 可以创建容器的镜像。

一个`Dockerfile`里面包含了构建一个容器镜像的完整命令。Docker通过`docker build`执行`Dockerfile`中的一系列命令自动构建`image`。


# dockerfile 介绍

首先，我使用一个简单的例子介绍一下 dockerfile。 一个标准最小化的 dockerfile 格式应该是这样子的：

```
FROM nginx
MAINTAINER kelvinblood <admin@kelu.org>
RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
ENTRYPOINT ["nginx", "-g", "daemon off;"]
```
它包含四个部分：

*   基础镜像（父镜像）信息
*   维护者信息
*   镜像操作命令
*   容器启动命令

完成之后，我们只需要简单的的使用 `docker build .` 命令，就可以将它制作出来了。

# build

通过上面的介绍，我们对 dockerfile 已经有了一个大概的认识，现在先来看如何制作镜像。

	docker build CONTENT

`docker build`命令从`Dockerfile`和`context`中构建image。`context`可以使是`PATH`或`URL`。`PATH`是本地文件目录。 `URL`则是 Git 仓库的位置。

`context`以递归方式处理。所以`PATH`会包括目录下的所有子目录，`URL`会包括仓库和仓库的子模块。下面是一个使用当前目录作为`context`的构建镜像的命令：

```
$ docker build .
Sending build context to Docker daemon  6.51 MB
...

```
构建由Docker 守护程序运行，而不是由 Docker 命令行运行。在这里放上一个 Docker 的架构图帮助理解。关于Docker守护进程和命令行的内容在本文就不展开了。

![](https://cdn.kelu.org/blog/2017/11/docker-architecture.jpg)

![](https://cdn.kelu.org/blog/2017/11/docker-interaction.jpg)

构建过程所做的第一件事是将整个context递归地发送给守护进程。一个比较好的做法是将`Dockerfile`和所需文件复制到一个空的目录，再到这个目录进行构建。(尤其注意不要把根目录这种传过去）

以下是一些与 build 相关的小知识点：

* 可以通过`.dockerignore` 来排除 content 中的文件和目录。这样能提高构建的性能。
* 一般默认`Dockerfile`位于`context`的根目录下。也可以使用`-f`标志可指定Dockerfile的位置。

	```
	$ docker build -f /path/to/a/Dockerfile .
	
	```
* 用 -t 保存为新的镜像，也可以保存成多个镜像：

	```
	$ docker build -t shykes/myapp .
	$ docker build -t shykes/myapp:1.0.2 -t shykes/myapp:latest .
	```

* Docker守护程序会按顺序运行`Dockerfile`中的指令。
* Docker 是根据 dockerfile 中的每一个指令进行镜像构建。在 dockerfile 中运行`RUN cd /tmp`对下一个指令不会有任何影响。Docker 每个指令都会产生中间镜像，以此来加速`docker build`的过程。

# 格式

## 解析器指令

用来做一些解析的设置，目前只支持 escape。

有一些注意事项，例如必须置于 `From` 之前，不能重复，不能换行，未知指令视为注释。

* escape

	用于在`Dockerfile`中转义字符的字符。如果未指定，则缺省转义字符为`\`。 
		
		# escape=\ (backslash)
			
	或
		
		# escape=` (backtick)
	
## ENV

ENV 主要用于设置变量，使用 `$variable_name`或 `${variable_name}` 进行访问。一般用后者，可以进行下面这种神奇的操作：

	${foo}_bar

同时还支持几个bash修饰符

*   `${variable:-word}`表示默认值是`word`，没有值则为空字符串。
*   `${variable:+word}`表示默认值是空字符串，否则是`word`。

除此之外 ENV 还可以用来处理转义。

## .dockerignore

docker CLI 将 content 发送到 docker 守护程序之前，它会项目根目录中查找名为`.dockerignore`的文件。如果文件存在，CLI 将排除匹配的文件和目录后再将 content 发送给守护程序。

使用方法与 .gitignore 类似，这里就不展开说明了。

## FROM

	FROM <image> 
	FROM <image>:<tag>
	FROM <image>@<digest>

* 除了注释，from 必须作为第一个命令。
* 可以出现多次。
* `tag`或`digest`可以不填。如果省略其中任何一个，构建器将默认使用`latest`。

## MAINTAINER

```
MAINTAINER <name>

```
指镜像维护的作者信息。

待续，我先提交个代码。

# 参考资料

* [Docker命令行与守护进程如何交互？](https://blog.fundebug.com/2017/05/22/docker-cli-daemon/)
