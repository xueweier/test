---
layout: post
title: docker-gitlab 的数据迁移
category: tech
tags: docker gitlab
---
![](https://cdn.kelu.org/blog/tags/gitlab.jpg)



这篇文章介绍下如何将容器化的 gitlab 进行数据迁移。

# gitlab是什么

GitLab是利用Ruby on Rails一个开源的版本管理系统，实现一个自托管的Git项目仓库，可通过Web界面进行访问公开的或者私人项目。

# gitlab迁移背景

目前个人环境上跑了一个单机版的 gitlab， 运行命令为：

```
sudo docker run -d \
   --env GITLAB_OMNIBUS_CONFIG="external_url 'http://xx:8181'" \ 
   --publish 8181:8181 \ 
   --name gitlab \ 
   --restart always \ 
   --volume /app/gitlab/config:/etc/gitlab \ 
   --volume /app/gitlab/logs:/var/log/gitlab \ 
   --volume /app/gitlab/data:/var/opt/gitlab \ 
gitlab/gitlab-ce:9.5.1-ce.0
```

目前我们需要做的就是将环境配置和数据内容还原到新的环境中去。



# 迁移过程

1. 备份数据库和配置文件，将数据文件与配置文件传输到新机器上。

   ```
   docker exec -t <your container name> gitlab-rake gitlab:backup:create
   ```

   备份结束后，在宿主机的  `/app/allblue/gitlab/data/backups` 目录下将得到形如`1493107454_2017_04_25_9.1.0_gitlab_backup.tar`的文件。

   打包 /app/gitlab/config 文件夹。

2. 新机器上运行相同的 run 命令

   ```
   docker stop gitlab # 停止容器
   mv 1493107454_2017_04_25_9.1.0_gitlab_backup.tar  /app/gitlab/data/backups # 用旧的数据文件和配置文件替换新的。
   mv config /app/gitlab/config # 用旧的数据文件和配置文件替换新的。
   ```

3. 重新运行容器，进入容器后断开 gitlab 与 数据库的连接

   ```
   # 进入容器
   docker exec -it <your container name> /bin/bash
   gitlab-ctl stop unicorn
   gitlab-ctl stop sidekiq

   # 验证
   gitlab-ctl status
   ```

   还原备份:

   ```
   # 重建数据库
   sudo gitlab-rake gitlab:backup:restore BACKUP=1493107454_2017_04_25_9.1.0
   ```

   重启 gitlab 并验证：

   ```
   sudo gitlab-ctl restart
   sudo gitlab-rake gitlab:check SANITIZE=true
   ```

4. 大功告成。



# 参考资料

* [GitLab的安装及使用教程](https://yq.aliyun.com/articles/74395)
* [docs.gitlab.com](https://docs.gitlab.com/omnibus/settings/backups.html#backup-and-restore-omnibus-gitlab-configuration)

