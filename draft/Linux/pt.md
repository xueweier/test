
几分钟搭建 Linux vps挂PT环境

Linux下有很多这类软件，本文以Debian 6安装transmission为例

1.安装
apt-get update apt-get install transmission-daemon
1
2
        
apt-get update
apt-get install transmission-daemon

2.修改配置文件
/etc/init.d/transmission-daemon stop #修改配置文件前一定要先停掉 vi /var/lib/transmission-daemon/info/settings.json #修改配置文件
1
2
        
/etc/init.d/transmission-daemon stop   #修改配置文件前一定要先停掉
vi /var/lib/transmission-daemon/info/settings.json    #修改配置文件

需要修改的几个地方
“dht-enabled”: false,
玩pt的，DHT肯定是关闭的，这也是主流PT的要求

“download-dir”: “/var/lib/transmission-daemon/downloads”,
自己定义一个下载路径，注意设置下载路径的权限

“rpc-password”: “password”,
“rpc-username”: “download”,
定义web访问的用户名和密码

“rpc-port”: 9091,
定义web访问的端口，建议改一个，安全点

“rpc-whitelist-enabled”: false,
如果你的访问ip不是很固定，建议这项取消，否则web访问会受限制

“peer-port”: 51515,
这项可选，主要是针对你的网络是否对pt软件有限制端口的行为，如果有限制可以改个端口

3.启动
/etc/init.d/transmission-daemon start
1
        
/etc/init.d/transmission-daemon start

访问http://你的ip:端口即可管理。
