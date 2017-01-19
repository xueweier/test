---
layout: post
title: Docker 新手上路（三）
category: tech
tags: docker maintenance
---

今天的好玩新闻有这几件事：

* [炉石传说罕见数据库事故！丢失30%数据](http://mp.weixin.qq.com/s/Vr36GpzR-UxcNLtiBLldEA)

前几天新学习了 [docker][docker_gitbook] 的基本知识，包括安装、环境配置、获取和运行镜像等最基础的东西。今天看看一些简单的网络配置和相关的开源软件。更加深入的内容暂时就不研究了。毕竟作为新手上路已经足够了。

# 外部访问容器

容器中可以运行一些网络应用，要让外部也可以访问这些应用，可以通过 -P 或 -p 参数来指定端口映射。

    docker run --name web -d -p 1644:80 nginx
    docker run -d -p 127.0.0.1:5000:5000/udp training/webapp python app.py
    docker logs -f web

使用 docker port 来查看当前映射的端口配置，也可以查看到绑定的地址

    docker port web

使用 docker inspect xxx 可以获取特定容器内部网络和 ip 地址所有的变量，-p 标记可以多次使用来绑定多个端口

# 容器互联

(待完成)
    
参考资料：

* [docker_gitbook][docker_gitbook]

[docker_gitbook]: https://www.gitbook.com/book/yeasy/docker_practice
[select_a_docker_storage_driver]: https://www.centos.bz/2016/12/select-a-docker-storage-driver
[docker_hub]: https://hub.docker.com
[docker_store]: https://store.docker.com
[huodongjia]: http://www.huodongjia.com/it/
