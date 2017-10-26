---
layout: post
title: gitlab 入门（四）—— docker build 创建自己的镜像
category: tech
tags: git gitlab docker
---
![](https://cdn.kelu.org/blog/tags/gitlab.jpg)

这一篇主要来看如何使用 docker build 创建自己的镜像


# 安装Go

访问[下载地址](http://golang.org/dl/ "Go安装包下载")，32位系统下载go1.8.3.linux-386.tar.gz，64位系统下载go1.8.3.linux-amd64.tar.gz，以我这一次下载为例：

	wget https://storage.googleapis.com/golang/go1.9.2.linux-amd64.tar.gz
	tar -C /usr/local -xzf go1.9.2.linux-amd64.tar.gz

	# 添加到系统路径
	ln -s /usr/local/go/bin/go /usr/local/bin/go

# build示例

以 [sameersbn](https://github.com/sameersbn)/**[docker-gitlab](https://github.com/sameersbn/docker-gitlab)** 作为示例项目进行创建。

	git clone https://github.com/sameersbn/docker-gitlab.git
	cd docker-gitlab
	docker build --tag="$USER/gitlab" .

build 很慢，耐心等等~

... 
待续。

# 参考资料

* [build-web-application-with-golang](https://github.com/astaxie/build-web-application-with-golang)
* [docker 操作命令详解](http://www.simapple.com/docker-commandline)
* [docker build 命令-建立一个新的image](http://www.simapple.com/319.html)
* [Dockerfile配置文件说明文档详解](http://www.simapple.com/docker-dockerfile)

