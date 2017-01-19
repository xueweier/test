---
layout: post
title: Docker 新手上路（二）——Dockerfile
category: tech
tags: docker maintenance
---

今天的好玩新闻有这几件事：

* 英国梅姨一声硬脱欧，英镑兑美元涨了将近2.6%
* 支付宝又搞起了敬业福，冠华是五福红包的产品经理。
* 甲骨文北京裁员2000人。
* 最近火起来一句话，“我可能是x假的x”。
* 一位微信陌生好友给我推荐了一个专门收集互联网大会的[网站][huodongjia]

前几天新学习了 [docker][docker_gitbook] 的基本知识，包括安装、环境配置、获取和运行镜像等最基础的东西。今天学习一些 docker 的启动查看定制等操作。

# 列出镜像

    $ docker images
    $ docker images -a // 中间层镜像
    $ docker images xxx // 特定镜像

# 运行镜像

    $ docker run -it --rm debian bash
    $ docker run --name webserver -d -p 1644:80 nginx
    $ docker run --name web2 -d -p 81:80 nginx:v2
    
# 定制镜像

镜像的定制实际上就是定制每一层所添加的配置、文件。如果我们可以把每一层修改、安装、构建、操作的命令都写入一个脚本，用这个脚本来构建、定制镜像，那么之前提及的无法重复的问题、镜像构建透明性的问题、体积的问题就都会解决。这个脚本就是 Dockerfile。
用过 PHP Laravel 框架的应该马上就理解了（其他例如java kotlin等也是类似），这个就和 composer.json 脚本一样，用来保证环境一致性的配置文件。

定制镜像需要用到下面几个命令

* FROM 指定基础镜像
* RUN 执行命令
* BUILD 构建镜像

Dockerfile 正确的写法应该是这样：

    FROM xxx

    RUN buildDeps='gcc libc6-dev make' \
        && apt-get update \
        && apt-get install -y $buildDeps \
        && wget -O redis.tar.gz "http://download.redis.io/releases/redis-3.2.5.tar.gz" \
        && mkdir -p /usr/src/redis \
        && tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1 \
        && make -C /usr/src/redis \
        && make -C /usr/src/redis install \
        && rm -rf /var/lib/apt/lists/* \
        && rm redis.tar.gz \
        && rm -r /usr/src/redis \
        && apt-get purge -y --auto-remove $buildDeps




然后在该目录执行

    $ docker build -t nginx:v3 .
    
使用 docker build 命令进行镜像构建。其格式为：
    
    docker build [选项] <上下文路径/URL/->
    
在这里我们指定了最终镜像的名称 -t nginx:v3。
一般大家习惯性的会使用默认的文件名 Dockerfile，以及会将其置于镜像构建上下文目录中。

# Docker Build 的用法

* Dockerfile  `docker build -t nginx:v3 .`
* Git repo    `docker build https://github.com/twang2218/gitlab-ce-zh.git#:8.14`
* tar 压缩包  `docker build http://server/context.tar.gz`
* 标准输入中读取 Dockerfile

        docker build - < Dockerfile
        cat Dockerfile | docker build -

* 标准输入中读取上下文压缩包 `docker build - < context.tar.gz`

参考资料：

* [Dockerfie 官方文档](https://docs.docker.com/engine/reference/builder/)
* [Dockerfile 最佳实践文档](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/)

[docker_gitbook]: https://www.gitbook.com/book/yeasy/docker_practice
[select_a_docker_storage_driver]: https://www.centos.bz/2016/12/select-a-docker-storage-driver
[docker_hub]: https://hub.docker.com
[docker_store]: https://store.docker.com
[huodongjia]: http://www.huodongjia.com/it/
