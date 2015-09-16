# vps备忘
***

linode选了美国加州的机房，一个月10美元的套餐


* 设置时区 `dpkg-reconfigure tzdata`
* 设定开机启动 `apt-get install sysv-rc-conf`
* hostname

echo "kelu.org" > /etc/hostname
hostname -F /etc/hostname
* ssh
* lnmp

wget -c http://soft.vpser.net/lnmp/lnmp1.1-full.tar.gz && tar zxf lnmp1.1-full.tar.gz && cd lnmp1.1-full && ./debian.sh
* apt-get install jwm xterm vnc4server iceweasel xrdp
* dropbox

32-bit:
cd ~ && wget -O - "https://www.dropbox.com/download?plat=lnx.x86" | tar xzf -

64-bit:
cd ~ && wget -O - "https://www.dropbox.com/download?plat=lnx.x86_64" | tar xzf -

~/.dropbox-dist/dropboxd

* 	`.bashrc` `.inputrc` `.vim` `/etc/profile` `/etc/rc.local``/etc/motd``/etc/bash.bashrc`
* 	字体 `cp *.tt* /usr/share/fonts/truetype`
* 	mutt msmtprc
*   cron
*   `/var/log/daily-report`


* iptables
* pptp
* 设置软链接快速查看文件`ln -s /var/log/syslog syslog`
* 系统备份

# 备份
tar cvpzf backup.tgz --exclude=/proc --exclude=/lost+found --exclude=/backup.tgz --exclude=/mnt --exclude=/sys /

# 恢复
tar xvpfz backup.tgz -C /

# 然后重建排除在外的文件夹。
mkdir proc
mkdir lost+found
mkdir mnt
mkdir sys


### 查看当前系统

查看Debian的版本信息

lsb_release -a
No LSB modules are available.
  Distributor ID: Debian
Description:    Debian GNU/Linux 7.8 (wheezy)
  Release:        7.8
  Codename:       wheezy

  cat /etc/debian_version
  7.8

  查看内核版本：

  cat /proc/version
  Linux version 3.18.3-x86_64-linode51 (maker@build.linode.com) (gcc version 4.4.5 (Debian 4.4.5-8) ) #1 SMP Fri Jan 23 09:57:22 EST 2015


  uname -a
  Linux kelu.org 3.18.3-x86_64-linode51 #1 SMP Fri Jan 23 09:57:22 EST 2015 x86_64 GNU/Linux

  cat /etc/issue
  Debian GNU/Linux 7 \n \l

#查看当前连接数，ban掉可疑物体

  1
  2
  netstat -ntu | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -n
  iptables -A INPUT -s 12.34.56.78 -j DROP
#测试硬盘io


  1
  2
  3
  dd if=/dev/zero of=test bs=128k count=4k oflag=dsync
  dd if=/dev/zero of=test bs=64k count=4k oflag=dsync
  dd if=/dev/zero of=test bs=48k count=4k oflag=dsync
#安装缺失运行库


  1
  2
  sudo apt-get install gcc automake autoconf libtool make   #适用于debian系
  yum install gcc automake autoconf libtool make   #适用于rpm系
#查看CPU信息


  1
  cat /proc/cpuinfo
#修改权限


  1
  chown -R www-data /home/wwwroot/*
#新建仅用于翻越的ssh


1
2
3
4
5
useradd username -s /usr/sbin/nologin
echo "username:password" | chpasswd
#or
useradd -M -s /sbin/nologin -n username
passwd username
#webbench:


1
2
3
4
5
wget http://img.jybb.me/soft/webbench-1.5.tar.gz
tar zxvf webbench-1.5.tar.gz
cd webbench-1.5
make &amp;&amp; make install
webbench -c 800 -t 50 http://baidu.com/
#httpload


1
2
3
4
5
wget http://soft.vpser.net/test/http_load/http_load-12mar2006.tar.gz
tar zxvf http_load-12mar2006.tar.gz
cd http_load-12mar2006
make &amp;&amp; make install
http_load -p 500 -s 20 list.txt
