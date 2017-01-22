# linux命令之cut

cut在文件中负责剪切数据用的，以每一行为一个处理对象，这个命令与awk中使用的print $N命令很相, 但是更受限. 在脚本中使用cut命令会比使用awk命令来得容易一些. 
cut -d ' ' -f2,3 filename等价于awk -F'[ ]' '{ print $2, $3 }' filename

### 剪切依据

cut命令主要是接受三个定位方法：
	
	第一，字节（bytes），用选项-b
	第二，字符（characters），用选项-c
	第三，域（fields），用选项-f
	
### 按字节
一个空格算一个字节，一个汉字算三个字节，多个定位之间用逗号隔开，cut会先把-b后面所有的定位进行从小到大排序，然后再提取
	
	# date |cut -b 10,1-7   
	2011年8
	date |cut -b -4,4-
	2011年08月11日 星期四21:06:53 EDT

	-4表示从第一个字节到第四个字节，而4-表示从第四个字节到行尾。这两种情况下，都包括了第4个字节“1”。如果我执行date |cut -b -4,4-，会输出整行，不会出现连续两个重叠的1
	
### 按字符

中文字符和空格都算一个字符。

	date |cut -c 5,9,13
	年月日
	
### 按域

-d指定域分隔符，-f 指定要剪出哪几个域，这个与awk的输出特定字段功能一样。
-d选项的默认间隔符就是制表符，所以当你就是要使用制表符的时候，完全就可以省略-d选项，而直接用－f来取域就可以了
 以/etc/passwd文件为例：

	# head -n5 /etc/passwd |cut -d : -f 1,3-5
	
	root:0:0:root
	bin:1:1:bin
	daemon:2:2:daemon
	adm:3:4:adm
	lp:4:7:lp
	
如何分的清空格和制表符？用sed命令可以让制表符原形毕露~

	# sed -n l test
	data11 data12\tdata13$
	data21 data22 data23$
	data31 data32    data33$
	
	# cat test |cut  -f 2
	data13
	data21   data22 data23
	data31 data32    data33
	
	
	
使用cut来获得所有mount上的文件系统的列表:	cut -d ' ' -f1,2 /etc/mtab