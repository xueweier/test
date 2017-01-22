# 在Ubuntu12.04上安装l2tp/ipsec VPN服务器
***

记录我在Ubuntu服务器上安装l2tp/ipsec VPN的过程，以供日后查询。ipsec用于验证和加密数据包，由openswan提供；l2tp即第二层隧道协议，由xl2tpd提供。

### 安装pptpd
	apt-get install openswan xl2tpd
	
### 配置ipsec

注意三件事

* 	将YOUR_SERVER_IP_ADDRESS改为你的服务器的ip地址。
* 	将YOUR_IPSEC_SHARED_KEY改为你的ipsec共享密钥。
* 	注意配置文件的缩进。


`/etc/ipsec.conf`

	sudo cat >/etc/ipsec.conf<<EOF
	version 2.0
	
	config setup
	    nat_traversal=yes
	    virtual_private=%v4:10.0.0.0/8,%v4:192.168.0.0/16,%v4:172.16.0.0/12
	    oe=off
	    protostack=netkey
	
	conn L2TP-PSK-NAT
	    rightsubnet=vhost:%priv
	    also=L2TP-PSK-noNAT
	
	conn L2TP-PSK-noNAT
	    authby=secret
	    pfs=no
	    auto=add
	    keyingtries=3
	    rekey=no
	    ikelifetime=8h
	    keylife=1h
	    type=transport
	    left=YOUR_SERVER_IP_ADDRESS
	    leftprotoport=17/1701
	    right=%any
	    rightprotoport=17/%any
	EOF

`/etc/ipsec.secrets`

	cat >/etc/ipsec.secrets<<EOF
	YOUR_SERVER_IP_ADDRESS %any: PSK "YOUR_IPSEC_SHARED_KEY"
	EOF

重启并检查ipsec配置

	service ipsec restart
	ipsec verify

输出没有FAILED项即可，WARNING可以不管。

### 配置xl2tpd

`/etc/xl2tpd/xl2tpd.conf`

	cat >/etc/xl2tpd/xl2tpd.conf<<EOF
	[global]
	ipsec saref = yes
	
	[lns default]
	local ip = 10.10.11.1
	ip range = 10.10.11.2-10.10.11.245
	refuse chap = yes
	refuse pap = yes
	require authentication = yes
	pppoptfile = /etc/ppp/xl2tpd-options
	length bit = yes
	EOF

`/etc/ppp/xl2tpd-options`

	cat >/etc/ppp/xl2tpd-options<<EOF
	require-mschap-v2
	ms-dns 8.8.8.8
	ms-dns 8.8.4.4
	asyncmap 0
	auth
	crtscts
	lock
	hide-password
	modem
	name l2tpd
	proxyarp
	lcp-echo-interval 30
	lcp-echo-failure 4
	EOF

### 添加ppp用户和密码

将USER和PASSWORD改为你的用户名和密码即可。

	cat >>/etc/ppp/chap-secrets<<EOF
	USER * PASSWORD *
	EOF

### 配置数据包转发

	for each in /proc/sys/net/ipv4/conf/*
	do
	    echo 0 > $each/accept_redirects
	    echo 0 > $each/send_redirects
	done

	sed -i 's/#net.ipv4.ip_forward=1/net.ipv4.ip_forward=1/g' /etc/sysctl.conf
	sysctl -p

### 配置iptables规则

	iptables -t nat -A POSTROUTING -j MASQUERADE
	iptables -I FORWARD -p tcp --syn -i ppp+ -j TCPMSS --set-mss 1356

### 重启xl2tpd服务器

	service xl2tpd restart
	
	
### 设置开机启动

修改/etc/rc.local
如果/etc/rc.local无法正常自动执行，尝试将换成#!/bin/bash。

	#!/bin/bash
	
	# for xl2tpd
	for each in /proc/sys/net/ipv4/conf/*
	do
	    echo 0 > $each/accept_redirects
	    echo 0 > $each/send_redirects
	done
	
	
	iptables -t nat -A POSTROUTING -j MASQUERADE
	iptables -I FORWARD -p tcp --syn -i ppp+ -j TCPMSS --set-mss 1356
	
	exit 0
