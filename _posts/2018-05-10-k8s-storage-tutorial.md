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
  
kubectl apply -f nfs-pvc1.yml
```

未完待续