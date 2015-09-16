# lnmp
***

### 写在前面
###### 配置文件 

`/usr/local/nginx/conf/`

修改之后执行 `service nginx restart`使得配置生效

其中在http域内最后的位置加上`include vhost/*.conf;`，可以单独配置vhost文件夹中对应的server配置。


###### LNMP状态管理命令
	LNMP状态管理:  /root/lnmp  {start|stop|reload|restart|kill|status} 
	Nginx状态管理:/etc/init.d/nginx  {start|stop|reload|restart} 
	MySQL状态管理:/etc/init.d/mysql  {start|stop|restart|reload|force-reload|status} 
	Memcached状态管理:/etc/init.d/memcached  {start|stop|restart} 
	PHP-FPM状态管理:/etc/init.d/php-fpm  {start|stop|quit|restart|reload|logrotate}
	PureFTPd状态管理:  /etc/init.d/pureftpd  {start|stop|restart|kill|status} ProFTPd状态管理:  /etc/init.d/proftpd  {start|stop|restart|reload}	如重启LNMP,输入命令:/root/lnmp  restart  即可,单独重启mysql:/etc/init.d/mysql  restart
###### LNMPA状态管理命令
	LNMPA状态管理:  /root/lnmpa  {start|stop|reload|restart|kill|status} 
	Nginx状态管理:/etc/init.d/nginx  {start|stop|reload|restart} 
	MySQL状态管理:/etc/init.d/mysql  {start|stop|restart|reload|force-reload|status} 
	Memcached状态管理:/etc/init.d/memcached  {start|stop|restart}
	PureFTPd状态管理:  /etc/init.d/pureftpd  {start|stop|restart|kill|status}
	ProFTPd状态管理:  /etc/init.d/proftpd  {start|stop|restart|reload} 
	Apache状态管理:/etc/init.d/httpd  {start|stop|restart|graceful|graceful-stop|configtest|status}

### 系统hostname
	echo "kelu.org" > /etc/hostname
	hostname -F /etc/hostname
	
	vi /etc/default/dhcpcd
	添加如下语句：
	#SET_HOSTNAME=‘yes'

### lnmp安装
参考[lnmp.org](http://lnmp.org/install.html)

	wget -c http://soft.vpser.net/lnmp/lnmp1.1-full.tar.gz && tar zxf lnmp1.1-full.tar.gz && cd lnmp1.1-full && ./debian.sh
	/root/vhost.sh

mysql的root密码kelumysqladmin

	# Completed on Mon Dec 15 01:29:22 2014
	===================================== Check install ===================================
	
	Checking...
	Nginx: OK
	MySQL: OK
	PHP: OK
	PHP-FPM: OK
	Install lnmp 1.1 completed! enjoy it.
	=========================================================================
	LNMP V1.1 for Ubuntu Linux Server, Written by Licess 
	=========================================================================
	
	For more information please visit http://www.lnmp.org/
	
	lnmp status manage: /root/lnmp {start|stop|reload|restart|kill|status}
	default mysql root password:kelumysqladmin
	phpinfo : http://yourIP/phpinfo.php
	phpMyAdmin : http://yourIP/phpmyadmin/
	Prober : http://yourIP/p.php
	Add VirtualHost : /root/vhost.sh
	
	The path of some dirs:
	mysql dir:   /usr/local/mysql
	php dir:     /usr/local/php
	nginx dir:   /usr/local/nginx
	web dir :     /home/wwwroot/default
	
	=========================================================================
	=========================================================================
	Manager for LNMP V1.1  ,  Written by Licess 
	=========================================================================
	LNMP is a tool to auto-compile & install Nginx+MySQL+PHP on Linux 
	This script is a tool to Manage status of lnmp 
	For more information please visit http://www.lnmp.org 
	
	Usage: /root/lnmp {start|stop|reload|restart|kill|status}
	=========================================================================
	nginx (pid 10666 10665) is running...
	php-fpm is runing!
	 * MySQL is not running
	Active Internet connections (only servers)
	Proto Recv-Q Send-Q Local Address           Foreign Address         State      
	tcp        0      0 0.0.0.0:1344            0.0.0.0:*               LISTEN     
	tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN     
	tcp        0      0 0.0.0.0:1723            0.0.0.0:*               LISTEN     
	tcp6       0      0 :::1344                 :::*                    LISTEN     

以前在ubuntu 12.04 下安装是没有问题的，现在换成了14.04,无法启动。
mysql的错误日志在  /usr/local/mysql/var/kelu.org.err
### 安装ftp

LNMP一键安装包中有Pureftpd和Proftpd服务器安装脚本。 
我使用pureftp，这个需要有mysql的支持。

	用户名keluftpadmin
	密码：keluftpadmin
	
按照[官方文档](http://lnmp.org/faq/ftpserver.html)

修改端口

	vi /usr/local/pureftpd/pure-ftpd.conf
	
	service pureftpd restart 

注:在目前 diet coda 的1.1版本中，如果修改ftp端口将会导致无法连接ftp。

	
	# PassivePortRange 30000 50000 //主动连接的端口范围

###### ftp常见问题

ftp 用户端是可以连到 ftp 主机并且看到欢迎登入画面，不过后续要浏览档案目录清单与档案抓取时却会发生错误...

ftp 协定本身于 data channnel 还可以区分使用 active mode 与 passive mode 这两种传输模式，而就以 passive mode 来说，最后是协议让 ftp client 连结到 ftp server 本身指定于大于 1024 port 的连接埠传输资料。
 
这样配置在 ftp 传输使用 active 可能正常，但是使用 passive mode 却发生错误，其中原因就是因为该主机firewall 规则配置不允许让 ftp client 连结到 ftp server 指定的连结埠才引发这个问题。

pureftpd的配置文件设置如下：
	
	ChrootEveryone yes //锁定所有用户到家目录中
	
	# TrustedGID 100 //信任组ID100，可以不锁定
	
	MaxClientsNumber 50 //最大的客户端数量
	
	MaxClientsPerIP 8 //同一个IP允许8个链接
	
	DisplayDotFiles no //不显示隐藏文件
	
	AnonymousOnly no //只允许匿名用户
	
	NoAnonymous yes//不允许匿名用户
	
	DontResolve yes //禁止反向解析
	
	MaxIdleTime 10 //最大空闲10分钟
	
	# LDAPConfigFile /etc/pureftpd-ldap.conf //LDAP配置文件目录
	
	# MySQLConfigFile /etc/pureftpd-mysql.conf//MySQL配置文件目录
	
	# PGSQLConfigFile /etc/pureftpd-pgsql.conf //PGSQL配置文件目录
	
	PureDB /usr/local/pureftpd/etc/pureftpd.pdb //虚拟用户数据库
	
	# UnixAuthentication yes //主机认证
	
	LimitRecursion 2000 8 //别表最大显示2000个文件，最深8个目录
	
	AnonymousCanCreateDirs no //是否允许匿名用户创建目录
	
	#MaxLoad 4 //最多可下载的数量
	
	# PassivePortRange 30000 50000 //主动连接的端口范围
	
	ForcePassiveIP 192.168.0.1 //这个地址总是直到匿名目录
	
	# AnonymousRatio 1 10 //匿名用户上传下载速度比率
	
	# UserRatio 1 10 //用户上传下载速度比率
	
	# Bind 127.0.0.1,21 //绑定IP和端口
	
	# AnonymousBandwidth 8 //匿名用户带宽8KB
	
	# UserBandwidth 8 //用户带宽8KB
	
	Umask 133:022 //文件和目录的umask
	
	MinUID 1000 //用户ID至少要大于1000才能登陆
	
	AllowUserFXP no //是否允许用户使用FXP协议登陆
	
	AllowAnonymousFXP no //是否允许匿名用户使用FXP协议
	
	ProhibitDotFilesWrite no //是否允许写入点文件
	
	ProhibitDotFilesRead no //是否允许读取点文件
	
	AnonymousCantUpload yes //不允许匿名用户上传
	
	#NoChmod yes //不允许用户改变权限
	
	#KeepAllFiles yes //允许用户断点续传
	
	#Quota 1000:10//磁盘配额
	
	#MaxDiskUsage 99 //磁盘的最大利用率
	
	#NoRename yes //不允许自动重命名
	
	IPV4Only yes //只允许使用IPV4协议
在上面的iptables规则下，只能使用主动模式连接。
	
如何允许使用被动模式呢？
方法一：使用nf_conntrack_ftp模块（这个Ubuntu似乎不成功）
方法二：配置文件中使用如下两个选项强制将被动模式时使用的端口号限定在一个范围，然后在iptables上运行对这个范围内端口的访问
于是我们应该设置配置文件如下

	PassivePortRange 49100 49600 
	
然后再在iptables文件下设置

	-A INPUT -p tcp --dport 49100:49600 -j ACCEPT
