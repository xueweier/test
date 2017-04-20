---
layout: post
title: supervisor 安装与配置
category: tech
tags: linux python
---

![](/assets/img/python.jpg)

使用 laravel 队列服务时，由于PHP本身对内存处理的缺陷，一个长期运行在后台的程序如果出现内存泄露问题，那进程就只有挂掉的份了。

Supervisor是一个进程控制系统，由python编写，它提供了大量的功能来实现对进程的管理。有以下一些特点

* 可以配置同时启动的进程数，而不需要一个个启动
* 可以根据程序的退出码来判断是否需要自动重启
* 可以配置进程初始化的环境，包括目录，用户，umask，关闭进程所需要的信号等等
* 有web界面进行管理

    
# 参考资料
    
* [How To Find BASH Shell Array Length ( number of elements ))](https://www.cyberciti.biz/faq/finding-bash-shell-array-length-elements)    
* [Shell脚本编程30分钟入门](https://github.com/qinjx/30min_guides/blob/master/shell.md)    
* [网络分析shell脚本(实时流量+连接统计)](https://www.centos.bz/2014/06/shell-script-for-network-analysis/)    
