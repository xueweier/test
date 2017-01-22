# PPTP
***
### 安装pptpd
	apt-get install pptpd
	
### 配置	
	vi /etc/pptpd.conf

	#remoteip 192.168.0.234-238,192.168.0.245
	# or
	#localip 192.168.0.234-238,192.168.0.245

写上自己要分配的ip段。remoteip是给客户端用的，localip是给主机用的。记得找个不容易冲突的段，以下为例

	localip 192.168.200.1
	remoteip 192.168.200.200-238,192.168.200.245	
添加PPTP VPN用户，编辑/etc/ppp/chap-secrets。内容一目了然，\*代表随机分配ip，pptpd字段也可改为\*表示自动。账户格式如下

	username pptpd password *
	或者
	username * password *

修改DNS服务器，一般Google的服务器还是很靠谱的。要换成其他的也OK，比如opendns的。这个地方我要强调一下，必须修改！

	vi /etc/ppp/pptpd-options
	
	ms-dns 8.8.8.8
	ms-dns 8.8.4.4

修改内核设置，使其支持转发

	vi /etc/sysctl.conf
	
	net.ipv4.ip_forward=1 //去掉注释
	
执行以下命令使修改后的内核生效

	sysctl -p
	
应该能看到

	net.ipv4.ip_forward=1
	
如果打开pptpd的debug功能的话请在/etc/pptpd.conf中去掉debug的注释

重启pptpd

	service pptpd restart
	
	
### iptables
	
接下来就是让我头痛了很久的东西…你要想自己的VPN能够正常使用就得让你的防火墙把数据包都放过去并设置转发，转发规则比较简单。以下两条命令分别对应OpenVZ架构和XEN架构的VPS。
适合于OpenVZ架构的VPS,假设12.34.56.78为您VPS的公网IP地址

	iptables -t nat -A POSTROUTING -s 192.168.200.0/24 -j SNAT –to-source 12.34.56.78 //OpenVZ架构
	iptables -t nat -A POSTROUTING -s 192.168.200.0/24 -o eth0 -j MASQUERADE //XEN架构

如果你设置了自定义规则，还得使防火墙允许pptp的服务。pptp要使用1723的tcp端口以及gre协议，因此要加上这两条规则

	-A INPUT -p tcp -m tcp –dport 1723 -j ACCEPT
	-A INPUT -p gre -j ACCEPT
	
另外要使数据能够通过防火墙应该加上

	-A INPUT -i eth0 -s 192.168.200.0/24 -j ACCEPT
	
请注意这里的eth0…一定要ifconfig下改成你的外网适配器…本人就是傻傻的按照攻略来配置，结果后来发现自己的适配器根本不是叫eth0而是venet0…
使防火墙支持数据的转发（如果嫌麻烦可以不拒绝防火墙的转发规则）

	-A FORWARD -i eth0 -d 192.168.200.0/24 -j ACCEPT
	-A FORWARD -o eth0 -s 192.168.200.0/24 -j ACCEPT
	
一条是从外网到主机，另一条是从主机到客户端。当然这只是针对我个人的情况，以上规则应该就可以了。如果还有问题的话可以去日志里查看哪些规则被拒绝了再添加相应的例外，所以说前面一定要在iptables里面加上log

### /etc/pptpd.conf

	option /etc/ppp/pptpd-options
	logwtmp
	localip 192.168.200.1
	remoteip 192.168.200.234-238,192.168.200.245
	
### /etc/ppp/pptpd-options

	name pptpd
	refuse-pap
	refuse-chap
	refuse-mschap
	require-mschap-v2
	require-mppe-128
	
	ms-dns 8.8.8.8
	ms-dns 8.8.4.4
	
	proxyarp
	nodefaultroute
	lock
	nobsdcomp 
