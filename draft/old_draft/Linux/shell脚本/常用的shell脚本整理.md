### 常用的Shell脚本整理

有用的网站：

[Advanced Bash-Scripting Guide](http://tldp.org/LDP/abs/html/)

[tutorialspoint](http://www.tutorialspoint.com/unix/unix-shell.htm)

[Linux Shell Scripting Tutorial (LSST) v2.0](http://bash.cyberciti.biz/guide/Main_Page)


常用的shell命令整理
2013年06月26日 ⁄ Linux, shell, 程序人生 ⁄ 共 4323字	⁄ 暂无评论 ⁄ 被围观 10,859 views+
工作快一年了，shell命令也玩了一年了。还是有点积累的，下面是本人常用的。

1、pwd | xargs -i basename {}   获取当前所在目录的名称

2、ps -ef|grep -w  indexd_admin_mcd.pid|grep -v grep|wc -l

ps -ef 查找进程    grep -v  查找不存在  grep -w 强制 PATTERN 仅匹配整个词

查找进程中为 indexd_admin_mcd.pid的进程，并且排除掉grep的进程。最后计数，这样进程的个数。

3、if [ $cn -lt 1 ]   如果cn的值< 1

4、ulimit  -c   最大的core文件的大小，以blocks为单位。    ulimit -c  unlimited  对生成的core文件大小不进行限制。

5、kill

kill -9 pid，是不顾后果的强制终止(如果的你的速度够快，有时候是和ctrl＋c是一样的)

kill -15 pid，是先关闭和其有关的程序，再将其关闭

6、crontab

crontab -l    显示crontab中的所有的内容

crontab filename     用新的文件代替crontab里的所有的内容。

所有想在crontab中添加新的自动运行的进程，则先用crontab -l >temp，先将crontab中所有的内容全部重定向到一个新的文件中，然后在这个temp文件后>>追加内容。最后用crontab temp，用temp的文件内容替代crontab的所有的内容。

crontab文件的格式：分  时  日  月  星期

7、$()与‘’的意思相同，获取shell执行后的值，但是用$()会更加的直观。  ${},{}中经常放的是变量，这样在${}就可以精确后面的变量的范围了。

${file#*/}：拿掉第一条/及其左边的字符串： dir1/dir2/dir3/my.file.txt

${file##*/}：拿掉最后一条/及其左边的字符串：my.file.txt

${file#*.}：拿掉第一个.及其左边的字符串：file.txt

${file##*.}：拿掉最后一个.及其左边的字符串：txt

${file%/*}：拿掉最后条/及其右边的字符串：/dir1/dir2/dir3

${file%%/*}：拿掉第一条 / 及其右边的字符串：(空值)

${file%.*}：拿掉最后一个 . 及其右边的字符串：/dir1/dir2/dir3/my.file

${file%%.*}：拿掉第一个 . 及其右边的字符串：/dir1/dir2/dir3/my

8、运算

(( A )) 做运算的，A是任何的运算的表达式 A中的变量可用$来替换，也可以不用。

 9、if  判断条件中 $#   和$? 是什么意思？

$#  获取参数的数目my.sh p1 "p2 p3" 为例   $#可得到2

$@ 与 $* 均可获得所有的参数 my.sh p1 "p2 p3" p4 $@ 与 $* 得到p1 p2 p3 p4

但是，如果置于 soft quote 中的话：

"$@" 则可得到 "p1" "p2 p3" "p4" 这三个不同的词段(word)

"$*" 则可得到 "p1 p2 p3 p4" 这一整串单一的词段。

10、在 shell command line 中可用 $? 这个变量得到最新的一个 return value ，也就是刚结束的那个行程传回的值。

* 若在 script 里，用 exit RV 来指定其值，若没指定，在结束时以最后一道命令之 RV 为值。

* 若在 function 里，则用 return RV 来代替 exit RV 即可。

11、重定向输出

* 2>&1 就是将 stderr 并进 stdout 作输出

* 1>&2 或 >&2 就是将 stdout 并进 stderr 作输出

ls my.file no.such.file 2>/dev/null

* 若将 FD1 跟 FD2 转到 /dev/null 去，就可将 stdout 与 stderr 弄不见掉。

* 若将 FD0 接到 /dev/null 来，那就是读进 nothing 。

>/dev/null 2>&1   单纯只跑程序，不想看到任何输出结果  除了用 >/dev/null 2>&1 之外，你还可以&>/dev/null

12、tee复制

所谓 tee 命令是在不影响原本 I/O 的情况下，将 stdout 复制一份到档案去。

cm1 | cm2 | tee file | cm3

在预设上， tee 会改写目标档案，若你要改为增加内容的话，那可用 -a 参数达成。

凡举 cat, more, head, tail, wc, expand, tr, grep, sed, awk, ... 等等文字处理工具，搭配起 pipe line 来使用

13、shell程序的参数

  $0   当前程序的执行名字

  $n   当前程序的第n个参数值，n=1..9

  $*   当前程序的所有参数

  $#   当前程序的参数个数

  $$   当前程序的PID

  $!   执行上一个指令的PID

  $?  执行上一个指令的返回值

14、date显示时间

date '+%F %T'    2012-08-27 15:52:54   等同于date +"%Y-%m-%d %T"

date -d '-1 days' +"%Y%m%d"    获取前一天的时间

15、if添加的判断的另一个方式

判断参数的个数

01
if (( $# < 1 ))
02
then
03
    echo "Usage: $0 worker_id"
04
    exit 1
05
fi
06
if (( $# == 1 ))
07
then
08
    WORKER_ID=$1
09
    MAX_WORKER_ID=$WORKER_ID
10
elif (( $# > 1 ))
11
then
12
    echo "Usage: $0 [worker_id]"
13
    exit 1
14
else
15
    echo "worker count $WORKER_CNT"
16
fi
这样写if的条件的判断就和c语言的一样了，条件运算

16、ipcs  查看共享内存的使用的情况

ipcrm -M  shmid  关闭共享内存

17、make -p

即使data这个文件夹不存在，也可以创建他的子目录，当然同同时，他也被创建。

mkdir -p /data/coredump

18、head  tail

head 是显示一个文件的内容的前多少行；

head  -n  行数值  文件名；

比如我们显示/etc/profile的前10行内容，应该是：

[root@localhost ~]# head -n 10 /etc/profile

tail 工具，显示文件内容的最后几行，用法比较简单；

tail   -n  行数值  文件名；

比如我们显示/etc/profile的最后5行内容，应该是：

[root@localhost ~]# tail  -n 5 /etc/profile

更多：http://blog.csdn.net/carzyer/article/details/4759593

19、创建软链接

ln -sf  a  b     b链向a

20、export  执行的路径

export LD_LIBRARY_PATH=./

21、if  [ expr ];   then … fi

-n str

字符串 str 是否不为空

-z str

字符串是否为空

str1  = str2

 str1是否与 str2 相同

str1 != str2

 str1是否与 str2 不同

int1 -eq int2

等于

int1 -le  int2

小于等于

int1 -ge int2

大于等于

int1 -lt   int2

小于

int1 -gt  int2

大于

int1 -ne int2

不等于

-b

是否块文件

-p

文件是否为一个命名管道

-c

是否字符文件

-r

文件是否可读

-d

是否一个目录 *

-s

文件的长度是否不为零

-e

文件是否存在 *

-S

是否为套接字文件

-f

是否普通文件 *

-x

文件是否可执行，则为真

-g

是否设置了文件的 SGID 位

-u

是否设置了文件的 SUID 位

-G

文件是否存在且归该组所有

-w

文件是否可写，则为真

-k

文件是否设置了的粘贴位

-t fd

fd 是否是一个与终端相连的打开的文件描述符（fd 默认为 1）

-O

文件是否存在且归该用户所有

22、for循环

1
for((i=1;i<=100;++i))
2
do
3
    echo $i
4
done
23、对一行的第一列进行排重，只保留最新的数据。awk的内置的数组进行排重，排重之后在sort

01
filename=$1
02
awk '
03
{
04
    cont[$1] = $0;
05
}
06
END {
07
    for (key in cont)
08
    {
09
    print cont[key];
10
    }
11
}' $filename | sort -k1
24、显示上次文件的创建的时间

stat -c %Y $file

25、while循环

http://www.linuxidc.com/Linux/2011-02/32239p2.htm

http://www.cnblogs.com/chengmo/archive/2010/10/14/1851434.html

26、查看当前目录下的文件的大小

du -h --max-depth=1

27、# nohup  ./pso > pso.file 2>&1 &

解释：nohup就是不挂起的意思，将pso直接放在后台运行，并把终端输出存放在当前目录下的pso.file文件中。

当客户端关机后重新登陆服务器后，直接查看pso.file文件就可看执行结果（命令：#cat pso.file ）。

http://www.cnblogs.com/xianghang123/archive/2011/08/02/2125511.html

28、将大文件分割为指定大小的文件

http://huangyandong.blog.51cto.com/1396940/690276

split -b 1m 20121018_quey.txt

29、对windows上传的文件进行处理，处理特殊字符

dos2unix over_wap_query_seed.20121023.txt

30、将window的^M回车符号换成linux的

tr -s "[\r]" "[\n]" < log.get

tr -s "[\015]" "\n" < log.get

31、在securecrt软件上显示用户名和ip

echo -ne "\e]2;${USER}@$(/sbin/ifconfig eth1 | awk -F"[ :]+" '/inet addr/{print $4}')\a"

如果你是root用户，把下面上面的那句命令行追加到 /etc/profile

如果你是个人账户，在自己的家目录新建一个 .profile 文件，把命令行追加到.profile文件

32、shell保留两位小数

echo "scale=2; 1*100/3" | bc

33、去掉变量的最后一个字符

AA=abcd

echo $AA | cut -c 1-$(expr ${#AA} - 1)

34、linux下如何查询已知进程运行目录

 ls -l /proc/PID/cwd

最后为大家分享两个pdf文档，这是我入门学习shell时候用的。

下载地址
shell十三问简体中文版本.pdf

下载地址
ShellProgramming





发表于: Linux, Shell, UNIX | 作者: 谋万世全局者
标签: Shell,常用,整理,脚本
如何计算当前目录下的文件数和目录数
	
	# ls -l * |grep "^-"|wc -l ---- to count files
	# ls -l * |grep "^d"|wc -l ----- to count dir

如何只列子目录？

ls -F | grep /$ 或者 alias sub = "ls -F | grep /$"(linux)

ls -l | grep "^d" 或者 ls -lL | grep "^d" (Solaris)

如何实现取出文件中特定的行内容

如果你只想看文件的前5行，可以使用head命令，
如： head -5 /etc/passwd

如果你想查看文件的后10行，可以使用tail命令，
如： tail -10 /etc/passwd

你知道怎么查看文件中间一段吗？你可以使用sed命令
如: sed -n '5,10p' /etc/passwd 这样你就可以只查看文件的第5行到第10行。

如何查找含特定字符串的文件

例如查找当前目录下含有"the string you want find..."字符串的文件：
　　$find . -type f -exec grep “the string you want find...” {} ; -print


如何列出目录树

下面的短小的shell程序可以列出目录树, 充分利用了sed强大的模式匹配能力.
　　目录树形式如下:
　　.
　　`—-shellp
　　`—-updates
　　`—-wu-ftpd-2.4
　　| `—-doc
　　| | `—-examples
　　| `—-src
　　| | `—-config
　　| | `—-makefiles
　　| `—-support
　　| | `—-makefiles
　　| | `—-man
　　| `—-util
　　脚本如下：

　　#!/bin/sh
　　# dtree: Usage: dtree [any directory]
　　dir=${1:-.}
　　(cd $dir; pwd)
　　find $dir -type d -print | sort -f | sed -e "s,^$1,," -e "/^$/d" -e "s,[^/]*/([^/]*)$,`----1," -e "s,[^/]*/,| ,g"

如何实现取出文件中特定的列内容

我们经常会遇到需要取出分字段的文件的某些特定字段，例如/etc/password就是通过“:”分隔各个字段的。可以通过cut命令来实现。例如，我们希望将系统账号名保存到特定的文件，就可以：
　　cut -d: -f 1 /etc/passwd >; /tmp/users
　　-d用来定义分隔符，默认为tab键，-f表示需要取得哪个字段。
　　当然也可以通过cut取得文件中每行中特定的几个字符，例如：
　　cut -c3-5 /etc/passwd
　　就是输出/etc/passwd文件中每行的第三到第五个字符。
　　-c 和 －f 参数可以跟以下子参数：
　　N 第N个字符或字段
　　N- 从第一个字符或字段到文件结束
　　N-M 从第N个到第M个字符或字段
　　-M 从第一个到第N个字符或字段

在vim中实现批量加密

密码中还是不能带空格，不管了，能加密就好，先这么用着。

============================================================

#!/bin/bash
# Encrypt file with vim

if (test $# -lt 2) then
echo Usage: decrypt password filename
else
vim -e -s -c ":set key=$1" -c ':wq' $2
echo "$2 encrypted."
fi

============================================================
[weeder@SMTH weeder]$ for file in *.txt ; do encrypt test $file ; done
test2.txt encrypted.
test4.txt encrypted.
test9.txt encrypted.
kick.txt encrypted.
echo “$2 encrypted.”
fi
[weeder@SMTH weeder]$ for file in *.txt ; do encrypt test $file ; done
test2.txt encrypted.
test4.txt encrypted.
test9.txt encrypted.
kick.txt encrypted.
too_old.txt encrypted.
too_old_again.txt encrypted.
bg5.txt encrypted.
[weeder@SMTH weeder]$

$@等特定shell变量的含义

在shell脚本的实际编写中，有一些特殊的变量十分有用：
$# 传递到脚本的参数个数

$* 以一个单字符串显示所有向脚本传递的参数。与位置变量不同，此选项参数可超过9个

$$ 脚本运行的当前进程ID号

$! 后台运行的最后一个进程的进程ID号

$@ 与$#相同，但是使用时加引号，并在引号中返回每个参数

$- 显示shell使用的当前选项，与set命令功能相同

$? 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。

如何使程序的执行结果同时定向到屏幕和文件

program_name |tee logfile
这样程序执行期间的显示都记录到logfile同时显示到标准输出（屏幕）。

如何用sendmail给系统所有用户送信

首先在aliases文件里面建立一个alias：
alluser: :include:/etc/mail/allusers
并执行newaliases使之生效，然后在/etc/mail/allusers里面列出所有用户，可以使用下面的命令：
awk -F: '$3 >; 100 { print $1 }' /etc/passwd >; /etc/mail/allusers

如何查找某条命令的相关库文件

在制作自己的发行版时经常需要判断某条命令需要哪些库文件的支持，以确保指定的命令在独立的系统内可以可靠的运行。
在Linux环境下通过ldd命令即可实现，在控制台执行：
ldd /bin/ls
即可得到/bin/ls命令的相关库文件列表。

如何使用host命令获得更多信息

Host能够用来查询域名，然而它可以得到更多的信息。host -t mx linux.com可以查询出Linux.com的MX记录，以及处理Mail的Host的名字。Host -l linux.com会返回所有注册在linux.com下的域名。host -a linux.com则会显示这个主机的所有域名信息。

如何停止终端多个进程

以下是脚本：

　　echo "系统当前用户"
　　echo "---------------"
　　who | awk '{print $2}'
　　echo "---------------"
　　echo "输入要杀死终端的终端号:"
　　read $TTY
　　kill -9 ${K}=`ps -t $TTY | grep [0-9] | awk '{print $1}'`
　　
　　
　　
　　
　　
　　
　　
　　
　　
　　
按时按登录IP记录Linux所有用户操作日志的方法（附脚本）
发表于: Linux, Security, Shell, UNIX, 个人日记, 原创总结 | 作者: 谋万世全局者
标签: IP记录,Linux,总结,按时,方法,日志,用户操作,脚本
PS：Linux用户操作记录一般通过命令history来查看历史记录，但是如果因为某人误操作了删除了重要的数据，这种情况下history命令就不会有什么作用了。以下方法可以实现通过记录登陆IP地址和所有用户登录所操作的日志记录！

在/etc/profile配置文件的末尾加入以下脚本代码就可以实现，下面脚本是我网上找来的，原作者不知。但原脚本的时间变量有错误，不能记录时间，本人测试发现并检查修正：


PS1="`whoami`@`hostname`:"'[$PWD]'
history
USER_IP=`who -u am i 2>/dev/null| awk '{print $NF}'|sed -e 's/[()]//g'`
if [ "$USER_IP" = "" ]
then
USER_IP=`hostname`
fi
if [ ! -d /tmp/history ]
then
mkdir /tmp/history
chmod 777 /tmp/history
fi
if [ ! -d /tmp/history/${LOGNAME} ]
then
mkdir /tmp/history/${LOGNAME}
chmod 300 /tmp/history/${LOGNAME}
fi
export HISTSIZE=4096
DT=`date +"%Y%m%d_%H%M%S"`
export HISTFILE="/tmp/history/${LOGNAME}/${USER_IP} history.$DT"
chmod 600 /tmp/history/${LOGNAME}/*history* 2>/dev/null
通过上面的代码可以看出来，在系统的/tmp新建个history目录（这个目录可以自定义），在目录中记录了所有的登陆过系统的用户和IP地址，这也是监测系统安全的方法之一。








Linux常用服务器安全shell脚本
发表于: Linux, Shell, UNIX | 作者: 谋万世全局者
标签: Linux,Shell,常用,服务器安全,脚本
1. 找出那些被规则禁掉的IP,嗅探器

这段代码会在当前目录下面生成黑名单，根据nginx的access日志，统计被禁掉的403访问，它们不是搜索引擎的爬虫 ，也不是本机地址。

安全提示： 这个脚本对所有IP地址都是全面封杀无误的，所以你自己在使用这个脚本之前，千万要记住，不要为了测试你的规则，而直接用你的电脑，去测试 。因为万一触发了规则，就可能会被下面的脚本所查出来，包含在内。这种状况是可以避免的。
1. 通过openvpn代理上网，如果openvpn和服务器是同一台，那么ip地址就是127.0.0.1，这个IP己经被安全的排除在外了。另外注意，如果你用了chnroute,那么就要看你的VPS是在美国还是在国内，如果在美国，则无需担心，如果在国内，那么chnroute会把路由判断为还是用原来国内的IP,还是会触发规则。建议如果你的VPS在国内，就不要用chnroute。你自己 对网站进行侵入测试，必须挂openvpn来弄。
2. 可以在下面的脚本里面增加一条 grep -v “xx.xx.xx.xx” ，把你的IP排除在外，这个时候就有一点小繁琐，因为如果你跟我一样是用adsl上网，就会需要手动查看自己的IP，每次手动查看IP也挺麻烦的，所以还是建议用第一条方案。
安全脚本文件，用于生成blacklist. 文件名: genblacklist.sh

#!/bin/sh
cat /var/log/nginx/access_log | grep " 403 " | grep -v google | grep -v sogou | grep -v baidu | grep -v "127.0.0.1" | grep -v "soso.com" | awk '{ print $1 }' | uniq | awk -F":" '{print $4}' | sort | uniq > blacklist.txt


2. 根据提供的blacklist禁止IP,解禁IP
### 禁止IP脚本blockip.sh

#!/bin/sh
echo "Block following ip:"
result=""
while read LINE
do
/sbin/iptables -A FORWARD -s $LINE -j DROP
if [ $? = "0" ];then
result=$result$LINE","
fi
done < /vhosts/blacklist.txt
echo $result"Done";

解禁IP脚本releaseip.sh


#!/bin/sh
echo "Release following ip:"
result=""
while read LINE
do
/sbin/iptables -D FORWARD -s $LINE -j DROP
if [ $? = "0" ];then
result=$result$LINE","
fi
done < /vhosts/blacklist.txt
echo $result"Done";

3. 封杀嗅探器

location ~* .(mdb|asp|rar) {
deny all;
}
if ($http_user_agent ~ ^Mozilla/4.0$ ) {
return 403;
}

4. 利用Cron自动屏蔽非法IP
crontab -e编辑cron任务表。
增加一条*/30 * * * * /vhosts/reblock.sh
/vhosts/reblock.sh是自动执行封锁的命令。


#!/bin/sh
cd /vhosts
/bin/sh /vhosts/releaseip.sh
/bin/sh /vhosts/genblacklist.sh
/bin/sh /vhosts/blockip.sh
echo "total ip in black list:"
/bin/cat /vhosts/blacklist.txt | /usr/bin/wc -l

来源：http://jed.dzhope.com/read.php?746