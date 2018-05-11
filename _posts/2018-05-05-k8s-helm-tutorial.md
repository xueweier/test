---
layout: post
title: kubernetes helm 入门
category: tech
tags: kubernetes docker
---
![](https://cdn.kelu.org/blog/tags/k8s.jpg)

这篇文章记录如何安装和使用helm。

# 什么是 helm

Kubernetes是容器集群管理系统，每个成功的软件平台都有一个优秀的打包系统，比如 Debian、Ubuntu 的 apt，Redhat、Centos 的 yum。而 Helm 则是 Kubernetes 上的包管理器。

helm相当于一个应用商店，将一个在云上部署的应用相关的组件全部打包起来，进行安装、升级、管理等。

在kubernetes中，我们部署一个标准的应用，以下的应用基本都会使用到：

1. Service
2. Secret
3. PVC
4. Deployment
5. ConfigMap
6. ...

helm 有几个概念名词：chart，release和repository。chart就是包，而release代表一个运行实例，Repository是用于发布和存储 Chart 的存储库。

helm 的工作，就是将这些yaml配置在更高一个层次，统一起来管理：

1. 新建chart。
2. 更新chart。
3. 在kubernetes中安装和卸载release。
4. 更新、回滚和测试release。



# helm 架构

helm 包含两个组件：helm 客户端 和 tiller 服务器：

* helm客户端负责管理chart
* tiller服务器负责管理release
* Repository 是 Chart 存储库，Helm 客户端通过 HTTP 协议来访问存储库中 Chart 的索引文件和压缩包。

![](https://cdn.kelu.org/blog/2018/05/helm-arch.jpg)

# helm安装

1. helm客户端

   ```
   curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get | bash
   helm version

   # 安装helm命令补全脚本
   cd ~ && helm completion bash > .helmrc && echo "source .helmrc" >> .bashrc
   ```

2. tiller服务器

   创建tiller的`serviceaccount`和`clusterrolebinding`

   ```
   kubectl create serviceaccount --namespace kube-system tiller
   kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
   ```

   然后安装helm服务端tiller

   ```
   helm init -i sz-pg-oam-docker-hub-001.tendcloud.com/library/kubernetes-helm-tiller:v2.9.0
   ```

   使用`-i`指定自己的镜像，因为官方的镜像因为某些原因无法拉取。

   为应用程序设置`serviceAccount`：

   ```
   kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'
   ```

   检查是否安装成功：

   ```
   $ kubectl -n kube-system get pods|grep tiller
   tiller-deploy-f5597467b-wdwfl      1/1       Running   0          2h
   $ helm version
   Client: &version.Version{SemVer:"v2.9.0", GitCommit:"f6025bb9ee7daf9fee0026541c90a6f557a3e0bc", GitTreeState:"clean"}
   Server: &version.Version{SemVer:"v2.9.0", GitCommit:"f6025bb9ee7daf9fee0026541c90a6f557a3e0bc", GitTreeState:"clean"}
   ```

tips：

> 安装出问题时使用  helm reset  && helm init 重置安装。

3. 使用第三方的 Chart 存储库

   ```
   helm repo add 存储库名 存储库URL
   helm repo update
   ```

   关于 Helm 相关命令的说明，您可以参阅 [Helm 文档](https://docs.helm.sh/helm/#helm-repo-add)

4. 使用helm安装应用

   ```
   helm install stable/mysql
   helm list
   ```

   因为我们还没有准备 PersistentVolume，当前 release 还不可用。

# UI安装





未完待续... 

# 参考资料

* [Error: no available release name found #3055](https://github.com/kubernetes/helm/issues/3055)
* [利用 Helm 简化 Kubernetes 应用部署](https://www.alibabacloud.com/help/zh/doc-detail/58587.htm)
* [Monocular UI](https://kubeapps.com/)
* <https://jimmysong.io/kubernetes-handbook/>
* [kubernetes-helm部署及本地repo搭建](https://blog.csdn.net/liukuan73/article/details/79319900)