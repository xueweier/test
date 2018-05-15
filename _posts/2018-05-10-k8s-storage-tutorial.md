---
layout: post
title: kubernetes storage入门
category: tech
tags: kubernetes docker
---
![](https://cdn.kelu.org/blog/tags/k8s.jpg)

这篇文章记录如何使用 kubernetes 的存储资源，包括volume、pv、pvc等。



系列文章：

- [kubernetes 简介](/tech/2018/05/05/k8s-tutorial.html)
- [kubernetes 安装入门](/tech/2018/05/02/kubernetes-install-tutorial.html)
- [kubernetes storage 入门](/tech/2018/05/10/kubernetes-storage-tutorial.html)
- [kubernetes helm 入门](/tech/2018/05/09/k8s-helm-tutorial.html)



## 三个概念： pv ，storageclass， pvc

- pv － 持久化卷， 支持本地存储和网络存储， 例如hostpath，ceph rbd， nfs等，只支持两个属性， capacity和accessModes。其中capacity只支持size的定义，不支持iops等参数的设定，accessModes有三种
  - ReadWriteOnce（被单个node读写）
  - ReadOnlyMany（被多个nodes读）
  - ReadWriteMany（被多个nodes读写）
- storageclass－另外一种提供存储资源的方式， 提供更多的层级选型， 如iops等参数。 但是具体的参数与提供方是绑定的。 如aws和gce它们提供的storageclass的参数可选项是有不同的。
- pvc － 对pv或者storageclass资源的请求， pvc 对 pv 类比于pod 对不同的cpu， mem的请求。

# 环境配置



# 示例

## 安装nfs

查看[关联文章](/tech/2018/05/06/nfs-install.html)

## PV

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mypv1
spec:
  capacity:
    storage: 20Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Recycle
  storageClassName: nfs
  mountOptions:
    - hard
    - nfsvers=4
  nfs:
    path: /
    server: 172.10.1.100
    
kubectl apply -f nfs-pv1.yml    
```

## mysql

```
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  ports:
  - port: 3306
  selector:
    app: mysql
  clusterIP: None
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 20Gi
  storageClassName: nfs
---
apiVersion: apps/v1beta2
kind: Deployment
metadata:
  name: mysql
spec:
  selector:
    matchLabels:
      app: mysql
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - image: mysql:5.6
        name: mysql
        env:
          # Use secret in real usage
        - name: MYSQL_ROOT_PASSWORD
          value: password
        ports:
        - containerPort: 3306
          name: mysql
        volumeMounts:
        - name: mysql-persistent-storage
          mountPath: /var/lib/mysql
      volumes:
      - name: mysql-persistent-storage
        persistentVolumeClaim:
          claimName: mysql-pv-claim
  
kubectl apply -f mysql-deployment.yml
```

检查PersistentVolumeClaim的信息

```
kubectl describe pvc mysql-pv-claim
```

进入MySQL实例

```
kubectl run -it --rm --image=mysql:5.6 --restart=Never mysql-client -- mysql -h mysql -ppassword
```

测试

```
mysql> create database test;
mysql> show databases;
```

此时可以在nfs的共享目录中发现相应的文件。

# 参考资料

* [在kubernetes中运行单节点有状态MySQL应用](https://scotch.io/@ykfq/kubernetesmysql)
* [k8s学习笔记之持久化存储](https://zhuanlan.zhihu.com/p/29706309)
* [Kubernetes之存储调研整理](https://yucs.github.io/2017/12/14/2017-12-14-kubernetes_volume/)
* [Kubernetes中的存储 - IBM](https://www.ibm.com/developerworks/community/wikis/form/anonymous/api/wiki/0be57f75-2769-40b2-9ea4-99fbec0f9073/page/5145e8c8-4833-4e5b-b623-d73d579c562e/attachment/85e05b23-0fb5-4068-992b-65c8110c58f5/media/Kubernetes%E7%AC%AC%E5%85%AD%E8%AE%B2.pdf)