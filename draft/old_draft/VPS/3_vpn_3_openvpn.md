 # openvpn for Ubuntu
***

### 服务器端配置

###### 卸载老版本
  如果以前安装过，希望卸载，那么

		# apt-get --purge remove openvpn

  在卸载的时候出现警告 

	/sbin/ldconfig.real: /usr/lib/libmysqlclient.so.16 is not a symbolic link
	/sbin/ldconfig.real: /usr/lib/libmysqlclient_r.so.16 is not a symbolic link

  提示两个so文件不是符号链接，这时再用`ldconfig -v`来查看两个文件链接的目标，分析之后重新链接。注意位置就行。lnmp安装包下的执行如下操作即可。。

	cd /root/lnmp1.1-full/
	ln -sf /usr/local/mysql/lib/mysql/libmysqlclient_r.so.16 /usr/lib/libmysqlclient_r.so.16
	ln -sf /usr/local/mysql/lib/mysql/libmysqlclient_so.16 /usr/lib/libmysqlclient.so.16
	
###### 安装

		apt-get install openvpn
		
`/sbin/ldconfig.real: /usr/lib/libmysqlclient.so.16 is not a symbolic link`
`/sbin/ldconfig.real: /usr/lib/libmysqlclient_r.so.16 is not a symbolic link`

	ln -sf /usr/local/mysql/lib/mysql/libmysqlclient_r.so.16 /usr/lib/libmysqlclient_r.so.16
	ln -sf /usr/local/mysql/lib/mysql/libmysqlclient_so.16 /usr/lib/libmysqlclient.so.16
	
###### 初始化服务端  
  将OpenVPN文件拷贝到/etc/openvpn/目录下

	cp -r /usr/share/doc/openvpn/examples/easy-rsa/2.0/* /etc/openvpn/
	gzip -d /usr/share/doc/openvpn/examples/sample-config-files/server.conf.gz
	cp /usr/share/doc/openvpn/examples/sample-config-files/server.conf /etc/openvpn/

###### 配置PKI

	cd /etc/openvpn/
	vim vars

找到“export KEY_SIZE=”这行，根据情况把1024改成2048或者4096
再定位到最后面，会看到类似下面这样的

	export KEY_COUNTRY=”US”
	export KEY_PROVINCE=”CA”
	export KEY_CITY=”SanFrancisco”
	export KEY_ORG=”Fort-Funston”
	export KEY_EMAIL=”me@myhost.mydomain“

这个自己根据情况改一下，不改也可以运行。其实不改vars这个文件，vpn也可以跑起来。

	export KEY_COUNTRY=”CN”
	export KEY_PROVINCE=”Guangdong”
	export KEY_CITY=”Shenzhen”
	export KEY_ORG=”kelu.org”
	export KEY_EMAIL=”admin@kelu.org“

注：在后面生成服务端ca证书时，这里的配置会作为缺省配置
做SSL配置文件软链

	ln -s openssl-1.0.0.cnf openssl.cnf
	
修改vars文件可执行并调用

	chmod +x vars
	
###### 	产生CA证书

	source ./vars
	// 得到提示：
	NOTE: If you run ./clean-all, I will be doing a rm -rf on /etc/openvpn/keys

清空原有证书

	./clean-all
	// 注：下面这个命令在第一次安装时可以运行，以后在添加完客户端后慎用，因为这个命令会清除所有已经生成的证书密钥，和上面的提示对应
	
生成服务器端ca证书

	./build-ca
	// 注：由于之前做过缺省配置，这里一路回车即可
	
###### 产生服务器证书

	./build-key-server openvpn.kelu.org
生成服务器端密钥证书, 后面这个openvpn.example.com就是服务器名，也可以自定义，可以随便起，但要记住，后面要用到。	产生成功后将显示：

	Certificate is to be certified until Nov 24 11:43:48 2024 GMT (3650 days)
	Sign the certificate? [y/n]:y
	
	1 out of 1 certificate requests certified, commit? [y/n]y
	Write out database with 1 new entries
	Data Base Updated

###### 生成DH验证文件

	./build-dh
生成diffie hellman参数，用于增强openvpn安全性（生成需要漫长等待），让服务器飞一会。

###### 生成客户端证书

	./build-key iMac
	（名字任意，建议写成你要发给的人的姓名，方便管理）
注：这里与生成服务端证书配置类似，中间一步提示输入服务端密码，其他按照缺省提示一路回车即可。

###### 生成ta.key文件

	openvpn --genkey --secret /etc/openvpn/keys/ta.key

###### 编辑服务配置文件

	vim /etc/openvpn/server.conf
	local 173.230.135.109
	port 21344
	;proto tcp
	proto udp
	;dev tap
	dev tun
	;dev-node MyTap
	ca /etc/openvpn/keys/ca.crt
	cert /etc/openvpn/keys/openvpn.kelu.org.crt
	key /etc/openvpn/keys/openvpn.kelu.org.key  # This file should be kept secret
	dh /etc/openvpn/keys/dh1024.pem
	server 10.8.0.0 255.255.255.0
	ifconfig-pool-persist ipp.txt
	push "redirect-gateway def1 bypass-dhcp"
	push "dhcp-option DNS 8.8.8.8"
	push "dhcp-option DNS 8.8.4.4"
	;client-to-client
	keepalive 10 120
	comp-lzo
	max-clients 20
	persist-key
	persist-tun
	status /var/log/openvpn-status.log
	log	    /var/log/openvpn.log
	log-append  /var/log/openvpn.log
	verb 3
	mute 20
	
	
	;server-bridge 10.8.0.4 255.255.255.0 10.8.0.50 10.8.0.100
	;server-bridge
	;push "route 192.168.10.0 255.255.255.0"
	;push "route 192.168.20.0 255.255.255.0"
	;client-config-dir ccd
	;route 192.168.40.128 255.255.255.248
	;client-config-dir ccd
	;route 10.9.0.0 255.255.255.252
	;learn-address ./script
	;duplicate-cn
	;tls-auth ta.key 0 # This file is secret
	;cipher BF-CBC        # Blowfish (default)
	;cipher AES-128-CBC   # AES
	;cipher DES-EDE3-CBC  # Triple-DES

###### 配置iptables
设置iptables（这一条至关重要，通过配置nat将vpn网段IP转发到server内网）

	iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o eth0 -j MASQUERADE
设置openvpn端口通过：

	iptables -A INPUT -p TCP --dport 1194 -j ACCEPT
	iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
更多相关iptables的设置就不写了。

###### 启动服务
	
	service openvpn start
	
配置开机启动
将openvpn配置开机启动

	vim /etc/rc.local
将`/etc/init.d/openvpn start`加入到`/etc/rc.local`文件中

设置外网访问

	vim /etc/sysctl.conf
	// 找到net.ipv4.ip_forward = 0
	// 把0改成1
	
	sysctl -p
	
### 客户端配置

Mac下载 `Tunnelblick`
将服务器上的`ca.crt`、`client.crt`、`client.key`、`ta.key`下载到本地。`Tunnelblick`会在本地自动生成一个`config.ovpn`。
	
	client
	;dev tap
	dev tun
	;dev-node MyTap
	;proto tcp
	proto udp
	remote 173.230.135.109 21344
	;remote my-server-2 1194
	;remote-random
	resolv-retry infinite #断线自动重新连接，在网络不稳定的情况下(例如：笔记本电脑无线网络)非常有用。
	nobind
	user nobody
	group nobody
	persist-key
	persist-tun
	;http-proxy-retry # retry on connection failures
	;http-proxy [proxy server] [proxy port #]
	;mute-replay-warnings
	ca ca.crt
	cert iMac.crt
	key iMac.key
	;ns-cert-type server
	;tls-auth ta.key 1
	;cipher x
	comp-lzo
	verb 3
	;mute 20
	

	tls-auth ta.key 1     #如果服务器设置了防御DoS等攻击的ta.key，则必须每个客户端开启；如果未设置，则注释掉这一行；
###### 最后的结果是，效果很好！	

###### 遇到过的问题

* Replay-window backtrack occurred
	![Replay-window](http://r.loli.io/mIVF32.png)
	Sometimes network congestion and latency cause the UDP protocol, most commonly used with OpenVPN, to drop packets and even lose the connection. You will see a 'Replay window backtrack occurred' error in the log if this is occurring. One solution is to switch to the TCP protocol, assuming your server is configured to support a TCP connection.
	
	
	
###### 未遇到过的问题	
Troubleshooting OpenVPN - Tips for connecting with the OpenVPN client
This article is directed for users of OpenVPN. It is for troubleshooting client connection issues. It assumes that the OpenVPN server you are connecting to is fully functional and properly configured. This article is for users of the Windows version of the OpenVPN client.

Error: "OpenVPN connects but I cannot reach the internet"
Your log file will show the following error: 'Initialization sequence completed with errors'. You can view the log file by right clicking on the OpenVPN-GUI icon in the system tray and selecting 'view log'.

This error is most commonly due to the Windows (or other) firewall blocking the TAP adapter. If you are using a different firewall than Windows, check your vendor's manual on how to allow programs to pass through. Sometimes there will be a button that lists programs that are allowed. Find OpenVPN and allow access.

If using Windows firewall, go to the 'Control Panel' and select 'Firewall' and then the 'Advanced' tab. Select the connection that corresponds to your TAP adapter. Uncheck the box next to it and select 'OK'. Reboot your computer to ensure the setting takes effect.

Error: "Bandwidth speed seems to be slow or even drop"
1) The OpenVPN default installation in Windows is set to 'Normal Priority'. For the average user, this is probably reasonable. In some cases, on computers that have a lot of other processing going on, the processor might become highly loaded. This shares less processing time to the VPN, slowing things down. This can be fixed by setting the priority of the application to 'High'. This setting is adjusted in the Windows registry. Before performing any registry adjustments it is wise to make a backup just in case.

--Select 'Start' and then 'Run' and then type 'regedit' and hit enter

--Navigate to: 'HKEY_LOCAL_MACHINE\SOFTWARE'

--Click on 'OpenVPN' and then click 'priority'

--Select 'HIGH_PRIORITY_CLASS'

--Find 'OpenVPN-GUI' and repeat

--Close regedit and you are set

2) A good deal of performance can be gained by optimizing Windows TCP in Windows XP. Windows Vista is fairly well managed and does not respond as well to any tweaks. The easiest way to accomplish this is by using a TCP optimizing program. We've used one from speedguide.net that works well and eliminates the need to make manual registry settings. It also will back up your settings before making changes so you can always go back to where you started. We've found that these tweaks in Windows XP have resulted in much faster web surfing in many but not all cases.

Error: "TLS Error: TLS key negotiation failed to occur within 60 seconds"
This error will show on your login screen and in the OpenVPN log. The most common cause of this error is no connectivity to the internet. Make sure you can reach the internet without the VPN. In some cases, your network provider may be blocking the UDP protocol or various ports. Depending on how the server you are attached to is configured, you might try alternate ports or the TCP protocol. In most cases, some combination of ports and TCP resolves the blockage.

Error: "I connect but my IP address has not changed or the TAP Driver fails to load."
This is a Windows Vista or WIndows 7issue that is easily resolved. OpenVPN must be installed by right clicking on the program and selecting the 'Run as administrator' option. Otherwise, the TAP driver will not load properly. Once installed, the program has to be launched in the same way or you will connect to the OpenVPN server but Vista will not allow OpenVPN to set the route and the VPN will fail.

Error: "The OpenVPN-GUI icon drops off from the system tray."
This is a Windows issue where the icon does not show but OpenVPN is still running. Hit ctrl-alt-delete which will launch Task Manager. Select the 'process' button and search for 'OpenVPN-GUI'. Kill the process and close Task Manager. Re-launch OpenVPN from your shortcut or the Start Menu.	

* Starting virtual private.. server FAILED
Stopping virtual private network daemon:.
Starting virtual private network daemon: server(FAILED).


'/usr/sbin/openvpn --config /etc/openvpn/server.conf'
我的问题是conf文件写错了。
