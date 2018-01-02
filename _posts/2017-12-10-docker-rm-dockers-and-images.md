---
layout: post
title: 删除 Docker 容器和镜像 | 转
category: tech
tags: docker
---
![](https://cdn.kelu.org/blog/tags/docker.jpg)

## 删除所有未运行 Docker 容器

```
docker rm $(docker ps -a -q)
```

## 删除所有 Docker 镜像

删除所有未打 tag 的镜像

```
docker rmi $(docker images -q | awk '/^<none>/ { print $3 }')
```
删除所有镜像

```
docker rmi $(docker images -q)
```
根据格式删除所有镜像

```
docker rm $(docker ps -qf status=exited)
```
# 参考资料

* [# [Docker 技巧：删除 Docker 容器和镜像](https://segmentfault.com/a/1190000004491286)](https://segmentfault.com/a/1190000004491286)