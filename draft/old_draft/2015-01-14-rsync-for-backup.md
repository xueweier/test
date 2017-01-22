---
layout: post
title: Linux下rsync文件同步服务器配置详解
category: linux
---

Linux下rsync文件同步服务器配置详解
发表于: Linux, Tools, UNIX | 作者: 谋万世全局者
标签: Linux,rsync,文件同步,服务器配置,详解

作者： 北南南北
来自：Linuxsir.Org
摘要： rsync 是一个快速增量文件传输工具，它可以用于在同一主机备份内部的备份，我们还可以把它作为不同主机网络备份工具之用。本文主要讲述的是如何自架rsync服务器，以实现文件传输、备份和镜像。相对tar和wget来说，rsync 也有其自身的优点，比如速度快、安全、高效。

目录

1. 什么是rsync；
2、rsync 服务器的理由和用途；
3、架设rsync服务器过程；
3.1 rsync的安装；
3.2 rsync服务器的配置文件
4 架设rsync服务器的示例说明
4.1 全局定义
4.2 模块定义
5 启动rsync 服务器及防火墙的设置；
5.1 启动rsync服务器；
5.2 rsync服务器和防火墙；
6 通过rsync客户端来同步数据；
6.1 列出rsync 服务器上的所提供的同步内容；
6.2 rsync 客户端同步数据；
6.3 让rsync 客户端自动与服务器同步数据；
7 问题处理；
8 未尽事宜；
9 关于本文;
10 更新日志；
11 参考文档；


+++++++++++++++++++++++++++++++++++
正文
+++++++++++++++++++++++++++++++++++



1 什么是rsync；

    rsync is a file transfer program for Unix systems. rsync uses the “rsync algorithm” which provides a very fast method for bringing remote files into sync. It does this by sending just the differences in the files across the link, without requiring that both sets of files are present at one of the ends of the link beforehand.

    rsync 是一个Unix系统下的文件同步和传输工具。rsync是用 “rsync 算法”提供了一个客户机和远程文件服务器的文件同步的快速方法。

    Some features of rsync include
    rsync 包括如下的一些特性：
    * can update whole directory trees and filesystems
    能更新整个目录和树和文件系统；
    * optionally preserves symbolic links, hard links, file ownership, permissions, devices and times
    有选择性的保持符号链链、硬链接、文件属于、权限、设备以及时间等；
    * requires no special privileges to install
    对于安装来说，无任何特殊权限要求；
    * internal pipelining reduces latency for multiple files
    对于多个文件来说，内部流水线减少文件等待的延时；
    * can use rsh, ssh or direct sockets as the transport
    能用rsh、ssh 或直接端口做为传输入端口；
    * supports anonymous rsync which is ideal for mirroring
    支持匿名rsync 同步文件，是理想的镜像工具；


2 rsync 服务器的理由；

    rsync 服务器架设比较简单，可能我们安装好rsync后，并没有发现配置文件，以及rsync服务器启动程序，因为每个管理员可能对rsync 用途不一样，所以一般的发行版只是安装好软件就完事了，让管理员来根据自己的用途和方向来自己架设rsync服务器；因为这个rsync应用比较广，能在同一台主机进行备份工作，还能在不同主机之间进行工作。在不同主机之间的进行备份，是必须架设rsync 服务器的。

    以我的观点上看，如果在同一台主机进行文件的备分，用复制工具cp就好了。没必要用rsync 这么相对复杂的工具，cp也简单易用，当然这仅仅是个人观点；

    对于重量级服务器来说，应该有网络备份服务器来说，只有本地备份还是不够的，最好还是有网络备份主机，这样数据的安全才有保证。毕竟数据放在服务器本地上还是不太安全，比如磁盘坏掉、被骇客攻入服务器删除数据。其实服务器本身价值并不大，重要的是数据的价值。

    另外对于大量文件从一台服务器上迁移到另一台服务器上，rsync 的确是一个不可不用传输工具。公司有一台文件服务器，配置是CPU Intel Celeon 333Mhz，内存128M，硬盘IDE 80Gx3=240G，里面仅是第一个硬盘的12G的分区安装系统，用了256M做为交换分区，其它的空间我都用来存数据，通过LVM卷来管理磁盘空间，我分了一个 180G的空间给数据存放，当时数据存储容量已经达到了160多G。当时的情况是服务器空间有限，没做本地备份。更不可能新增硬盘上去，因为这台机器没做RAID，硬盘坏掉一个，数据会全毁掉，安全性没有一点保障。在这种情况下，为了保证数据的安全性，我被迫做了一台带有Raid5支持的文件服务器。在选择如何把数据文件完整的传输到新服务器上，我想到了很多的工具，最后想到了rsync 。我花了十分钟架设并调试了rsync ，然后就开工文件传输，因为文件服务器上的文件太多，老的文件服务器配置又低，大约花了两三天吧才得以把所有文件传输完毕。


3 架设rsync服务器过程；

    架设rsync 服务器比较简单，写一个配置文件rsyncd.conf 。文件的书写也是有规则的，我们可以参照rsync.samba.org 上的文档来做；当然我们首先要安装好rsync 这个软件才行；


3.1 rsync的安装；

    软件安装过于简单，现在Linux各大发行版都提供这个软件包，当然您也可以自己编译安装，在目前的情况下，我看没太大的必要；

    [root@linuxsir:beinan]$ sudo apt-get  install  rsync  注：在debian、ubuntu 等在线安装方法；
    [root@linuxsir:beinan]# slackpkg  install  rsync   注：Slackware 软件包在线安装；
    [root@linuxsir:beinan]# yum install rsync    注：Fedora、Redhat 等系统安装方法；

    其它Linux发行版，请用相应的软件包管理方法来安装；如果是源码包，也就是用下面的办法；
    [root@linuxsir:/home/beinan]# tar xvf  sync-xxxx.tar.gz 或sync-xxx.tar.bz2
    [root@linuxsir:/home/beinan]# cd  sync-xxx
    [root@linuxsir:/home/beinan/sync-xxx]# ./configure --prefix=/usr  ;make ;make install   注：在用源码包编译安装之前，您得安装gcc等编译开具才行；


3.2 rsync服务器的配置文件rsyncd.conf ；

    我们可以参照 rsyncd.conf.html。具体步骤如下；

    [root@linuxsir:~]#mkdir /etc/rsyncd  注：在/etc目录下创建一个rsyncd的目录，我们用来存放rsyncd.conf 和rsyncd.secrets文件；
    [root@linuxsir:~]#touch /etc/rsyncd/rsyncd.conf  注：创建rsyncd.conf ，这是rsync服务器的配置文件；
    [root@linuxsir:~]#touch /etc/rsyncd/rsyncd.secrets  注：创建rsyncd.secrets ，这是用户密码文件；
    [root@linuxsir:~]#chmod 600 /etc/rsyncd/rsyncd.secrets  注：为了密码的安全性，我们把权限设为600；
    [root@linuxsir:~]#ls -lh /etc/rsyncd/rsyncd.secrets
    -rw------- 1 root root 14 2007-07-15 10:21 /etc/rsyncd/rsyncd.secrets
    [root@linuxsir:~]#touch /etc/rsyncd/rsyncd.motd

    下一就是我们修改 rsyncd.conf 和rsyncd.secrets 和rsyncd.motd 文件的时候了；

    rsyncd.conf 是rsync服务器主要配置文件，我们来个简单的示例；比如我们要备份服务器上的 /home 和/opt ，在/home中，我想把beinan和samba目录排除在外；

    # Distributed under the terms of the GNU General Public License v2
    # Minimal configuration file for rsync daemon
    # See rsync(1) and rsyncd.conf(5) man pages for help

    # This line is required by the /etc/init.d/rsyncd script
    pid file = /var/run/rsyncd.pid
    port = 873
    address = 192.168.1.171
    #uid = nobody
    #gid = nobody
    uid = root
    gid = root

    use chroot = yes
    read only = yes

    #limit access to private LANs
    hosts allow=192.168.1.0/255.255.255.0 10.0.1.0/255.255.255.0
    hosts deny=*

    max connections = 5
    motd file = /etc/rsyncd/rsyncd.motd

    #This will give you a separate log file
    #log file = /var/log/rsync.log

    #This will log every file transferred - up to 85,000+ per user, per sync
    #transfer logging = yes

    log format = %t %a %m %f %b
    syslog facility = local3
    timeout = 300

    [linuxsirhome]
    path = /home
    list=yes
    ignore errors
    auth users = linuxsir
    secrets file = /etc/rsyncd/rsyncd.secrets
    comment = linuxsir home
    exclude =   beinan/  samba/

    [beinan]
    path = /opt
    list=no
    ignore errors
    comment = optdir
    auth users = beinan
    secrets file = /etc/rsyncd/rsyncd.secrets

    注： 关于 auth users 是必须在服务器上存在的真实的系统用户，如果你想用多个用户，那就以,号隔开；比如 auth users = beinan , linuxsir

    密码文件：/etc/rsyncd/rsyncd.secrets的内容格式；
    用户名:密码

    而我们在例子中rsyncd.secrets的内容如下类似的；在文档中说，有些系统不支持长密码，自己尝试着设置一下吧。另外 rsyncd.secrets文件权限对其它用户组是不可读的。如果你设置错了，可能rsync不工作。
    linuxsir:222222
    beinan:333333

    注： 这里的密码值得注意，为了安全，你不能把系统用户的密码写在这里。比如你的系统用户 linuxsir 密码是 abcdefg ，为了安全，你可以让rsync 中的linuxsir 为 222222 。这和samba的用户认证的密码原理是差不多的；

    rsyncd.motd 文件;

    它是定义rysnc 服务器信息的，也就是用户登录信息。比如让用户知道这个服务器是谁提供的等；类似ftp服务器登录时，我们所看到的 linuxsir.org ftp ……。 当然这在全局定义变量时，并不是必须的，你可以用#号注掉，或删除；我在这里写了一个 rsyncd.motd的内容为：
    +++++++++++++++++++++++++++
    + linuxsir.org  rsync  2002-2007 +
    +++++++++++++++++++++++++++


4 架设rsync服务器的示例说明；


4.1 全局定义；

    在rsync 服务器中，全局定义有几个比较关健的，根据我们前面所给的配置文件 rsyncd.conf 文件；

    pid file = /var/run/rsyncd.pid   注：告诉进程写到 /var/run/rsyncd.pid 文件中；
    port = 873  注：指定运行端口，默认是873，您可以自己指定；
    address = 192.168.1.171  注：指定服务器IP地址；
    uid = nobody
    gid = nobdoy

    注：服务器端传输文件时，要发哪个用户和用户组来执行，默认是nobody。 如果用nobody 用户和用户组，可能遇到权限问题，有些文件从服务器上拉不下来。所以我就偷懒，为了方便，用了root 。不过您可以在定义要同步的目录时定义的模块中指定用户来解决权限的问题。
    use chroot = yes

    注：用chroot，在传输文件之前，服务器守护程序在将chroot 到文件系统中的目录中，这样做的好处是可能保护系统被安装漏洞侵袭的可能。缺点是需要超级用户权限。另外对符号链接文件，将会排除在外。也就是说，你在rsync服务器上，如果有符号链接，你在备份服务器上运行客户端的同步数据时，只会把符号链接名同步下来，并不会同步符号链接的内容；这个需要自己来尝试；
    read only = yes

    注：read only 是只读选择，也就是说，不让客户端上传文件到服务器上。还有一个 write only选项，自己尝试是做什么用的吧；
    #limit access to private LANs
    hosts allow=192.168.1.0/255.255.255.0 10.0.1.0/255.255.255.0

    注：在您可以指定单个IP，也可以指定整个网段，能提高安全性。格式是ip 与ip 之间、ip和网段之间、网段和网段之间要用空格隔开；
    max connections = 5

    注：客户端最多连接数；
    motd file = /etc/rsyncd/rsyncd.motd

    注：motd file 是定义服务器信息的，要自己写 rsyncd.motd 文件内容。当用户登录时会看到这个信息。比如我写的是：
    +++++++++++++++++++++++++++
    + linuxsir.org  rsync  2002-2007 +
    +++++++++++++++++++++++++++
    log file = /var/log/rsync.log

    注：rsync 服务器的日志；
    transfer logging = yes

    注：这是传输文件的日志；
    log format = %t %a %m %f %b
    syslog facility = local3
    timeout = 300


4.2 模块定义；

    模块定义什么呢？主要是定义服务器哪个目录要被同步。每个模块都要以[name]形式。这个名字就是在rsync 客户端看到的名字，其实有点象Samba服务器提供的共享名。而服务器真正同步的数据是通过 path 来指定的。我们可以根据自己的需要，来指定多个模块。每个模块要指定认证用户，密码文件、但排除并不是必须的；

    下面前面配置文件模块的例子：
    [linuxsirhome]
    注：模块，它为我们提供了一个链接的名字，链接到哪呢，在本模块中，链接到了/home目录；要用[name] 形式；
    path = /home    注：指定文件目录所在位置，这是必须指定的；
    auth users = linuxsir   注：认证用户是linuxsir  ，是必须在 服务器上存在的用户；
    list=yes   注：list 意思是把rsync 服务器上提供同步数据的目录在服务器上模块是否显示列出来。默认是yes 。如果你不想列出来，就no ；如果是no是比较安全的，至少别人不知道你的服务器上提供了哪些目录。你自己知道就行了；
    ignore errors  注：忽略IO错误，详细的请查文档；
    secrets file = /etc/rsyncd/rsyncd.secrets   注：密码存在哪个文件；
    comment = linuxsir home  data  注：注释可以自己定义，写什么都行，写点相关的内容就行；
    exclude =   beinan/   samba/

    注：exclude 是排除的意思，也就是说，要把/home目录下的beinan和samba 排除在外； beinan/和samba/目录之间有空格分开 ；
    [beinan]
    path = /opt  注：指定文件目录所在位置；
    list=no
    comment = optdir
    auth users = beinan  注：是必段在服务器上存在的用户；
    secrets file = /etc/rsyncd/rsyncd.secrets
    ignore errors


5 启动rsync 服务器及防火墙的设置；


5.1 启动rsync服务器；

    启动rsync 服务器相当简单，–daemon 是让rsync 以服务器模式运行；

    [root@linuxsir:~]#/usr/bin/rsync --daemon  --config=/etc/rsyncd/rsyncd.conf

    注： 如果你找不到rsync 命令，你应该知道rsync 是安装在哪了。比如rsync 可执行命令可能安装在了 /usr/local/bin目录；也就是如下的命令；
    [root@linuxsir:~]#/usr/local/bin/rsync --daemon  --config=/etc/rsyncd/rsyncd.conf

    当然您也可以写一个脚本来开机自动启动rysnc 服务器，你自己查查文档试试，这个简单。因为我用slackware 也有一个类似的脚本。我感觉不如直接手工运行方面，或者把这个命令写入rc.local文件中，这样也一样能自动运行；


5.2 rsync服务器和防火墙；

    Linux 防火墙是用iptables，所以我们至少在服务器端要让你所定义的rsync 服务器端口通过，客户端上也应该让通过。

    [root@linuxsir:~]#iptables -A INPUT -p tcp -m state --state NEW  -m tcp --dport 873 -j ACCEPT
    [root@linuxsir:~]#iptables -L  查看一下防火墙是不是打开了 873端口；


6 通过rsync客户端来同步数据；


6.1 列出rsync 服务器上的所提供的同步内容；


    首先：我们看看rsync服务器上提供了哪些可用的数据源；

    [beinan@beinnaIBM:~]$ rsync  --list-only  linuxsir@linuxsir.org::

    +++++++++++++++++++++++++++++++++
    +++ linuxsir.org rsync  server ++
    +++++++++++++++++++++++++++++++++

    linuxsirhome    linuxsir  home data

    注： 前面是rsync 所提供的数据源，也就是我们在rsyncd.conf 中所写的[linuxsirhome]模块。而“linuxsir home data”是由[linuxsirhome]模块中的 comment = linuxsir home data 提供的；为什么没有把beinan 数据源列出来呢？因为我们在[beinan]中已经把list=no了。
    [beinan@beinnaIBM:~]$ rsync  --list-only  linuxsir@linuxsir.org::linuxsirhome

    试试这个？


6.2 rsync 客户端同步数据；

    [beinan@beinnaIBM:~]$ rsync -avzP linuxsir@linuxsir.org::linuxsirhome   linuxsirhome
    Password: 这里要输入linuxsir的密码，是服务器端提供的，在前面的例子中，我们用的是 222222，输入的密码并不显示出来；输好后就回车；

    注： 这个命令的意思就是说，用linuxsir 用户登录到服务器上，把linuxsirhome数据，同步到本地目录linuxsirhome上。当然本地的目录是可以你自己定义的，比如 linuxsir也是可以的；当你在客户端上，当前操作的目录下没有linuxsirhome这个目录时，系统会自动为你创建一个；当存在linuxsirhome这个目录中，你要注意它的写权限。

    说明：

    -a 参数，相当于-rlptgoD，-r 是递归 -l 是链接文件，意思是拷贝链接文件；-p 表示保持文件原有权限；-t 保持文件原有时间；-g 保持文件原有用户组；-o 保持文件原有属主；-D 相当于块设备文件；

    -z 传输时压缩；
    -P 传输进度；
    -v 传输时的进度等信息，和-P有点关系，自己试试。可以看文档；
    [beinan@beinnaIBM:~]$ rsync -avzP  --delete linuxsir@linuxsir.org::linuxsirhome   linuxsirhome

    这回我们引入一个 –delete 选项，表示客户端上的数据要与服务器端完全一致，如果 linuxsirhome目录中有服务器上不存在的文件，则删除。最终目的是让linuxsirhome目录上的数据完全与服务器上保持一致；用的时候要小心点，最好不要把已经有重要数所据的目录，当做本地更新目录，否则会把你的数据全部删除；
    [beinan@beinnaIBM:~]$ rsync -avzP  --delete  --password-file=rsync.password  linuxsir@linuxsir.org::linuxsirhome   linuxsirhome

    这次我们加了一个选项 –password-file=rsync.password ，这是当我们以linuxsir用户登录rsync服务器同步数据时，密码将读取 rsync.password 这个文件。这个文件内容只是linuxsir用户的密码。我们要如下做；
    [beinan@beinnaIBM:~]$ touch rsync.password
    [beinan@beinnaIBM:~]$ chmod 600 rsync.passwod
    [beinan@beinnaIBM:~]$ echo "222222"> rsync.password

    [beinan@beinnaIBM:~]$ rsync -avzP  --delete  --password-file=rsync.password  linuxsir@linuxsir.org::linuxsirhome   linuxsirhome

    注： 这样就不需要密码了；其实这是比较重要的，因为服务器通过crond 计划任务还是有必要的；


6.3 让rsync 客户端自动与服务器同步数据；

    服务器是重量级应用，所以数据的网络备份还是极为重要的。我们可以在生产型服务器上配置好rsync 服务器。我们可以把一台装有rysnc机器当做是备份服务器。让这台备份服务器，每天在早上4点开始同步服务器上的数据；并且每个备份都是完整备份。有时硬盘坏掉，或者服务器数据被删除，完整备份还是相当重要的。这种备份相当于每天为服务器的数据做一个镜像，当生产型服务器发生事故时，我们可以轻松恢复数据，能把数据损失降到最低；是不是这么回事？？

    第一步：创建同步脚本和密码文件

    [beinan@beinnaIBM:~] mkdir   /etc/cron.daily.rsync
    [beinan@beinnaIBM:~] cd  /etc/cron.daily.rsync
    [beinan@beinnaIBM:~] touch linuxsir.sh  beinan.sh
    [beinan@beinnaIBM:~] chmod 755 /etc/cron.daily.rsync/*.sh
    [beinan@beinnaIBM:~] mkdir /etc/rsyncd/
    [beinan@beinnaIBM:~] touch  /etc/rsyncd/rsynclinuxsir.password
    [beinan@beinnaIBM:~] touch  /etc/rsyncd/rsyncbeinan.password
    [beinan@beinnaIBM:~] chmod 600  /etc/rsyncd/rsyncbeinan.*

    注： 我们在 /etc/cron.daily/ 中创建了两个文件beinan.sh和linuxsir.sh ，并且是权限是 755的。创建了两个密码文件，linuxsir用户用的是 rsynclinuxsir.password ，而beinan用户用的是 rsyncbeinan.password ，权限是600；

    我们编辑linuxsir.sh，内容是如下的：
    #!/bin/sh
    #linuxsir.org home backup
    /usr/bin/rsync   -avzP  --password-file=/etc/rsyncd/rsynclinuxsir.password   linuxsir@192.168.1.171::linuxsirhome   /home/linuxsirhome/$(date +'%m-%d-%y')

    我们编辑 beinan.sh ，内容是：
    #!/bin/sh
    #linuxsir.org beinan home backup
    /usr/bin/rsync   -avzP  --password-file=/etc/rsyncd/rsyncbeinan.password   linuxsir@192.168.1.171::beinan   /home/beinanhome/$(date +'%m-%d-%y')

    注：你可以把linuxsir.sh 和beinan.sh 的内容合并到一个文件中，比如都写到 linuxsir.sh 中；

    接着我们修改 /etc/rsyncd/rsynclinuxsir.password 和 rsyncbeinan.password的内容；
    [beinan@beinnaIBM:~] echo "222222" > /etc/rsyncd/rsynclinuxsir.password
    [beinan@beinnaIBM:~] echo "333333"> /etc/rsyncd/rsyncbeinan.password

    然后我们再/home目录下创建linuxsirhome 和beinanhome两个目录，意思是服务器端的linuxsirhome数据同步到备份服务器上的/home/linuxsirhome下，beinan数据同步到 /home/beinanhome/目录下。并按年月日归档创建目录；每天备份都存档；
    [beinan@beinnaIBM:~] mkdir /home/linuxsirhome
    [beinan@beinnaIBM:~] mkdir /home/beinanhome

    第二步：修改crond服务器的配置文件
    [beinan@beinnaIBM:~] crontab  -e

    加入下面的内容：
    # Run daily cron jobs at 4:10 every day  backup linuxsir data:
    10 4 * * * /usr/bin/run-parts   /etc/cron.daily.rsync    1> /dev/null

    注：
    第一行是注释，是说明内容，这样能自己记住。
    第二行表示在每天早上4点10分的时候，运行 /etc/cron.daily.rsync 下的可执行脚本任务；

    第三步：重启crond服务器；

    配置好后，要重启crond 服务器；
    [beinan@beinnaIBM:~]# killall crond    注：杀死crond 服务器的进程；
    [beinan@beinnaIBM:~]# ps aux |grep crond  注：查看一下是否被杀死；
    [beinan@beinnaIBM:~]# /usr/sbin/crond    注：启动 crond 服务器；
    [beinan@beinnaIBM:~]# ps aux  |grep crond  注：查看一下是否启动了？
    root      3815  0.0  0.0   1860   664 ?        S    14:44   0:00 /usr/sbin/crond
    root      3819  0.0  0.0   2188   808 pts/1    S+   14:45   0:00 grep crond


7 问题处理；

    当同步出现错误时，可能是你的密码文件权限的问题，或者格式不对，也可能是你复制、粘贴造成的。
    另外权限的问题也应该关注一下，这是最容易出问题的地方；如果您对权限不太了解，应该在LinuxSir.Org 上查找用户和用户组，以及权限方面的知识；


8 未尽事宜；

    本文并不是大而全的rsync 说明文档，仅仅是把网络同步文件的内容说一说，也不一定能完全能说的清楚；对于任何一个工具来说，都足可以写一本书，所以本文也不可能完全全面的介绍rsync 。如果您想了解更多，请参考官方文档或者man ，我也一样查看这些内容来应用rsync 。

    如果我们用普通用户用rsync 进行与服务器同步数据时，同步下来的数据，可能属主会改变。为了保持文件的属主和用户组与服务器端完全一致，用root来运行rsync 就可以了……


9 关于本文；

    本文是为了解决公司内部文件上服务器数据迁移到新的服务器应用时而写的，并不是特地为写rysnc 而写rsync 。我用不到的东西，或者我不会的东西，我想写也写不出来；其实我什么不会，只是想应用时，才临时抱佛脚，解决好问题后就忘掉。等用到的时候再查我写过的东西…… :(

    本文适合对象，面向所有初学者。如果您是高级应用者，http://rsync.samba.org 完全能满足我们的需要。

    “北南不息，写DOC不止；”，只是没有太多的时间罢了……


10 更新日志；

    2007/07/17 v0.1b 全文完成，进入抓虫阶段；


11 参考文档；

    rsyncd.conf
    http://rsync.samba.org
    《HOWTO_Local_Rsync_Mirror》
    《Linux 文件和目录的属性》

转自：http://www.linuxsir.org/main/?q=node/256
