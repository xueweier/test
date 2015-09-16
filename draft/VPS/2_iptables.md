# iptables
***

iptables相当于静态的防火墙，可以定义访问规则。debian下没有用于储存规则的文件，可以自己写一个。参考[wiki](http://wiki.debian.org/iptables)
  
	[root@tp ~]# iptables -F 清除预设表filter中的所有规则链的规则
	[root@tp ~]# iptables -X 清除预设表filter中使用者自定链中的规则
	
### 新建规则	
新建 `/etc/iptables.test.rules` 来编辑规则，比如以下是wiki上的例子，可以添加自己需要的端口和规则，记得加上log的条目，这样方便以后进行修改。可以在/var/log/syslog里面查看记录。编辑好了用以下命令加载到iptables中

	iptables-restore < /etc/iptables.test.rules
	iptables -L
	iptables-save > /etc/iptables.up.rules

经常默认drop，可以改回来。iptables -t filter -P INPUT ACCEPT

### 开机启动
	vi /etc/network/if-pre-up.d/iptables
	
	#!/bin/sh
	/sbin/iptables-restore < /etc/iptables.up.rules

	chmod +x /etc/network/if-pre-up.d/iptables


### 规则文件

    *nat
    
    # VPN postrouting
    -A POSTROUTING -s 192.168.200.0/24 -o eth0 -j MASQUERADE
    
    COMMIT
    
    *filter
    
    # Allows all loopback (lo0) traffic and drop all traffic to 127/8 that doesn't use lo0
    -A INPUT -i lo -j ACCEPT
    -A INPUT ! -i lo -d 127.0.0.0/8 -j REJECT
    
    # Accepts all established inbound connections
    -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
    
    # Allows all outbound traffic
    # You could modify this to only allow certain traffic
    -A OUTPUT -j ACCEPT
    
    # Allows HTTP and HTTPS connections from anywhere (the normal ports for websites)
    -A INPUT -p tcp --dport 80 -j ACCEPT
    -A INPUT -p tcp --dport 443 -j ACCEPT
    -A INPUT -p udp --dport 80 -j ACCEPT
    -A INPUT -p udp --dport 443 -j ACCEPT
    
    # Allows FTP connection
    -A INPUT -p tcp --dport 20 -j ACCEPT
    -A INPUT -p tcp --dport 21 -j ACCEPT
    -A INPUT -p tcp --dport 31344 -j ACCEPT
    -A INPUT -p tcp --dport 49000:49500 -j ACCEPT
    
    # Allows DNS connection
    -A INPUT -p tcp --dport 53 -j ACCEPT
    -A INPUT -p udp --dport 53 -j ACCEPT
    
    # Allows SMTP connection
    -A INPUT -p tcp --dport 465 -j ACCEPT
    -A INPUT -p tcp --dport 25 -j ACCEPT
    
    # Allows POP3 connection
    -A INPUT -p tcp --dport 110 -j ACCEPT
    
    # Allows IMAP connection
    -A INPUT -p tcp --dport 143 -j ACCEPT
    
    -A INPUT -p tcp --dport 21344 -j ACCEPT
    # Allow VPN port and protocol
    # PPTP
    -A INPUT -p tcp --dport 1723 -j ACCEPT
    -A INPUT -p gre -j ACCEPT
    -A INPUT -i eth0 -s 192.168.200.0/24 -j ACCEPT
    -A FORWARD -i eth0 -d 192.168.200.0/24 -j ACCEPT
    -A FORWARD -o eth0 -s 192.168.200.0/24 -j ACCEPT
    
    # # L2TP
    # -A INPUT -p udp --dport 1701 -j ACCEPT
    # # IPSec
    # -A INPUT -p udp --dport 500 -j ACCEPT
    # -A INPUT -p udp --dport 4500 -j ACCEPT
    # -A FORWARD -i eth0 -d 10.10.20.0/24 -j ACCEPT
    # -A FORWARD -o eth0 -s 10.10.20.0/24 -j ACCEPT
    
    # #OpenVPN
    # -A INPUT -p udp --dport 21344 -j ACCEPT
    # -A INPUT -p udp --dport 21345 -j ACCEPT
    # -A FORWARD -i eth0 -d 10.8.1.0/24 -j ACCEPT
    # -A FORWARD -o eth0 -s 10.8.1.0/24 -j ACCEPT
    # -A FORWARD -i eth0 -d 10.8.2.0/24 -j ACCEPT
    # -A FORWARD -o eth0 -s 10.8.2.0/24 -j ACCEPT
    
    #Squid http proxy
    
    # Allows SSH connections
    # THE -dport NUMBER IS THE SAME ONE YOU SET UP IN THE SSHD_CONFIG FILE
    -A INPUT -p tcp -m state --state NEW --dport 1344 -j ACCEPT
    
    # Now you should read up on iptables rules and consider whether ssh access
    # for everyone is really desired. Most likely you will only allow access from certain IPs.
    
    # Allow ping
    -A INPUT -p icmp -m icmp --icmp-type 8 -j ACCEPT
    
    # log iptables denied calls (access via 'dmesg' command)
    -A INPUT -m limit --limit 5/min -j LOG --log-prefix "iptables input denied: " --log-level 7
    -A FORWARD -m limit --limit 5/min -j LOG --log-prefix "iptables forward denied: " --log-level 7
    
    # Reject all other inbound - default deny unless explicitly allowed policy:
    -A INPUT -j REJECT
    -A FORWARD -j REJECT
    
    COMMIT
