---
layout: post
title: kubernetes helm chart和 helm repo
category: tech
tags: kubernetes docker
---
![](https://cdn.kelu.org/blog/tags/k8s.jpg)

这篇文章记录如何自建 helm chart 和 helm repo。一些前置的背景知识可以在这里了解：[kubernetes helm 入门](/tech/2018/05/05/k8s-helm-tutorial.html)

# 创建chart 

```
helm create alpine
```

目录结构如下所示，我们主要关注目录中的这三个文件即可：Chart.yaml、values.yaml和NOTES.txt。

```
alpine
├── charts
├── Chart.yaml
├── templates
│   ├── deployment.yaml
│   ├── _helpers.tpl
│   ├── NOTES.txt
│   └── service.yaml
└── values.yaml
```

打开Chart.yaml，填写应用的详细信息

打开并根据需要编辑 values.yaml

# chart校验/打包

对Chart进行校验

```
helm lint alpine
```

对Chart进行打包：

```
helm package alpine --debug

Successfully packaged chart and saved it to: /var/local/k8s/helm/alpine-0.1.0.tgz
[debug] Successfully saved /var/local/k8s/helm/alpine-0.1.0.tgz to /root/.helm/repository/local
```

# 本地repo

使用Helm serve命令启动一个repo server，该server缺省使用’$HELM_HOME/repository/local’目录作为Chart存储，并在8879端口上提供服务。

```
helm serve --address 0.0.0.0:8879 &
```

启动本地repo server后，将其加入Helm的repo列表。

```
helm repo add local http://127.0.0.1:8879
"local" has been added to your repositories
```

现在再查找testapi chart包，就可以找到了。

```
helm search local
helm install --name example local/alpine --set service.type=NodePort
```

# chart中定义依赖

在chart目录中创建一个requirements.yaml文件定义该chart的依赖。

```
$ cat > ./alpine/requirements.yaml <<EOF
dependencies:
- name: mariadb
  version: 0.6.0
  repository: https://kubernetes-charts.storage.googleapis.com
EOF
```

通过helm命令更新和下载cahrt的依赖

```
$ helm dep update ./alpine

Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "local" chart repository
...Successfully got an update from the "monocular" chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈Happy Helming!⎈
Saving 1 charts
Downloading mariadb from repo https://kubernetes-charts.storage.googleapis.com
Deleting outdated charts
```

在次安装运行chart时会把依赖中定义的chart运行起来。

# 自定义chart repository

把每个chart打包的tar文件集中存放到charts目录，使用以下命令生成index.yaml文件。

```
mkdir -p charts
mv alpine-0.1.0.tgz charts/

$ helm serve --repo-path ./charts --address 0.0.0.0:8879 &
```

测试可以看到index.yaml的内容为：

![](https://cdn.kelu.org/blog/2018/05/20180524155509.jpg)

我们可以自定义repo地址：

```
helm serve --repo-path ./charts --address 0.0.0.0:8879 --url http://eur2.kelu.org:8897 &
```

可以发现index.yaml 的 url 地址变了。

