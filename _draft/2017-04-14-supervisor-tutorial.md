---
layout: post
title: supervisor 安装与配置
category: tech
tags: linux python
---

![](/assets/img/python.jpg)

Supervisor是一个进程控制系统，由python编写。可以很方便的用来启动、重启、关闭进程（不仅仅是 Python 进程）。 它有以下一些特点

* 可以配置同时启动的进程数，而不需要一个个启动
* 可以根据程序的退出码来判断是否需要自动重启
* 可以配置进程初始化的环境，包括目录，用户，umask，关闭进程所需要的信号等等
* 有web界面进行管理

下面介绍一下 supervisor 的安装使用过程。

# 安装

使用python包管理工具pip进行安装：

    pip install supervisor

# 配置

安装完 supervisor 之后，运行echo_supervisord_conf 命令输出默认的配置项，重定向到一个配置文件里：

    echo_supervisord_conf > /etc/supervisord.conf

下面是我经过简化过的配置：

    [unix_http_server]
    file=/tmp/supervisor.sock   ; (the path to the socket file)

    [supervisord]
    logfile=/var/local/log/supervisor/supervisord.log ; (main log file;default $CWD/supervisord.log)
    logfile_maxbytes=50MB        ; (max main logfile bytes b4 rotation;default 50MB)
    logfile_backups=10           ; (num of main logfile rotation backups;default 10)
    loglevel=info                ; (log level;default info; others: debug,warn,trace)
    pidfile=/tmp/supervisord.pid ; (supervisord pidfile;default supervisord.pid)
    nodaemon=false               ; (start in foreground if true;default false)
    minfds=1024                  ; (min. avail startup file descriptors;default 1024)
    minprocs=200                 ; (min. avail process descriptors;default 200)
    
    [rpcinterface:supervisor]
    supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

    [supervisorctl]
    serverurl=unix:///tmp/supervisor.sock ; use a unix:// URL  for a unix socket

    [include]
    files = /etc/supervisor/*.conf

然后新建文件夹来存放各种进程的配置文件：

    mkdir /etc/supervisor

目前我是用来管理 laravel 的日志，在 `/etc/supervisor/` 里添加文件 `laravel-wechat.conf`

    














使用 laravel 队列服务时，由于PHP本身对内存处理的缺陷，一个长期运行在后台的程序如果出现内存泄露问题，那进程就只有挂掉的份了。
    
# 参考资料
    
* [使用Supervisor来管理你的Laravel队列](http://yansu.org/2014/03/22/managing-your-larrvel-queue-by-supervisor.html)    
