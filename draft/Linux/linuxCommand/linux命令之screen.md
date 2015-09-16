# linux命令之screen

向您推荐以下文章，已经将链接贴出来啦。Linux 技巧：使用 screen 管理你的远程会话 http://www.linuxidc.com/Linux/2011-02/32178.htm

详细资料参考以上链接，最下面是本人的一些实际操作，仅供参考：

已经将个人示例的系统版本和YUM库的配置列出来了

1、新建screen会话：直接输入screen命令或者screen -S [会话名称]

2、退出会话：按下组合键Ctrl+a并松开，此时screen窗口等待命令，然后按下d并松开，退出screen窗口。

3、查看当前系统所有screen会话：screen -ls

4、进入某个screen会话：screen -r [会话的PID]

5、在进入某个screen会话后，杀死screen会话：按下组合键Ctrl+a并松开，此时screen窗口等待命令，然后按下大写的K（即组合键：Shift+k）并松开，（系统提示是否要杀死）按下y确认杀死screen会话。

总结：当系统中只有一个screen会话时，输入：screen -r 即可进入这个会话，

当系统中有多个screen会话时，此时输入同样的命令，系统会列出当前所有screen回话，相当于命令：screen -ls

更多的关于screen的参数，可以参考手册：man screen

同时还要多多实际操作，会得到更多的切身体会，希望本文能够对感兴趣的读者有所帮助，欢迎转载，很愿意分享分享，共同成长，谢谢！

[root@localhost ~]# cat /etc/issue
CentOS release 5.4 (Final)
Kernel \r on an \m
[root@localhost ~]# cat /etc/yum.repos.d/CentOS-Base.repo
# CentOS-Base.repo
#
# The mirror system uses the connecting IP address of the client and the
# update status of each mirror to pick mirrors that are updated to and
# geographically close to the client.  You should use this for CentOS updates
# unless you are manually picking other mirrors.
#
# If the mirrorlist= does not work for you, as a fall back you can try the 
# remarked out baseurl= line instead.
#
#
[base]
name=CentOS-$releasever - Base
baseurl=http://mirrors.163.com/centos/5.5/os/i386/
enabled=1
gpgcheck=0
#gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5
[root@localhost ~]# screen
-bash: screen: command not found
[root@localhost ~]# yum -y install screen
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * addons: data.nicehosting.co.kr
 * base: mirror01.idc.hinet.net
 * extras: data.nicehosting.co.kr
 * updates: data.nicehosting.co.kr
Setting up Install Process
Resolving Dependencies
--> Running transaction check
---> Package screen.x86_64 0:4.0.3-1.el5_4.1 set to be updated
--> Finished Dependency Resolution
Dependencies Resolved
==============================================================================================================================================================
 Package                             Arch                                Version                                      Repository                         Size
==============================================================================================================================================================
Installing:
 screen                              x86_64                              4.0.3-1.el5_4.1                              base                              571 k
Transaction Summary
==============================================================================================================================================================
Install      1 Package(s)         
Update       0 Package(s)         
Remove       0 Package(s)         
Total download size: 571 k
Downloading Packages:
screen-4.0.3-1.el5_4.1.x86_64.rpm                                                                                                      | 571 kB     00:01     
warning: rpmts_HdrFromFdno: Header V3 DSA signature: NOKEY, key ID e8562897
base/gpgkey                                                                                                                            | 1.5 kB     00:00     
Importing GPG key 0xE8562897 "CentOS-5 Key (CentOS 5 Official Signing Key) centos-5-key@centos.org>" from /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5
Running rpm_check_debug
Running Transaction Test
Finished Transaction Test
Transaction Test Succeeded
Running Transaction
  Installing     : screen                                                                                                                                 1/1 
Installed:
  screen.x86_64 0:4.0.3-1.el5_4.1                                                                                                                             
Complete!
[root@localhost ~]# screen
[root@localhost ~]# dateSun Apr  3 21:02:22 EDT 2011
[root@localhost ~]#
 
[detached]
[root@localhost ~]# screen -S 20110403
[root@localhost ~]# dateSun Apr  3 21:05:45 EDT 2011
 [root@localhost ~]#
 
[detached]
[root@localhost ~]# screen -ls
There are screens on:
        31550.pts-2.localhost   (Detached)
        31577.20110403  (Detached)
2 Sockets in /var/run/screen/S-root.
 
[root@localhost ~]# screen -r 31577
[root@localhost ~]# date
Sun Apr  3 21:05:45 EDT 2011
[root@localhost ~]#
 
[detached]
[root@localhost ~]# screen -r 31550
[root@localhost ~]# date
Sun Apr  3 21:02:22 EDT 2011
[root@localhost ~]#
 
[screen is terminating]
[root@localhost ~]# man screen
SCREEN(1)                                                            SCREEN(1)
NAME
       screen - screen manager with VT100/ANSI terminal emulation
SYNOPSIS
       screen [ -options ] [ cmd [ args ] ]
       screen -r [[pid.]tty[.host]]
       screen -r sessionowner/[[pid.]tty[.host]]