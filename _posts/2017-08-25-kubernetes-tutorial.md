---
layout: post
title: kubernetes 架构简介
category: tech
tags: virtualization kubernetes
---
![](/assets/img/k8s.jpg)

最近接触了一些 kubernetes 的知识，记录一下。

# 什么是Kubernetes？

Kubernetes（k8s）是 google 开源的一套自动化容器管理平台，前身是 Borg，用于容器的部署、自动化调度和集群管理。可以将Docker看成Kubernetes内部使用的低级别组件。Kubernetes不仅仅支持Docker，还支持Rocket，这是另一种容器技术。

Kubernetes 是一套分布式系统，由多个节点组成，节点分为两类：一类是属于管理平面的主节点/控制节点（Master Node）；一类是属于运行平面的工作节点（Worker Node）。

使用Kubernetes可以：

*   自动化容器的部署和复制 
*   随时扩展或收缩容器规模 
*   将容器组织成组，并且提供容器间的负载均衡 
*   很容易地升级应用程序容器的新版本 
*   提供容器弹性，如果容器失效就替换它，等等...

kubernetes 的总体概览如下图：

![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201709/1026003.png)

下面介绍一些核心概念。

# 集群

集群是一组节点，这些节点可以是物理服务器或者虚拟机。以下是 k8s 典型的架构图：

总视角一：

![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201709/1026000.png)

总视角二：

![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201709/1026002.png)

Master 视角：

![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201709/1026001.png)

Minion 视角一：

![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201709/1026002.png)

Minion 视角二：

![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201709/k8s-1.png)



# Minion

从 Minion 视角二图可以看到如下组件：

*   Pod
*   Container（容器）
*   Label（标签）
*   Replication Controller（复制控制器）
*   Service（服务）
*   Node（节点）
*   Kubernetes Master（Kubernetes主节点）

### Pod

Pod 安排在节点上，包含一组容器和卷。同一个Pod里的容器共享同一个网络命名空间，可以使用localhost互相通信。Pod是短暂的，不是持续性实体。

### Lable

如图所示，一些Pod有Label。Label是一对键/值对，用来传递用户定义的属性。比如，你可能创建了一个"tier"和“app”标签，通过Label（**tier=frontend, app=myapp**）来标记前端Pod容器，使用Label（**tier=backend, app=myapp**）标记后台Pod。

### Replication Controller

Replication Controller确保任意时间都有指定数量的Pod“副本”在运行。
当创建Replication Controller时，需要指定两个东西：

1.  Pod模板 ：用来创建Pod副本的模板
2.  Label：Replication Controller需要监控的Pod的标签。

现在已经创建了Pod的一些副本，那么在这些副本上如何均衡负载呢？我们需要的是Service。

### Service

Service 是定义一系列Pod以及访问这些Pod的策略的一层抽象。Service通过Label找到Pod组。因为Service是抽象的，所以在图表里通常看不到它们的存在。

### Node

节点是物理或者虚拟机器，通常称为Minion。每个节点都运行如下Kubernetes关键组件：

*   Kubelet：是主节点代理。
*   Kube-proxy：Service使用其将链接路由到Pod，如上文所述。
*   Docker或Rocket：Kubernetes使用的容器技术来创建容器。

Minion 有两个重要的组件 Kubelet 和 Proxy，如下。

### Kuberlet 

Kubelet是Kubernetes集群中每个Minion和Master API Server的连接点，Kubelet运行在每个Minion上，是Master API Server和Minion之间的桥梁，接收Master API Server分配给它的commands和work，与持久性键值存储etcd、file、server和http进行交互，读取配置信息。Kubelet的主要工作是管理Pod和容器的生命周期，具体工作如下：

1) 通过Worker给Pod异步运行特定的Action。

2) 设置容器的环境变量。

3) 给容器绑定Volume。

4) 给容器绑定Port。

5) 根据指定的Pod运行一个单一容器。

6) 杀死容器。

7) 给指定的Pod创建network 容器。

8) 删除Pod的所有容器。

9) 同步Pod的状态。

10) 从Cadvisor获取container info、 pod info、root info、machine info。

11) 检测Pod的容器健康状态信息。

12) 在容器中运行命令。

### Proxy

Proxy是为了解决外部网络能够访问跨机器集群中容器提供的应用服务而设计的，运行在每个Minion上。

Proxy 为 pod 提供代理服务，用来实现 kubernetes 的 service 机制。每个 service 都会被分配一个虚拟 ip，当应用访问到 service ip 和端口的时候，会先发送到 kube-proxy，然后才转发到机器上的容器服务。

Proxy提供TCP/UDP sockets的proxy，每创建一种Service，Proxy主要从etcd获取Services和Endpoints的配置信息，或者也可以从file获取，然后根据配置信息在Minion上启动一个Proxy的进程并监听相应的服务端口，当外部请求发生时，Proxy会根据Load Balancer将请求分发到后端正确的容器处理。

# Master

kubernetes 是典型的 master-slave 模式，master 是整个集群的大脑，负责控制集群的方方面面。Kubernetes Master拥有一系列组件，提供如下的管理服务：

*   apiserver 是整个系统的对外接口，提供一套 RESTful 的 Kubernetes API，供客户端和其它组件调用；
*   scheduler 负责对资源进行调度，分配某个 pod 到某个节点上。是 pluggable的，意味着很容易选择其它实现方式；
*   controller-manager 负责管理控制器，包括 
		* endpoint-controller（刷新服务和 pod 的关联信息）
		* eplication-controller（维护某个 pod 的复制为配置的数值）


从上文 Master 视角图简单介绍下 Master 的工作流：

1) Kubecfg将特定的请求，比如创建Pod，发送给Kubernetes Client。

2) Kubernetes Client将请求发送给API server。

3) API Server根据请求的类型，比如创建Pod时storage类型是pods，然后依此选择何种REST Storage API对请求作出处理。

4) REST Storage API对的请求作相应的处理。

5) 将处理的结果存入高可用键值存储系统Etcd中。

6) 在API Server响应Kubecfg的请求后，Scheduler会根据Kubernetes Client获取集群中运行Pod及Minion信息。

7) 依据从Kubernetes Client获取的信息，Scheduler将未分发的Pod分发到可用的Minion节点上。

### 各种 Registry

这一块我只在 InfoQ 的文章里看到过，目前并不知道这个Registry是什么23333333

* Minion Registry - 负责跟踪Kubernetes 集群中有多少Minion(Host)
* Pod Registry - 负责跟踪Kubernetes集群中有多少Pod在运行，以及这些Pod跟Minion是如何的映射关系。
* Service Registry - 负责跟踪Kubernetes集群中运行的所有服务。
* Controller Registry - 负责跟踪Kubernetes集群中所有的Replication Controller。
* Endpoints Registry - 负责收集Service的endpoint。
* Binding Registry - 绑定需要绑定Pod的ID和被绑定的Host

### controller-manager

用来执行整个系统中的后台任务，它其实是多个控制进程的合体。大致包括如下

*   Node Controller 负责整个系统中 node up 或 down 的状态的响应和通知；
*   Replication Controller 负责维持 Pods 中的正常运行的 pod 的个数；
*   Endpoints Controller 负责维持 Pods 和 Service 的关联关系；
*   Service Account & Token Controllers 负责为新的命名空间创建默认的账号和 API 访问 Token；

### Scheduler 

具体来说，Scheduler做以下工作：

1) 实时监测Kubernetes集群中未分发的Pod。

2) 实时监测Kubernetes集群中所有运行的Pod，Scheduler需要根据这些Pod的资源状况安全地将未分发的Pod分发到指定的Minion节点上。

3) Scheduler也监测Minion节点信息，由于会频繁查找Minion节点，Scheduler会缓存一份最新的信息在本地。

4) 最后，Scheduler在分发Pod到指定的Minion节点后，会把Pod相关的信息Binding写回API Server。


# Etcd

etcd 是 kubernetes 存放集群状态和配置的地方，这是集群状态同步的关键，所有节点都是从 etcd 中获取集群中其他机器状态的；集群中所有容器的状态也是放在这里的，很容易实现主节点的分布式扩展。

组件可以自动的去侦测 Etcd 中的数值变化来获得通知，并且获得更新后的数据来执行相应的操作。

# 调侃

Kubernetes"实际上是一个希腊词κυβερνήτης, 意思是"船的舵手"。 在这个意义上,Kubernetes对于Docker(集装箱船)倒是挺般配的。

# 参考资料

* [十分钟带你理解Kubernetes核心概念 - dockone.io](http://dockone.io/article/932)
* [Kubernetes系统架构简介 - InfoQ](http://www.infoq.com/cn/articles/Kubernetes-system-architecture-introduction)
* [kubernetes - gitbook](https://yeasy.gitbooks.io/docker_practice/content/kubernetes/design.html)
* [kubernetes 简介：kubernetes 架构介绍](http://cizixs.com/2016/07/12/kubernetes-intro)
*   [kubernetes 系统架构简介](http://www.infoq.com/cn/articles/Kubernetes-system-architecture-introduction)
*   [introduction to kubernetes](http://www.slideshare.net/rajdeep/introduction-to-kubernetes)
* [如果有10000台机器，你想怎么玩？](http://qinghua.github.io/kubernetes-in-mesos-1/ "如果有10000台机器，你想怎么玩？（一）概述")
* [Kubernetes 基础介绍](http://lonf.me/2017/03/08/Introduction-to-Kubernetes/#kube-controller-manager)
*  [Kubernetes - paddlepaddle.org](http://doc.paddlepaddle.org/doc_cn/howto/usage/k8s/k8s_basis_cn.html)
* [Docker_容器与容器云（第2版）.pdf](http://kaiyinghe.com/Docker_容器与容器云（第2版）.pdf)
