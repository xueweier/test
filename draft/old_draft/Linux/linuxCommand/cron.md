
linux计划任务之crontab
2014-01-05 23:03 1421人阅读 评论(0) 收藏 举报
语法：
       crontab [ -u user ] file
       crontab [ -u user ] [ -i ] { -e | -l | -r }

说明：

      crontab命令是为个人用户用于设置周期性被执行的指令。每一个用户都可以有自己的crontab。

      如果/etc/cron.allow文件存在，该文件中所列用户允许使用crontab命令。

      如果/etc/cron.allow文件不存在，而/etc/cron.deny文件存在，该文件中所列用户不允许使用crontab命令。

      如果 /etc/cron.allow和 /etc/cron.deny 都不存在，   根据配置参数的不同，仅有超级用户允许使用这个命令或者所有的用户都允许使用。

      如果 /etc/cron.allow和 /etc/cron.deny 都存在，/etc/cron.allow的优先级大于/etc/cron.deny，因此/etc/cron.deny可以被忽略，但是如果想使用此命令，必须在/etc/cron.allow中明确列出用户。

      /var/spool/cron/，所有用户crontab文件存放的目录,以用户名命名。

参数：

     -u：用来设定某个用户的crontab服务，例如，“-u liujl”表示设定liujl用户的crontab服务，此参数一般有root用户来运行。

     -l：显示某个用户的crontab文件内容，如果不指定用户，则表示显示当前用户的crontab文件内容。

     -r：从/var/spool/cron目录中删除某个用户的crontab文件，如果不指定用户，则默认删除当前用户的crontab文件。

     -e：编辑某个用户的crontab文件内容。如果不指定用户，则表示编辑当前用户的crontab文件。

     -i：在删除用户的crontab文件时给确认提示。

     file：file是命令文件的名字,表示将file做为crontab的任务列表文件并载入crontab。如果在命令行中没有指定这个文件，crontab命令将接受标准输入（键盘）上键入的命令，并将它们载入crontab。



crontab使用方法：

     crontab每项（行）工作的格式：


意义    分钟    小时    日  月  星期    命令
范围    0-59    0-23    1-31    1-12    0-6     执行的命令


在使用时，会用到几种辅助字符，先说明如下：

特殊字符


意义

*


代表任意匹配，例如“** * * /bin/execute/this/script.sh”代表：每一分钟，每一小时，每天，每月，一周的每一天都执行/bin/execute/this/script.sh这个命令，简单的说是：每一分钟都执行此命令，没有例外。

，


代表分割时段，比如每10分钟执行一次命令？可以使用

0，10，20，30，40，50* * * * command

参数栏数不变，但是第一栏是0，10，20，30，40，50，以逗号（，）分割。

-


代表一段时间范围内，比如工作日（周一至周五）凌晨1点执行某一个命令？

*1 * * 1-5 command

第五栏为1-5，代表1，2，3，4，5都适用的意思。

/n


n代表数字，为每隔n单位间隔。例如上文逗号（，）中举例，每10分钟执行一次命令，可以这样写：

*/10* * * * command

第一栏为*/10，不要忘了*不能省略。


1). 创建一个新的crontab文件

在考虑向cron进程提交一个crontab文件之前，首先要做的一件事情就是设置环境变量EDITOR。cron进程根据它来确定使用哪个编辑器编辑crontab文件。99%的UNIX和LINUX用户都使用vim，如果你也是这样，那么你就编辑$HOME目录下的. profile文件，在其中加入这样一行：EDITOR=vim; export EDITOR

然后保存并退出。不妨创建一个名为<user> cron的文件，其中<user>是用户名，例如， davecron。在该文件中加入如下的内容。

      # (put your own initials here)echo the date to the console every

      # 15minutes between 6pm and 6am

      0,15,30,45 18-06 * * * /bin/echo 'date' > /dev/console

    保存并退出。确信前面5个域用空格分隔。

在上面的例子中，系统将每隔1 5分钟向控制台输出一次当前时间。如果系统崩溃或挂起，从最后所显示的时间就可以一眼看出系统是什么时间停止工作的。在有些系统中，用tty1来表示控制台，可以根据实际情况对上面的例子进行相应的修改。为了提交你刚刚创建的crontab文件，可以把这个新创建的文件作为cron命令的参数：

     $ crontab davecron

现在该文件已经提交给cron进程，它将每隔1 5分钟运行一次。

同时，新创建文件的一个副本已经被放在/var/spool/cron目录中，文件名就是用户名(即dave)。

2). 列出crontab文件

   为了列出crontab文件，可以用：

     $ crontab -l

     0,15,30,45,18-06 * * * /bin/echo `date` > dev/tty1

你将会看到和上面类似的内容。可以使用这种方法在$ H O M E目录中对crontab文件做一备份：

     $ crontab -l > $HOME/mycron

    这样，一旦不小心误删了crontab文件，可以用上一节所讲述的方法迅速恢复。

3). 编辑crontab文件

   如果希望添加、删除或编辑crontab文件中的条目，而EDIT环境变量又设置为vim，那么就可以用vim来编辑crontab文件，相应的命令为：

     $ crontab -e

可以像使用v i编辑其他任何文件那样修改crontab文件并退出。如果修改了某些条目或添加了新的条目，那么在保存该文件时， c r o n会对其进行必要的完整性检查。如果其中的某个域出现了超出允许范围的值，它会提示你。

我们在编辑crontab文件时，没准会加入新的条目。例如，加入下面的一条：

    # DT:delete core files,at 3.30am on 1,7,14,21,26,26 days of each month

     30 3 1,7,14,21,26 * * /bin/find -name "core' -exec rm {} \;

现在保存并退出。最好在crontab文件的每一个条目之上加入一条注释，这样就可以知道它的功能、运行时间，更为重要的是，知道这是哪位用户的作业。

现在让我们使用前面讲过的crontab -l命令列出它的全部信息：

    $ crontab -l

    # (crondave installed on Tue May 4 13:07:43 1999)

    # DT:ech the date to the console every 30 minites

   0,15,30,45 18-06 * * * /bin/echo `date` > /dev/tty1

    # DT:delete core files,at 3.30am on 1,7,14,21,26,26 days of each month

    30 3 1,7,14,21,26 * * /bin/find -name "core' -exec rm {} \;

4). 删除crontab文件

要删除crontab文件，可以用：

    $ crontab -r

5). 恢复丢失的crontab文件

如果不小心误删了crontab文件，假设你在自己的$

HOME目录下还有一个备份，那么可以将其拷贝到/var/spool/cron/<username>，其中<username>是用户名。如果由于权限问题无法完成拷贝，可以用：

$ crontab <filename>

其中，<filename>是你在$HOME目录中副本的文件名。

我建议你在自己的$HOME目录中保存一个该文件的副本。我就有过类似的经历，有数次误删了crontab文件（因为r键紧挨在e键的右边）。这就是为什么有些系统文档建议不要直接编辑crontab文件，而是编辑该文件的一个副本，然后重新提交新的文件。

有些crontab的变体有些怪异，所以在使用crontab命令时要格外小心。如果遗漏了任何选项，crontab可能会打开一个空文件，或者看起来像是个空文件。这时敲delete键退出，不要按<Ctrl-D>，否则你将丢失crontab文件。

简单的例子：
● 0 */2 * * * /sbin/service httpd restart

每两个小时重启一次apache

● 50 7 * * * /sbin/service sshd start

每天7：50开启ssh服务

● 50 22 * * * /sbin/service sshd stop

每天22：50关闭ssh服务

● 0 0 1,15 * * fsck /home

每月1号和15号检查/home 磁盘

● 1 * * * * /home/bruce/backup

每分钟都执行 /home/bruce/backup这个文件

● 00 03 * * 1-5 find /home "*.xxx" -mtime +4 -exec rm {} \;

每周一至周五凌晨3点，在目录/home中，查找文件名为*.xxx的文件，并删除4天前的文件。
● 30 6 */10 * * ls

每月的1、11、21、31日是的6：30执行一次ls命令
