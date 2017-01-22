# linux命令之watch

如果您需要监控一个命令进行的运行结果，你会怎么做？一遍一遍地执行命令查看结果的不同或使用一个shell脚本来实现。linux watch命令就可以实现,帮你监测一个命令的运行结果。


直接在 watch 后面接想运行的命令, watch 就会帮您重复运行, 并把每次的结果都更新在屏幕上.  watch 命令默认会以 2s 的间隔重复运行命令.

	-n 或–interval参数指定时间间隔.
	-d, –differences[=cumulative]       高亮显示变动,-d=cumulative选项会把变动过的地方(不管最近的那次有没有变动)都高亮显示出来.
	-n, –interval=<seconds>              周期(秒)
	-t 或-no-title  会关闭watch命令在顶部的时间间隔

	watch -n 1 "ifconfig eth0"
	watch -n 1 "/sbin/ifconfig eth0 | grep bytes"

	#watch uptime
	#watch -t uptime
	#watch -d -n 1 netstat -ntlp
	#watch -d ’ls -l | fgrep goface’   //监测goface的文件
	#watch -t -differences=cumulative uptime
	#watch -n 60 from  //监控mail
	#watch -n 1 ”df -i;df”  //监测磁盘inode和block数目变化情况
	
		
		
	实例1：
	命令：每隔一秒高亮显示网络链接数的变化情况
	watch -n 1 -d netstat -ant
	说明：
	其它操作：
	切换终端： Ctrl+x
	退出watch：Ctrl+g
	实例2：每隔一秒高亮显示http链接数的变化情况
	命令：
	watch -n 1 -d 'pstree|grep http'
	说明：
	每隔一秒高亮显示http链接数的变化情况。 后面接的命令若带有管道符，需要加''将命令区域归整。
	实例3：实时查看模拟攻击客户机建立起来的连接数
	命令：
	watch 'netstat -an | grep:21 | \ grep<模拟攻击客户机的IP>| wc -l' 
	说明：
	实例4：监测当前目录中 scf' 的文件的变化
	命令：
	watch -d 'ls -l|grep scf' 
	实例5：10秒一次输出系统的平均负载
	命令：
	watch -n 10 'cat /proc/loadavg'