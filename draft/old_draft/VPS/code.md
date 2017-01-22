# 常用vps指令

### 软件包
	dpkg -l
    dpkg -L gedit 查询安装路径
    whereis gedit 查询安装路径

### 系统备份
        du --max-depth=1 -h 
        df -sh
	tar cvpzf backup.tgz --exclude=/proc --exclude=/lost+found --exclude=/backup.tgz --exclude=/mnt --exclude=/sys /
        sysv-rc-conf /*rc启动一览*/
	
### 日常操作
	find . -name 'hello.txt' -print
	whereas denyhosts.conf
	chown -R www-data:www-data wordpress
	rm -rf mydir /*删除mydir目录，不需要确认，直接删除*/
	ln -s tool bac 
	cp -a tool /home/vpser/www /*把tool目录，复制到www目录下 */
	wget -c http://soft.vpser.net/web/nginx/nginx-0.8.0.tar.gz /* 继续下载上次未下载完的文件 */


### 资源占用查询
	free -m /* 查看内存核swap使用情况 */
	top /* 查看程序的cpu、内存使用情况 */
        vmstat
        mpstat
	netstat -ntl /* 查看端口占用情况 */
	df -h              /* 查看磁盘剩余空间 */
	find /home/kelu -printf "%k %p\n" | sort -g -k 1,1 | \ awk '{if($1 > 500000) print $1/1024 "MB" " " $2 }' |tail -n 40
	找出某个目录下（这里是 /home/kelu）大小超过 500MB 的文件（打印前40行并按照 MB 从小到大排列）
	scp -P 2222 -r /home/lnmp0.4/ root@www.vpser.net:/root/lnmp0.4/ /*将本地目录上传到服务器上*/
	tail -F filename  /*跟踪文件变化*/

### 进程管理
	ps -aux   /*ps 进程状态查询命令*/
	kill 1234    /*1234为进程ID，即ps -aux 中的PID*/
	killall nginx /*killall 通过程序的名字，直接杀死所有进程，nginx为进程名*/
	
    
&copy;kelu.org

