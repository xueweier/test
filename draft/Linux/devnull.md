
：>/dev/null 2>&1 的作用

    博客分类：
    Linux

 
shell中可能经常能看到：>/dev/null 2>&1

命令的结果可以通过%>的形式来定义输出

/dev/null 代表空设备文件
> 代表重定向到哪里，例如：echo "123" > /home/123.txt
1 表示stdout标准输出，系统默认值是1，所以">/dev/null"等同于"1>/dev/null"
2 表示stderr标准错误
& 表示等同于的意思，2>&1，表示2的输出重定向等同于1

那么本文标题的语句：
1>/dev/null 首先表示标准输出重定向到空设备文件，也就是不输出任何信息到终端，说白了就是不显示任何信息。
2>&1 接着，标准错误输出重定向等同于 标准输出，因为之前标准输出已经重定向到了空设备文件，所以标准错误输出也重定向到空设备文件。
A. 1> /dev/null 表示将命令的标准输出重定向到 /dev/null2>/dev/null 表示将命令的错误输出重定向到 /dev/null1 - denotes stdout ( standard output )2 - denotes stderr  ( standard error )/dev/null就相当与windows里的回收站，只是进去了不能再出来了。>/dev/null 就是将标准输出和标准出错的信息屏蔽不显示
B.>/dev/null 2>&1   also can write  as  1>/dev/null 2>&1     - stdout redirect to /dev/null (no stdout) ,and redirect stderr to stdout  (stderr gone as well) . end up it turns both stderr and stdout off
C.a little practice may help to undstand above .  #ls /usr  /nothing #ls /usr  /nothing  2>/dev/null #ls /usr  /nothing  >/dev/null 2>&1

我们经常会在UNIX系统下的一些脚本中看到类似”2>&1″这样的用法，例如“/path/to/prog 2>&1 > /dev/null &”，那么它的具体含义是什么呢？
　　UNIX有几种输入输出流，它们分别与几个数字有如下的对应关系：0-标准输入流(stdin)，1-标准输出流(stdout)，2-标准错误流 (stderr)。”2>&1″的意思就是将stderr重定向至stdout，并一起在屏幕上显示出来。如果不加数字，那么默认的重定向动作是针对stdout(1)的，比如”ls -l > result”就等价于”ls -l 1 > result”。这样便于我们更普遍性的理解重定向过程。
　　下面举例说明：
#cat std.sh
#!/bin/sh
echo “stdout”
echo “stderr” >&2

#/bin/sh std.sh 2>&1 > /dev/null
stderr

#/bin/sh std.sh > /dev/null 2>&1

　　第一条命令的输出结果是stderr，因为stdout和stderr合并后一同重定向到/dev/null，但stderr并未被清除，因此仍将在屏幕中显示出来；第二条命令无输出，因为当stdout重定向至/dev/null后，stderr又重定向到了stdout，这样stderr也被输出到了/dev/null。

今天在做例行工作的时候，发现机器上的sendmail进程奇多无比，并且机器IO好像也很慢。后来发现在/var/spool/clientmqueue目录下ls几乎要死人 – 最少有10万个文件

ps|grep sendmail看这些sendmail进程里面都有/var/spool/clientmqueue

cd过去随便打开了个文件看了下，发现是我crontab里面执行的程序的exception，估计是我的crontab每次执行，linux都试图发邮件给crontab的用户但是又没有配sendmail，所以东西就都被扔到/var/spool/clientmqueue下面了。然后我才明白为啥以前别人写的crontab要加上> /dev/null 2>&1，原来这样就不会每次执行crontab都把结果或者excetion发邮件了。

把这10万个文件删掉后，一切恢复正常

问题现象:
linux操作系统中的/var/spool/clientmqueue/目录下存在大量文件。
原因分析：系统中有用户开启了cron，而cron中执行的程序有输出内容，输出内容会以邮件形式发给cron的用户，而sendmail没有启动所以就产生了这些文件；
解决办法: 1、 将crontab里面的命令后面加上> /dev/null 2>&1
2、知识点：
2>：重定向错误。
2>&1：把错误重定向到输出要送到的地方。即把上述命令的执行结果重定向到/dev/null，即抛弃，同时，把产生的错误也抛弃。
3、具体代码：
（1）、# crontab -u cvsroot -l
01 01 * * * /opt/bak/backup
01 02 * * * /opt/bak/backup2
（2）、# vi /opt/bak/backup
#!/bin/sh
cd /
getfacl -R repository > /opt/bak/backup.acl
（3）、# vi /opt/bak/backup2
#!/bin/sh
week=`date +%w`
tar zcvfp /opt/bak/cvs$week/cvs.tar.gz /repository >/dev/null 2>&1
4、清除/var/spool/clientmqueue/目录下的文件：
# cd /var/spool/clientmqueue
# rm -rf *
如果文件太多，占用空间太大，用上面命令删除慢的话，就执行下面的命令：
# cd /var/spool/clientmqueue
# ls | xargs rm -f
在一個風和日麗的夜晚，我坐在家裡看著電視，後來手機一陣響起，結果是楊老師發現一台主機發生異常，伺服器的 /var/spool/mqueue 目錄被塞了一堆還沒有寄出的信件，而當時沒有把 /var/spool 另外分割出來，所以也影響到了系統 root (/) 區塊，只剩六百多 MB 可以使用，這時一想會有幾個可能.

這台 server 有幫學校的 PC 做寄送信件，所以可能是廣告信在寄出.

使用這台 server 做 mail 寄信的機器，可能是中毒，於是就不斷的送信出去.
一開始只有想到這兩個原因，但是可要把被吞掉的空間給吐出來，所以就打算把所有的 mail queue 都先砍了，當然，要先停掉 mail service.
在砍這些正在排隊的信件時，發現一件事，就是裡面的檔案太多了，使用 ls 命令就變得超級遲頓，沒有反應，使用 mailq 來看看到底是那些信被 queue 住也沒辦法，後來想想算了，只好全剖砍了，不要再玩下去，之後，很順手的下了 rm -rf * 這下子呢，發生了一件很離奇的事，居然檔案太多無法刪除，第一次聽到 rm 在 complain (我是聽到的，楊老師是實作者，所以他有看到 ^^).
那個 error 是: bash: /bin/rm: Argument list too long
雖然無法刪除，但是楊兄並不放棄，到主機面前，開啟了 X Window 之後使用那 Linuxer 最常使用的鸚鵡螺 (nautilus) 開啟到 /var/spool/mqueue. 喔 ~ 可以使用 X Window 來刪呢 ! 後來想說即然 X Window 有這麼大的本事，那麼就用它來刪了其它的 queue files 就好啦，於是掛上電話，放楊兄一個人努力的在機房刪著 ...
當然我也沒有閒著，電視劇剛好演完，於是開啟我的工作伙伴，再度當網路潛水艇 ... 游著游著，突然想到，何不使用 find 來刪除看看 ? 於是刪回歷史文件，發現一個命令就是 find ./ | xargs rm -rf 千萬別小看這小小的指令，因為在我看完之後不久，楊兄打進來，說已經刪到手軟，這時也是晚上十點了，於是我就推薦了這個這道指令，嗯，很好，全都刪了，還頗快的 ...
喔，還沒說為什麼會刪到手軟，是因為 nautilus 在 Load 目錄時，是分批的，不是一次全部讀，所以一次大約是幾千封在讀，刪了之後，沒想到又冒出了還有幾千封 ... 真是嚇死人，後來推論應該是分批的關係.
在下了 find ./ | xargs rm -rf 之後，還在訝異快速之餘，就發現時間不多了，學校也要關門，所以就先 say bye bye，在現場苦命的楊兄也回家休息了.
分析:
rm 有最大一次刪除的數量，所以當一個目錄裡有太多的檔案或目錄時，就會出現錯誤，小弟試過應該是在二萬以下，而使用 find ./ | xargs rm -rf 的目的是先使用 find 列出檔案，再導向到 xargs，xargs 再喂給 rm，在這裡，xargs 會分批依照 rm 的最大數量餵給 rm，然後就可以順利刪除檔案了
。而真正的原因，有可能是 rm 的版本或是檔案系統的問題，我也不再繼續追就，反正能辦好事就好
下面提供當時小弟測試的一個小小 shell script
下載：
mk-file.sh
(這個 shell script 會有目錄下產生 20000 個檔案。)
接下來來做個小小測試：
root # mkfile.sh
root #
會產生 20000 個小檔案，名稱為 test-file-{1~19999}
直接使用 rm 去刪除：
root # rm -rf test-file-*
-bash: /bin/rm: Argument list too long (會回應引數過長的訊息)
改搭配 find 來刪除
root # find ./ -iname 'test-file-*' | xargs rm -rf
root # ls
mk-file.sh
root #
這樣就順利被刪除了。

---------------------------------
#tool_action
45 4 * * * /bin/sh /data/stat/crontab/exec_tool_action_analysis_db.sh >> /data/stat/logs/exec_tool_action_analysis_db.sh.log > /dev/null 2>&1

45 5 * * * /bin/sh /data/stat/crontab/exec_tool_action_analysis_user.sh >> /data/stat/logs/exec_tool_action_analysis_user.sh.log > /dev/null 2>&1

否则在/var/spool/clientmqueue 下会产生以下文件：
-rw-rw---- 1 smmsp   smmsp  975 Jan 17 10:50 qfq0H2o4ei031197

----------------------------------------------------------
#
40 3 * * * sh /work/yule/shell/export_anchor_multi_4_mysql.sh > /work/yule/logs/export_anchor_multi_4_mysql.log 2>&1 




 2>&1使用

2>&1使用

一 相关知识

1）默认地，标准的输入为键盘，但是也可以来自文件或管道（pipe |）。
2）默认地，标准的输出为终端（terminal)，但是也可以重定向到文件，管道或后引号（backquotes `）。
3) 默认地，标准的错误输出到终端，但是也可以重定向到文件。
4）标准的输入，输出和错误输出分别表示为STDIN,STDOUT,STDERR，也可以用0，1，2来表示。
5）其实除了以上常用的3中文件描述符，还有3~9也可以作为文件描述符。3~9你可以认为是执行某个地方的文件描述符，常被用来作为临时的中间描述符。


二 实例

1）command 2>errfile : command的错误重定向到文件errfile。
2）command 2>&1 | ...: command的错误重定向到标准输出，错误和标准输出都通过管道传给下个命令。
3）var=`command 2>&1`: command的错误重定向到标准输出，错误和标准输出都赋值给var。
4）command 3>&2 2>&1 1>&3 | ...:实现标准输出和错误输出的交换。
5）var=`command 3>&2 2>&1 1>&3`:实现标准输出和错误输出的交换。
6）command 2>&1 1>&2 | ...     (wrong...) :这个不能实现标准输出和错误输出的交换。因为shell从左到右执行命令，当执行完2>&1后，错误输出已经和标准输出一样的，再执行1>&2也没有意义。


三 "2>&1 file"和 "> file 2>&1"区别

1）cat food 2>&1 >file ：错误输出到终端，标准输出被重定向到文件file。
2）cat food >file 2>&1 ：标准输出被重定向到文件file，然后错误输出也重定向到和标准输出一样，所以也错误输出到文件file。


四 注意
通常打开的文件在进程推出的时候自动的关闭，但是更好的办法是当你使用完以后立即关闭。用m<&-来关闭输入文件描述符m，用m>&-来关闭输出文件描述符m。如果你需要关闭标准输入用<&-; >&- 被用来关闭标准输出。


五 同时输出到终端和文件 copy source dest | tee.exe copyerror.txt


六 参考

1）http://docstore.mik.ua/orelly/unix/upt/ch45_21.htm
2）http://www.unix.com/shell-programming-scripting/34011-meaning-dev-null-2-1-a.html
3）http://docstore.mik.ua/orelly/unix/upt/ch08_13.htm
