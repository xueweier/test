---
layout: post
title: Docker 新手上路
category: tech
tags: docker maintenance
---

新学习了 [docker][docker_gitbook]。记录一下。

Docker 是个划时代的开源项目，它彻底释放了计算虚拟化的威力，极大提高了应用的运行效率，降低了云计算资源供应的成本！ 使用 Docker，可以让应用的部署、测试和分发都变得前所未有的高效和轻松！

无论是应用开发者、运维人员、还是其他信息技术从业人员，都有必要认识和掌握 Docker，以在有限的时间内做更多有意义的事。

# 安装

先查看服务器的版本。`lsb_release -a`

    Distributor ID:	Debian
    Description:	Debian GNU/Linux 8.7 (jessie)
    Release:	8.7
    Codename:	jessie
    
### 添加源

按照教程的说法，`Debian 8 的内核默认为 3.16，仅仅满足基本的 Docker 运行条件。如果打算使用 overlay2 存储层驱动，或某些功能不够稳定希望升级到较新版本的内核，可以添加 backports 源，升级到新版本的内核。`
执行下面的命令添加 backports 源：

    $ echo "deb http://http.debian.net/debian jessie-backports main" | sudo tee /etc/apt/sources.list.d/backports.list
    
升级到 backports 内核：

    $ sudo apt-get update
    $ sudo apt-get -t jessie-backports install linux-image-amd64
    
### 配置存储驱动

升级到 backports 的内核之后，会因为 AUFS 内核模块不可用，而使用默认的 devicemapper 驱动，并且配置为 loop-lvm，这是不推荐的。因此，不要忘记安装 Docker 后，配置 overlay2 存储层驱动。（这一步我跳过了，待熟悉以后再看看存储驱动选择的问题）.

### 安装 Docker

在一切准备就绪后，就可以安装最新版本的 Docker 了，软件包名称为 docker-engine。将当前用户加入 docker 组，然后启动引擎。

    $ sudo apt-get install docker-engine
    $ sudo usermod -aG docker $USER
    $ sudo systemctl enable docker
    $ sudo systemctl start docker




# 获取/定制镜像

[Docker Hub][docker_hub]，[Docker Store][docker_store] 上有大量的高质量的镜像可以用。有可以直接拿来使用的服务类的镜像，如 nginx、redis、mongo、mysql、httpd、php、tomcat 等； 也有一些方便开发、构建、运行各种语言应用的镜像，如 node、openjdk、python、ruby、golang 等。 可以在其中寻找一个最符合我们最终目标的镜像为基础镜像进行定制。 如果没有找到对应服务的镜像，官方镜像中还提供了一些更为基础的操作系统镜像，如 ubuntu、debian、centos、fedora、alpine 等，这些操作系统的软件库为我们提供了更广阔的扩展空间。

获取镜像命令：

    docker pull [选项] [Docker Registry地址]<仓库名>:<标签>

例如`docker pull debian`,安装完成后显示镜像列表`docker images`，如下

    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    debian              latest              19134a8202e7        4 weeks ago         123 MB

有了镜像后，我们就可以以这个镜像为基础启动一个容器来运行。以上面的 debian 为例，启动里面的 bash 并且进行交互式操作的话，可以执行下面的命令。

    $ docker run -it --rm debian bash

![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201701/QQ%E6%88%AA%E5%9B%BE20170117000548.jpg)

    -it：这是两个参数，一个是 -i：交互式操作，一个是 -t 终端。我们这里打算进入 bash 执行一些命令并查看返回结果，因此我们需要交互式终端。
    --rm：这个参数是说容器退出后随之将其删除。默认情况下，为了排障需求，退出的容器并不会立即删除，除非手动 docker rm。
    debian：这是指用 debian 镜像为基础来启动容器。
    bash：放在镜像名后的是命令，这里我们希望有个交互式 Shell，因此用的是 bash。 
    最后 exit 退出容器。

这篇就到这里。下一篇再写其他方面的。

参考资料：

[docker_gitbook]: https://www.gitbook.com/book/yeasy/docker_practice
[select_a_docker_storage_driver]: https://www.centos.bz/2016/12/select-a-docker-storage-driver
[docker_hub]: https://hub.docker.com
[docker_store]: https://store.docker.com
