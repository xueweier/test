---
layout: post
title: kubernetes 简介
category: tech
tags: virtualization kubernetes docker
---
![](https://cdn.kelu.org/blog/tags/k8s.jpg)

kubernetes 网上的资料良莠不均，刚开始表示完全不知道从何入手。最近在入门中，将一些资料做了整理，精简了大部分得出了这一篇文章。可以参照文末参考资料查看原文。当前最新版的k8s版本为：1.10.

# 什么是Kubernetes？

Kubernetes（k8s）是 google 开源的一套自动化容器管理平台，前身是 Borg，用于容器的部署、自动化调度和集群管理。

## 为什么使用容器？

传统的应用部署方式是通过插件或脚本来安装应用。这样做的缺点是应用的运行、配置、管理、所有生存周期将与当前操作系统绑定，这样做并不利于应用的升级更新/回滚等操作，当然也可以通过创建虚机的方式来实现某些功能，但是虚拟机非常重，并不利于可移植性。

新的方式是通过部署容器方式实现，每个容器之间互相隔离，每个容器有自己的文件系统 ，容器之间进程不会相互影响，能区分计算资源。相对于虚拟机，容器能快速部署，由于容器与底层设施、机器文件系统解耦的，所以它能在不同云、不同版本操作系统间进行迁移。

![为什么使用容器](https://cdn.kelu.org/blog/2018/05/why_containers.svg)

## kubernetes能做什么？

可以在物理或虚拟机的Kubernetes集群上运行容器化应用，Kubernetes能提供一个以“**容器为中心的基础架构**”，满足在生产环境中运行应用的一些常见需求：负载均衡、服务发现、高可用、滚动升级、自动伸缩等。



Kubernetes是为生产环境而设计的容器调度管理系统，对于负载均衡、服务发现、高可用、滚动升级、自动伸缩等容器云平台的功能要求有原生支持。

## kubernetes不能做什么？

- Kubernetes不提供中间件（如message buses）、数据处理框架（如Spark）、数据库(如Mysql)或者集群存储系统(如Ceph)作为内置服务。但这些应用都可以运行在Kubernetes上面。
- Kubernetes 不部署源码不编译应用。持续集成的 (CI)工作流方面，不同的用户有不同的需求和偏好的区域，因此，我们提供分层的 CI工作流，但并不定义它应该如何工作。
- Kubernetes 允许用户选择自己的日志、监控和报警系统。
- Kubernetes 不提供或授权一个全面的应用程序配置 语言/系统。
- Kubernetes 不提供任何机器配置、维护、管理或者自修复系统。

## Kubernetes是什么意思？

Kubernetes 实际上是一个希腊词κυβερνήτης, 意思是"船的舵手"。*K8s*是将中间8个字母“ubernete”替换为“8”的缩写。

# Kubernetes 架构

一个K8s集群是由分布式存储（etcd）、服务节点（Minion，也称为Node）和控制节点（Master）构成的。所有的集群状态都保存在etcd中，Master节点上则运行集群的管理控制模块。Node节点是真正运行应用容器的主机节点，在每个Minion节点上都会运行一个Kubelet代理，控制该节点上的容器、镜像和存储卷等。

![](https://cdn.kelu.org/blog/2018/05/000.jpg)

K8s在实现上述架构时基于以下架构理念：

- 只有API Server与存储通信，其他模块通过API Server访问集群状态。这样第一，是为了保证集群状态访问的安全。第二，是为了隔离集群状态访问的方式和后端存储实现的方式：API Server是状态访问的方式，不会因为后端存储技术etcd的改变而改变。加入以后将etcd更换成其他的存储方式，并不会影响依赖依赖API Server的其他K8s系统模块。
- 一个工作节点被攻破不能导致整个K8s集群被攻破。这是所有分布式系统架构设计中都应该考虑的问题。
- 考虑网络随时可能断开的情况，没有新配置声明时各模块按照之前的配置声明继续工作。在K8s集群中，所有的配置管理操作都声明式而非命令式的，因为声明式操作对于网络故障等分布式系统常见的故障情况更加稳定。
- 各个模块在内存中缓存自己的相关状态以提高系统性能。
- 需要监控某个系统状态来做下一步动作的时候，优先考虑观察通知模式，其次再考虑轮询模式，这也是为了提高系统的响应速度。

## 分层架构

Kubernetes设计理念和功能其实就是一个类似Linux的分层架构，如下图所示

![img](https://cdn.kelu.org/blog/2018/05/kubernetes2.jpg)

- 核心层：Kubernetes最核心的功能，对外提供API构建高层的应用，对内提供插件式应用执行环境
- 应用层：部署（无状态应用、有状态应用、批处理任务、集群应用等）和路由（服务发现、DNS解析等）
- 管理层：系统度量（如基础设施、容器和网络的度量），自动化（如自动扩展、动态Provision等）以及策略管理（RBAC、Quota、PSP、NetworkPolicy等）
- 接口层：kubectl命令行工具、客户端SDK以及集群联邦
- 生态系统：在接口层之上的庞大容器集群管理调度的生态系统，可以划分为两个范畴
  - Kubernetes外部：日志、监控、配置管理、CI、CD、Workflow、FaaS、OTS应用、ChatOps等
  - Kubernetes内部：CRI、CNI、CVI、镜像仓库、Cloud Provider、集群自身的配置和管理等



# Kubernetes的核心技术概念

API对象是K8s集群中的管理操作单元。K8s集群系统每支持一项新功能，引入一项新技术，一定会新引入对应的API对象，支持对该功能的管理操作。例如副本集Replica Set对应的API对象是RS。

每个API对象都有3大类属性：

* 元数据metadata，用来标识API对象，每个对象都至少有3个元数据：namespace，name和uid。

  除此以外还有各种各样的标签labels用来标识和匹配不同的对象，例如用户可以用标签env来标识区分不同的服务部署环境，分别用env=dev、env=testing、env=production来标识开发、测试、生产的不同服务。

* 规范spec

  规范描述了用户期望K8s集群中的分布式系统达到的理想状态（Desired State），例如用户可以通过复制控制器Replication Controller设置期望的Pod副本数为3。

* 状态status

  status描述了系统实际当前达到的状态（Status），例如系统当前实际的Pod副本数为2；那么复制控制器当前的程序逻辑就是自动启动新的Pod，争取达到副本数为3。



K8s中所有的配置都是通过API对象的spec去设置的，也就是用户通过配置系统的理想状态来改变系统，这是k8s重要设计理念之一，即所有的操作都是声明式（Declarative）的而不是命令式（Imperative）的。

声明式操作在分布式系统中的好处是稳定，不怕丢操作或运行多次，例如设置副本数为3的操作运行多次也还是一个结果，而给副本数加1的操作就不是声明式的，运行多次结果就错了。

# kurbernetes 组件

Master 组件

- [kube-apiserver](https://kubernetes.io/docs/admin/kube-apiserver)： 提供了资源操作的唯一入口，并提供认证、授权、访问控制、API注册和发现等机制；
- [ETCD](https://kubernetes.io/docs/admin/etcd)： 默认的存储系统，保存所有集群数据，使用时需要为etcd数据提供备份计划。
- [kube-controller-manager](https://kubernetes.io/docs/admin/kube-controller-manager)： 负责维护集群的状态，比如故障检测、自动扩展、滚动更新等；
  - 节点（Node）控制器。
  - 复制（Replication）控制器RC：负责维护系统中每个副本中的pod。
  - 端点（Endpoints）控制器：填充Endpoints对象（即连接Services＆Pods）。
  - Service Account和Token控制器：为新的Namespace 创建默认帐户访问API Token。
- cloud-controller-manager：负责与底层云提供商的平台交互。云控制器管理器是Kubernetes版本1.6中引入的，目前还是Alpha的功能。
- kube-scheduler： 负责资源的调度，按照预定的调度策略将Pod调度到相应的机器上；
- 插件 addons
  - DNS
  - Ingress Controller为服务提供外网入口
  - [Dashboard](https://kubernetes.io/docs/tasks/access-kubernetes-api/http-proxy-access-api/)提供GUI
  - Heapster 提供资源监控
  - Cluster-level Logging
  - Federation提供跨可用区的集群
  - Fluentd-elasticsearch提供集群日志采集、存储与查询

Node组件

- kubelet：负责维护容器的生命周期，同时也负责Volume（CVI）和网络（CNI）的管理；
- kube-proxy：为Service提供cluster内部的服务发现和负载均衡；



# 概念

1. Pod

   Pod是在K8s集群中运行部署应用或服务的最小单元，支持多容器的。Pod的设计理念是支持多个容器在一个Pod中共享网络地址和文件系统，可以通过进程间通信和文件共享这种简单高效的方式组合完成服务。

2. 复制控制器（Replication Controller，RC）

   RC是K8s集群中最早的保证Pod高可用的API对象。通过监控运行中的Pod来保证集群中运行指定数目的Pod副本。

3. Service

   在K8集群中，客户端需要访问的服务就是Service对象。每个Service会对应一个集群内部有效的虚拟IP，集群内部通过虚拟IP访问一个服务。

   在K8s集群中微服务的负载均衡是由Kube-proxy实现的。Kube-proxy是K8s集群内部的负载均衡器。它是一个分布式代理服务器，在K8s的每个节点上都有一个；这一设计体现了它的伸缩性优势，需要访问服务的节点越多，提供负载均衡能力的Kube-proxy就越多，高可用节点也随之增多。

4. Job

   Job是K8s用来控制批处理型任务的API对象。批处理业务与长期业务的主要区别是批处理业务的运行有头有尾，而长期伺服业务在用户不停止的情况下永远运行。

   Job管理的Pod根据用户的设置把任务成功完成就自动退出了。成功完成的标志根据不同的spec.completions策略而不同：单Pod型任务有一个Pod成功就标志完成；定数成功型任务保证有N个任务全部成功；工作队列型任务根据应用确认的全局成功而标志成功。

5. 后台支撑服务集（DaemonSet）

   后台支撑型服务的核心关注点在K8s集群中的节点（物理机或虚拟机），要保证每个节点上都有一个此类Pod运行。典型的后台支撑型服务包括，存储，日志和监控等在每个节点上支持K8s集群运行的服务。

6. 存储卷（Volumn）

   K8s 集群中的存储卷跟Docker的存储卷有些类似，只不过Docker的存储卷作用范围为一个容器，而K8s的存储卷的生命周期和作用范围是一个Pod。

   **每个Pod中声明的存储卷由Pod中的所有容器共享。**

   K8s还支持使用Persistent Volumn Claim即PVC这种逻辑存储，使用这种存储，使得存储的使用者可以忽略后台的实际存储技术（例如AWS，Google或GlusterFS和Ceph），而将有关存储实际技术的配置交给存储管理员通过Persistent Volumn来配置。

7. 持久存储卷（Persistent Volumn，PV）和持久存储卷声明（Persistent Volumn Claim，PVC）

   PV和PVC使得K8s集群具备了存储的逻辑抽象能力，使得在配置Pod的逻辑里可以忽略对实际后台存储技术的配置，而把这项配置的工作交给PV的配置者，即集群的管理者。存储的PV和PVC的关系，跟计算的Node和Pod的关系是非常类似的；

   PV和Node是资源的提供者，根据集群的基础设施变化而变化，由K8s集群管理员配置；

   而PVC和Pod是资源的使用者，根据业务服务的需求变化而变化，有K8s集群的使用者即服务的管理员来配置

8. 节点（Node）

   K8s集群中的计算能力由Node提供，最初Node称为服务节点Minion，后来改名为Node，是所有Pod运行所在的工作主机，可以是物理机也可以是虚拟机。

9. Secret

   Secret是用来保存和传递密码、密钥、认证凭证这些敏感信息的对象。使用Secret的好处是可以避免把敏感信息明文写在配置文件里。在K8s集群中配置和使用服务不可避免的要用到各种敏感信息实现登录、认证等功能，例如访问AWS存储的用户名密码。为了避免将类似的敏感信息明文写在所有需要使用的配置文件中，可以将这些信息存入一个Secret对象，而在配置文件中通过Secret对象引用这些敏感信息。这种方式的好处包括：意图明确，避免重复，减少暴漏机会。

10. 用户帐户（User Account）和服务帐户（Service Account）

   顾名思义，用户帐户为人提供账户标识，而服务账户为计算机进程和K8s集群中运行的Pod提供账户标识。用户帐户和服务帐户的一个区别是作用范围；用户帐户对应的是人的身份，人的身份与服务的namespace无关，所以用户账户是跨namespace的；而服务帐户对应的是一个运行中程序的身份，与特定namespace是相关的。

11. Namespace

    Namespace 为K8s集群提供虚拟的隔离作用，K8s集群初始有两个Namespace，分别是默认default和系统kube-system，除此以外，管理员可以创建新的名字空间。

12. RBAC模式授权 和 ABAC模式授权

    基于角色的访问控制（Role-based Access Control，RBAC），基于属性的访问控制（Attribute-based Access Control，ABAC）。RBAC主要是引入了角色（Role）和角色绑定（RoleBinding）的抽象概念。

    在ABAC中，K8s集群中的访问策略只能跟用户直接关联；而在RBAC中，访问策略可以跟某个角色关联，具体的用户在跟一个或多个角色相关联。这一新的概念抽象一定会使集群服务管理和使用更容易扩展和重用。



附录：

# 云计算与弹性伸缩

从云计算的定义出发，云计算系统一个基本特性就是计算能力的弹性和伸缩性。弹性和伸缩性的意思是能够根据实际的需要，随时增多或减少计算能力。弹性伸缩针对不同的处理对象，有不同的伸缩模式，如下图，分别处于X、Y、Z轴3个不同维度。

1. 对于同一类事物的无状态服务，数据不需要持久化，处理结果不需要保存在信息系统中，可以通过X轴计算实例的**水平复制**进行伸缩，这就是Kubernetes系统中Replication Controller和ReplicaSet所完成的功能；举例说明，一个电子商务系统的下订单服务，本身只是做订单处理但不存储数据的无状态服务，这个微服务就可以用水平复制的模式来伸缩。

2. 对于同一类事物的有状态服务，数据存储的内容各不相同，身份也各不相同，可以通过Z轴存储实例的**数据分片**进行伸缩，这就是Kuberenetes的PetSet或者StatefulSet所完成的功能；举例说明，下订单服务的订单数据存储在MySQL数据库中，要想增加订单数据存储的能力，就需要对针对订单数据库表进行数据分片。

   > tips: 何为数据分片（segment，fragment， shard， partition），就是按照一定的规则，将数据集划分成相互独立、正交的数据子集，然后将数据子集分布到不同的节点上。注意，数据分片需要按照一定的规则，不同的分布式应用有不同的规则，但都遵循同样的原则：按照最主要、最频繁使用的访问方式来分片。)

3. 对于不同类别事物的处理，则需要通过Y轴的功能分割来进行伸缩，Y轴的分割也称为**垂直分割**；举例说明，一个完整的电子商务系统，可以根据业务功能分割成用户管理、购物车管理、订单管理、产品目录管理、库存管理、支付管理、配送管理等多个独立的微服务。需要说明的是，**功能分割（Y轴）是水平复制（X轴）和数据分片（Z轴）的前提**，只有通过功能分割解耦将不同的事物分割出来，才能针对同一事物进行水平复制或数据分片。

![4bd0da72f12b4c47834228a74a169ca219208981](https://cdn.kelu.org/blog/2018/05/4b.jpg)

### 容器与微服务架构

微服务架构的理念，是将一个完整的应用，按照业务拆分成彼此独立的模块以支撑服务的独立开发、部署和伸缩。

微服务分割也就是业务功能分割。微服务分割和传统软件模块分割的一大区别是，

微服务分割强调领域数据模型的分割，也就是数据存储服务的分割，以保证不同微服务之间没有持久化数据方面的依赖性，从而使得不同的微服务真正可以在运行时独立进行部署和伸缩。

传统的软件模块分割，比较多的是考虑代码的重用性和各模块开发的独立性，但因为并没有为超高用户压力的弹性要求做准备，比较少考虑数据持久化层面的伸缩性。

而如前所述，Y轴业务功能分割是Z轴数据分片的前提，因此要想真正应对超高用户压力的弹性计算要求，进行业务功能分割是第一步。

# 参考资料

* [Kubernetes中文社区 中文文档](http://docs.kubernetes.org.cn/)
* [Kubernetes与云原生应用](http://www.infoq.com/cn/articles/kubernetes-and-cloud-native-applications-part01)
* [Kubernetes Documentation](https://kubernetes.io/)