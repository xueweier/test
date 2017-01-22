### Linux的经典shell命令整理
发表于: Linux, Shell, UNIX, 运维经验 | 作者: 谋万世全局者
标签: Linux,Shell,命令,整理,经典
Linux常用的系统监控shell脚本
发表于: Linux, Shell, UNIX, 运维经验 | 作者: 谋万世全局者
标签: Linux,Shell,常用,系统监控,脚本
下面是我常用的几个Linux系统监控的脚本，大家可以根据自己的情况在进行修改，希望能给大家一点帮助。

1、查看主机网卡流量

#!/bin/bash

#network

#Mike.Xu

while : ; do

time=’date +%m”-”%d” “%k”:”%M’

day=’date +%m”-”%d’

rx_before=’ifconfig eth0|sed -n “8″p|awk ‘{print $2}’|cut -c7-’

tx_before=’ifconfig eth0|sed -n “8″p|awk ‘{print $6}’|cut -c7-’

sleep 2

rx_after=’ifconfig eth0|sed -n “8″p|awk ‘{print $2}’|cut -c7-’

tx_after=’ifconfig eth0|sed -n “8″p|awk ‘{print $6}’|cut -c7-’

rx_result=$[(rx_after-rx_before)/256]

tx_result=$[(tx_after-tx_before)/256]

echo “$time Now_In_Speed: “$rx_result”kbps Now_OUt_Speed: “$tx_result”kbps”

sleep 2

done


2、系统状况监控

#!/bin/sh

#systemstat.sh

#Mike.Xu

IP=192.168.1.227

top -n 2| grep “Cpu” >>./temp/cpu.txt

free -m | grep “Mem” >> ./temp/mem.txt

df -k | grep “sda1″ >> ./temp/drive_sda1.txt

#df -k | grep sda2 >> ./temp/drive_sda2.txt

df -k | grep “/mnt/storage_0″ >> ./temp/mnt_storage_0.txt

df -k | grep “/mnt/storage_pic” >> ./temp/mnt_storage_pic.txt

time=`date +%m”.”%d” “%k”:”%M`

connect=`netstat -na | grep “219.238.148.30:80″ | wc -l`

echo “$time  $connect” >> ./temp/connect_count.txt

3、监控主机的磁盘空间,当使用空间超过90％就通过发mail来发警告

#!/bin/bash

#monitor available disk space

SPACE=’df | sed -n ‘/ / $ / p’ | gawk ‘{print $5}’ | sed  ’s/%//’

if [ $SPACE -ge 90 ]

then

fty89@163.com

fi

4、 监控CPU和内存的使用情况

#!/bin/bash

#script  to capture system statistics

OUTFILE=/home/xu/capstats.csv

DATE=’date +%m/%d/%Y’

TIME=’date +%k:%m:%s’

TIMEOUT=’uptime’

VMOUT=’vmstat 1 2′

USERS=’echo $TIMEOUT | gawk ‘{print $4}’ ‘

LOAD=’echo $TIMEOUT | gawk ‘{print $9}’ | sed “s/,//’ ‘

FREE=’echo $VMOUT | sed -n ‘/[0-9]/p’ | sed -n ’2p’ | gawk ‘{print $4} ‘ ‘

IDLE=’echo  $VMOUT | sed -n ‘/[0-9]/p’ | sed -n ’2p’ |gawk ‘{print $15}’ ‘

echo “$DATE,$TIME,$USERS,$LOAD,$FREE,$IDLE” >> $OUTFILE

5、全方位监控主机

#!/bin/bash

# check_xu.sh

# 0 * * * * /home/check_xu.sh

DAT=”`date +%Y%m%d`”

HOUR=”`date +%H`”

DIR=”/home/oslog/host_${DAT}/${HOUR}”

DELAY=60

COUNT=60

# whether the responsible directory exist

if ! test -d ${DIR}

then

/bin/mkdir -p ${DIR}

fi

# general check

export TERM=linux

/usr/bin/top -b -d ${DELAY} -n ${COUNT} > ${DIR}/top_${DAT}.log 2>&1 &

# cpu check

/usr/bin/sar -u ${DELAY} ${COUNT} > ${DIR}/cpu_${DAT}.log 2>&1 &

#/usr/bin/mpstat -P 0 ${DELAY} ${COUNT} > ${DIR}/cpu_0_${DAT}.log 2>&1 &

#/usr/bin/mpstat -P 1 ${DELAY} ${COUNT} > ${DIR}/cpu_1_${DAT}.log 2>&1 &

# memory check

/usr/bin/vmstat ${DELAY} ${COUNT} > ${DIR}/vmstat_${DAT}.log 2>&1 &

# I/O check

/usr/bin/iostat ${DELAY} ${COUNT} > ${DIR}/iostat_${DAT}.log 2>&1 &

# network check

/usr/bin/sar -n DEV ${DELAY} ${COUNT} > ${DIR}/net_${DAT}.log 2>&1 &

#/usr/bin/sar -n EDEV ${DELAY} ${COUNT} > ${DIR}/net_edev_${DAT}.log 2>&1 &

放在crontab里每小时自动执行：

0 * * * * /home/check_xu.sh

这样会在/home/oslog/host_yyyymmdd/hh目录下生成各小时cpu、内存、网络，IO的统计数据。

如果某个时间段产生问题了，就可以去看对应的日志信息，看看当时的主机性能如何。

作者：Mike.Xu
来源：http://www.dbasky.net/archives/2009/10/shell.html
1.check_user.sh

#!/bin/bash

echo "You are logged in as `whoami`";

if [ `whoami` != linuxtone ]; then
echo "Must be logged on as linuxtone to run this script."
exit
fi


echo "Running script at `date`"

2.do_continue.sh

#!/bin/bash

doContinue=n
echo "Do you really want to continue? (y/n)"
read doContinue

if [ "$doContinue" != y ]; then
echo "Quitting..."
exit
fi


echo "OK... we will continue."

3.hide_input.sh

#!/bin/bash

stty -echo
echo -n "Enter the database system password: "
read pw
stty echo


echo "$pw was entered"

4.is_a_directory.sh

#!/bin/bash

if [ -z "$1" ]; then

echo ""
echo " ERROR : Invalid number of arguments"
echo " Usage : $0 "
echo ""
exit
fi


if [ -d $1 ]; then
echo "$1 is a directory."
else
echo "$1 is NOT a directory."
fi

5.is_readable.sh

#!/bin/bash

if [ -z "$1" ]; then

echo ""
echo " ERROR : Invalid number of arguments"
echo " Usage : $0 "
echo ""
exit
fi


if [ ! -r $1 ]; then
echo "$1 is NOT readable."
else
echo "$1 is readable."
fi

6.print_args.sh

#!/bin/bash
#
# The shift command removes the argument nearest
# the command name and replaces it with the next one
#


while [ $# -ne 0 ]
do
echo $1
shift
done
1.删除0字节文件
find -type f -size 0 -exec rm -rf {} ;

2.查看进程
按内存从大到小排列
ps -e -o “%C : %p : %z : %a”|sort -k5 -nr

3.按cpu利用率从大到小排列
ps -e -o “%C : %p : %z : %a”|sort -nr

4.打印说cache里的URL
grep -r -a jpg /data/cache/* | strings | grep “http:” | awk -F’http:’ ‘{print “http:”$2;}’

5.查看http的并发请求数及其TCP连接状态：
netstat -n | awk ‘/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}’

6.
sed -i ‘/Root/s/no/yes/’ /etc/ssh/sshd_config sed在这个文里Root的一行，匹配Root一行，将no替换成yes.

7.1.如何杀掉mysql进程：
ps aux|grep mysql|grep -v grep|awk ‘{print $2}’|xargs kill -9

(从中了解到awk的用途)
pgrep mysql |xargs kill -9

killall -TERM mysqld

kill -9 `cat /usr/local/apache2/logs/httpd.pid`

试试查杀进程PID

8.显示运行3级别开启的服务:
ls /etc/rc3.d/S* |cut -c 15-

(从中了解到cut的用途，截取数据)

9.如何在编写SHELL显示多个信息，用EOF
cat << EOF
+--------------------------------------------------------------+
| === Welcome to Tunoff services === |
+--------------------------------------------------------------+
EOF

10. for 的巧用(如给mysql建软链接)
cd /usr/local/mysql/bin
for i in *
do ln /usr/local/mysql/bin/$i /usr/bin/$i
done

11. 取IP地址：
ifconfig eth0|sed -n '2p'|awk '{print $2}'|cut -c 6-30

或者:
ifconfig eth0 |grep "inet addr:" |awk '{print $2}'|cut -c 6-

或者
ifconfig | grep 'inet addr:'| grep -v '127.0.0.1' | cut -d: -f2 | awk '{ print $1}'

或者：
ifconfig eth0 | sed -n '/inet /{s/.*addr://;s/ .*//;p}'

Perl实现获取IP的方法:
ifconfig -a | perl -ne 'if ( m/^s*inet (?:addr:)?([d.]+).*?cast/ ) { print qq($1n); exit 0; }'

12.内存的大小:
free -m |grep "Mem" | awk '{print $2}'

13.
netstat -an -t | grep ":80" | grep ESTABLISHED | awk '{printf "%s %sn",$5,$6}' | sort

14.查看Apache的并发请求数及其TCP连接状态：
netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'

15.因为同事要统计一下服务器下面所有的jpg的文件的大小,写了个shell给他来统计.原来用xargs实现,但他一次处理一部分,搞的有多个总和....,下面的命令就能解决啦.
find / -name *.jpg -exec wc -c {} ;|awk '{print $1}'|awk '{a+=$1}END{print a}'

CPU的数量（多核算多个CPU，
cat /proc/cpuinfo |grep -c processor

）越多，系统负载越低，每秒能处理的请求数也越多。

--------------------------------------------------------------------------------------------------------------------
16 CPU负载 # cat /proc/loadavg
检查前三个输出值是否超过了系统逻辑CPU的4倍。

18 CPU负载 #mpstat 1 1
检查%idle是否过低(比如小于5%)

19 内存空间 # free
检查free值是否过低 也可以用 # cat /proc/meminfo

20 swap空间 # free
检查swap used值是否过高 如果swap used值过高，进一步检查swap动作是否频繁：
# vmstat 1 5
观察si和so值是否较大

21 磁盘空间 # df -h
检查是否有分区使用率(Use%)过高(比如超过90%) 如发现某个分区空间接近用尽，可以进入该分区的挂载点，用以下命令找出占用空间最多的文件或目录：
# du -cks * | sort -rn | head -n 10

22 磁盘I/O负载 # iostat -x 1 2
检查I/O使用率(%util)是否超过100%

23 网络负载 # sar -n DEV
检查网络流量(rxbyt/s, txbyt/s)是否过高

24 网络错误 # netstat -i
检查是否有网络错误(drop fifo colls carrier) 也可以用命令：# cat /proc/net/dev

25 网络连接数目 # netstat -an | grep -E “^(tcp)” | cut -c 68- | sort | uniq -c | sort -n

26 进程总数 # ps aux | wc -l
检查进程个数是否正常 (比如超过250)

27 可运行进程数目 # vmwtat 1 5
列给出的是可运行进程的数目，检查其是否超过系统逻辑CPU的4倍

28 进程 # top -id 1
观察是否有异常进程出现

29 网络状态 检查DNS, 网关等是否可以正常连通

30 用户 # who | wc -l
检查登录用户是否过多 (比如超过50个) 也可以用命令：# uptime

31 系统日志 # cat /var/log/rflogview/*errors
检查是否有异常错误记录 也可以搜寻一些异常关键字，例如：
# grep -i error /var/log/messages
# grep -i fail /var/log/messages
# egrep -i 'error|warn' /var/log/messages 查看系统异常
32 核心日志 # dmesg
检查是否有异常错误记录

33 系统时间 # date
检查系统时间是否正确

34 打开文件数目 # lsof | wc -l
检查打开文件总数是否过多

35 日志 # logwatch ?print 配置/etc/log.d/logwatch.conf，将 Mailto 设置为自己的email 地址，启动mail服务 (sendmail或者postfix)，这样就可以每天收到日志报告了。
缺省logwatch只报告昨天的日志，可以用# logwatch ?print ?range all 获得所有的日志分析结果。
可以用# logwatch ?print ?detail high 获得更具体的日志分析结果(而不仅仅是出错日志)。

36.杀掉80端口相关的进程
lsof -i :80|grep -v "PID"|awk '{print "kill -9",$2}'|sh

37.清除僵死进程。
ps -eal | awk '{ if ($2 == "Z") {print $4}}' | kill -9

38.tcpdump 抓包 ，用来防止80端口被人攻击时可以分析数据
# tcpdump -c 10000 -i eth0 -n dst port 80 > /root/pkts

39.然后检查IP的重复数 并从小到大排序 注意 “-t +0″ 中间是两个空格
# less pkts | awk {‘printf $3″n”‘} | cut -d. -f 1-4 | sort | uniq -c | awk {‘printf $1″ “$2″n”‘} | sort -n -t +0

40.查看有多少个活动的php-cgi进程
netstat -anp | grep php-cgi | grep ^tcp | wc -l

41.利用iptables对应简单攻击
netstat -an | grep -v LISTEN | awk ‘{print $5}’ |grep -v 127.0.0.1|grep -v 本机ip|sed “s/::ffff://g”|awk ‘BEGIN { FS=”:” } { Num[$1]++ } END { for(i in Num) if(Num>8) { print i} }’ |grep ‘[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}’| xargs -i[] iptables -I INPUT -s [] -j DROP

Num>8部分设定值为阀值，这条句子会自动将netstat -an 中查到的来自同一ＩＰ的超过一定量的连接的列入禁止范围。 基中本机ip改成你的服务器的ip地址

42. 怎样知道某个进程在哪个CPU上运行？
# ps -eo pid,args,psr

43. 查看硬件制造商
dmidecode -s system-product-name

44.perl如何编译成字节码，这样在处理复杂项目的时候会更快一点？
perlcc -B -o webseek webseek.pl

45. 统计var目录下文件以M为大小,以列表形式列出来。
find /var -type f | xargs ls -s | sort -rn | awk ‘{size=$1/1024; printf(“%dMb %sn”, size,$2);}’ | head
查找var目录下文件大于100M的文件，并统计文件的个数
find /var -size +100M -type f | tee file_list | wc -l

46. sed 查找并替换内容
sed -i “s/varnish/LTCache/g” `grep “Via” -rl /usr/local/src/varnish-2.0.4`

sed -i “s/X-Varnish/X-LTCache/g” `grep “X-Varnish” -rl /usr/local/src/varnish-2.0.4`

47. 查看服务器制造商
dmidecode -s system-product-name

48. wget 模拟user-agent抓取网页
wget -m -e robots=off -U “Mozilla/5.0 (Windows; U; Windows NT 5.1; zh-CN; rv:1.9.1.6) Gecko/20091201 Firefox/3.5.6″ http://www.example.com/

50. 统计目录下文件的大小（按M打印显示）
du $1 –max-depth=1 | sort -n|awk ‘{printf “%7.2fM —-> %sn”,$1/1024,$2}’|sed ‘s:/.*/([^/]{1,})$:1:g’

51.关于CND实施几个相关的统计
统计一个目录中的目录个数
ls -l | awk ‘/^d/’ | wc -l

统计一个目录中的文件个数
ls -l | awk ‘/^-/’ | wc -l

统计一个目录中的全部文件数
find ./ -type f -print | wc -l

统计一个目录中的全部子目录数
find ./ -type d -print | wc -l

统计某类文件的大小:
find ./ -name “*.jpg” -exec wc -c {} ;|awk ‘{print $1}’|awk ‘{a+=$1}END{print a}’

53. 查找占用磁盘IO最多的进程
wget -c http://linux.web.psi.ch/dist/scientific/5/gfa/all/dstat-0.6.7-1.rf.noarch.rpm
dstat -M topio -d -M topbio

54. 去掉第一列（如行号代码）
awk ‘{for(i=2;i<=NF;i++) if(i!=NF){printf $i” “}else{print $i} }’ list

55.输出256中色彩
for
i in {0..255}; do echo -e “e[38;05;${i}m${i}”; done | column -c 80 -s ‘
‘; echo -e “e[m”

56.查看机器支持内存
机器插内存情况：
dmidecode |grep -P “Maximums+Capacity”

机器最大支持内存：
dmidecode |grep -P “Maximums+Capacity”

57.查看PHP-CGI占用的内存总数：
total=0; for i in `ps -C php-cgi -o rss=`; do total=$(($total+$i)); done; echo “PHP-CGI Memory usage: $total kb”

作者：NetSeek
来源：http://bbs.linuxtone.org/thread-16-1-1.html