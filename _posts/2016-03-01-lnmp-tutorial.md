---
layout: post
title: 军哥的lnmp一键安装包
category: tech
tags: linux nginx mysql php
---

我自己部署的话，一般用的是debian+openresty+postgres+php。虽然如此，为别人快速安装一键部署环境的时候，lnmp依然是最简便易行的选择。

基本的操作在官网上都有，[lnmp一键安装包](http://lnmp.org/)。今天仅仅记录一些容易忘记的操作，避免每次都上官网去找。

我使用的版本是1.2，发现mysql和pureftp会有些问题，因为网站已经开放使用了，也就懒得换了。


## 安装

按照官网的提示，安装使用下面的命令，再输入些相关的信息，即可安装完成。

	wget -c http://soft.vpser.net/lnmp/lnmp1.2-full.tar.gz && tar zxf lnmp1.2-full.tar.gz && cd lnmp1.2-full && ./install.sh lnmp

## 常用命令

LNMP 1.2输入lnmp命令即可看到常用命令。

LNMP 1.1及之前的版本采用/root/vhost.sh 进行添加虚拟主机。 

	Usage: lnmp {start|stop|reload|restart|kill|status}
	Usage: lnmp {nginx|mysql|mariadb|php-fpm|pureftpd} {start|stop|reload|restart|kill|status}
	Usage: lnmp vhost {add|list|del}
	Usage: lnmp database {add|list|del}
	Usage: lnmp ftp {add|list|del}


## 虚拟主机管理

直接使用lnmp命令即可

	lnmp vhost add
	lnmp vhost list
	lnmp vhost del

虚拟主机配置文件在：

	/usr/local/nginx/conf/vhost/域名.conf

执行：/etc/init.d/nginx restart 重启生效

上传网站后执行：chown www:www -R /path/to/dir 对网站目录进行权限设置

[链接](http://lnmp.org/install.html)

## 插件

FTP服务器： PureFTPd，执行：./pureftpd.sh 安装，http://yourIP/ftp/ 进行管理。

缓存加速： LNMP1.2下统一使用./addons.sh 进行安装和卸载。 

使用方法：./addons.sh {install|uninstall} {eaccelerator|xcache|memcached|opcache|redis|imagemagick|ioncube} 

## 安装目录

LNMP相关软件安装目录

    Nginx 目录: /usr/local/nginx/
    MySQL 目录 : /usr/local/mysql/
    MySQL数据库所在目录：/usr/local/mysql/var/
    MariaDB 目录 : /usr/local/mariadb/
    MariaDB数据库所在目录：/usr/local/mariadb/var/
    PHP目录 : /usr/local/php/
    PHPMyAdmin目录 : 0.9版本为/home/wwwroot/phpmyadmin/ 1.0及以后版本为 /home/wwwroot/default/phpmyadmin/ 强烈建议将此目录重命名为其不容易猜到的名字。phpmyadmin可自己从官网下载新版替换。
    默认网站目录 : 0.9版本为 /home/wwwroot/ 1.0及以后版本为 /home/wwwroot/default/
    Nginx日志目录：/home/wwwlogs/
    /root/vhost.sh添加的虚拟主机配置文件所在目录：/usr/local/nginx/conf/vhost/
    PureFtpd 目录：/usr/local/pureftpd/
    PureFtpd web管理目录： 0.9版为/home/wwwroot/default/ftp/ 1.0版为 /home/wwwroot/default/ftp/
    Proftpd 目录：/usr/local/proftpd/
    Redis 目录：/usr/local/redis/
    
LNMP相关配置文件位置

    Nginx主配置文件：/usr/local/nginx/conf/nginx.conf
    /root/vhost.sh添加的虚拟主机配置文件：/usr/local/nginx/conf/vhost/域名.conf
    MySQL配置文件：/etc/my.cnf
    PHP配置文件：/usr/local/php/etc/php.ini
    php-fpm配置文件：/usr/local/php/etc/php-fpm.conf
    PureFtpd配置文件：/usr/local/pureftpd/pure-ftpd.conf
    PureFtpd MySQL配置文件：/usr/local/pureftpd/pureftpd-mysql.conf
    Proftpd配置文件：/usr/local/proftpd/etc/proftpd.conf 1.2及之前版本为/usr/local/proftpd/proftpd.conf
    Proftpd 用户配置文件：/usr/local/proftpd/etc/vhost/用户名.conf
    Redis 配置文件：/usr/local/redis/etc/redis.conf
   


---

转载自：

* [lnmp.org](http://lnmp.org/)