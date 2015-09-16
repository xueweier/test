# linux命令之sed

sed编辑器逐行处理输入，然后把结果发送到屏幕。



### sed命令

	a\	在当前行后添加一行或多行
	c\	用新文本替换当前行中的文本
	d	删除行
	i\	在当前行之前插入文本
	h	把模式空间的内容复制到暂存缓冲区
	H	把模式空间的内容添加到缓冲区
	g	取出暂存缓冲区的内容，将其复制到模式缓冲区
	G	取出暂存缓冲区的内容，将其追加到模式缓冲区
	l	列出非打印字符
	p	打印行
	n	读入下一行输入，并从下一条而不是第一条命令对其处理
	q	结束或退出sed
	r	从文件中读取输入行
	!	对所选行以外的行应用所有命令
	s	用一个字符串替换另外一个字符串

	替换标志：
	g	在行内进行全局替换
	p	打印行
	w	将行写入文件
	x	交换暂存缓冲区和模式空间的内容
	y	将字符转换成另外一个字符
 
### sed选项

	-i	 直接作用源文件，源文件将被修改。
	-e	 进行多项编辑，即对输入行应用多条sed命令时使用
	-n	 取消默认的输出
	-f	 指定sed脚本的文件名
 

sed例子：

打印：p命令

	sed ‘/abc/p’ file	
	打印file中包含abc的行。默认情况sed把所有行都打印到屏幕，如果某行匹配到模式，则把该行另外再打印一遍
	sed  -n ‘/abc/p’ file	和上面一样，只是去掉了sed的默认行为，只会打印匹配的行
	
删除：d命令

	sed ‘$d’ file	删除最后一行的内容
	sed ‘/abc/d’	删除包含abc的行。
	sed ‘3d’ file	删除第三行的内容

查找
 
替换：s命令

	sed  ‘s/abc/def/g’ file	把行内的所有abc替换成def，如果没有g,则只替换行内的第一个abc
	sed  -n ‘s/abc/def/p’ file	只打印发生替换的那些行
	sed  ‘s/abc/&def/’ file	在所有的abc后面添加def（&表示匹配的内容）
	sed  -n ‘s/abc/def/gp’ file	把所有的abc替换成def，并打印发生替换的那些行
	sed  ‘s#abc#def#g’ file	把所有的abc替换成def，替换串之间的分割字符
 
指定行的范围：逗号

	sed  -n ‘/abc/,/def/p’ file	打印模式abc到def的行
	sed  -n ‘5/,/def/p’ file	打印从第五行到包含def行之间的行。
	sed /abd/,/def/s/aaa/bbb/g	修改从模式abc到模式def之间的行，把aaa替换成def

多重编辑-e

	sed  -e ‘1,3d’ -e ‘s/abc/def/g’ file	删除1-3行，然后把其余行的abc替换成def
 
读文件：r命令

	sed  ‘/abc/r newfile’ file	在包含abc的行后读入newfile的内容
 
写文件：w命令  

	sed  ‘/abc/w newfile’ file	在包含abc的行写入newfile
 
追加：a命令

	sed  ‘/abc/a\def’ file	在包含abc的行后新起一行，写入def
 
插入：i命令     
	sed  ‘/abc/i\def’ file	在包含abc的行前新起一行，写入def
 
修改：c命令   
	
	sed  ‘/abc/c\def’ file	在包含abc的行替换成def，旧文本被覆盖
 
读取下一行：n命令  
	
	sed  ‘/abc/{n ; s/aaa/bbb/g;}’ file	读取包含abc的行的下一行，替换aaa为bbb
 
转换：y命令       
	
	sed  ‘y/abc/ABC’ file	将a替换成A，b替换成B，c替换成C（正则表达式元字符不起作用）
 
退出：q命令   

	sed  ‘/abc/{ s/aaa/bbb/ ;q; }’ file	在某行包含了abc，把aaa替换成bbb，然后退出sed。

暂存和取用：h命令（把模式行存储到暂存缓冲区）和g（取出暂存缓冲区的行并覆盖模式缓冲区）G（取出临时缓冲区的行）命令 

 

h和g是复制行为(覆盖），H和G表示追加。

	sed  -e ‘/abc/h’  -e ‘$G’ file	包含abc的行通过h命令保存到暂存缓冲区，在第二条命令汇中，sed读到最后一行$时，G命令从暂存缓冲区中读取一行，追加到模式缓冲区的后面。即所有包含abc的行的最后一行被复制到文件末尾。
	sed -e ‘/abc/{h; d;}’ 
      -e  ‘/def/{g; }’ file	包含abc的行会移到包含def的行上，并进行覆盖。
 
暂存和互换：h和x命令

	sed  -e ‘/abc/h’  
	-e ‘/def/x’ file	包含abc的行会被换成def的行。
	
	
a 追加内容 sed ‘/匹配词/a\要加入的内容’ example.file（将内容追加到匹配的目标行的下一行位置）
i 插入内容 sed ‘/匹配词/i\要加入的内容’ example.file 将内容插入到匹配的行目标的上一行位置）
示例：

	1	#行前加
	2	sed -i '/allow chengyongxu.com/i\allow chengyongxu.cn' the.conf.file
	3	#行前后
	4	sed -i '/allow chengyongxu.com/a\allow chengyongxu.cn' the.conf.file
	---------------------------------------------------
	1、删除指定行的上一行
	sed -i -e :a -e '$!N;s/.*\n\(.*ServerName abc.com\)/\1/;ta' -e 'P;D' $file
	2、删除指定字符串之间的内容
	sed -i '/ServerName abc.com/,/\/VirtualHost/d' $file
  
sed命令的调用:
    在命令行键入命令;将sed命令插入脚本文件,然后调用sed;将sed命令插入脚本文件,并使sed脚本可执行
    sed [option] sed命令 输入文件            在命令行使用sed命令,实际命令要加单引号
    sed [option] -f sed脚本文件 输入文件     使用sed脚本文件
    sed脚本文件 [option] 输入文件            第一行具有sed命令解释器的sed脚本文件
    option如下: 
      n 不打印; sed不写编辑行到标准输出,缺省为打印所有行(编辑和未编辑),p命令可以用来打印编辑行
      c 下一命令是编辑命令,使用多项编辑时加入此选项
      f 如果正在调用sed脚本文件,使用此选项,此选项通知sed一个脚本文件支持所用的sed命令,如
          sed -f myscript.sed input_file  这里myscript.sed即为支持sed命令的文件
    使用重定向文件即可保存sed的输出

使用sed在文本中定位文本的方式:
    x       x为一行号,比如1
    x,y     表示行号范围从x到y,如2,5表示从第2行到第5行
    /pattern/    查询包含模式的行,如/disk/或/[a-z]/
    /pattern/pattern/   查询包含两个模式的行,如/disk/disks/
    /pattern/,x  在给定行号上查询包含模式的行,如/disk/,3
    x,/pattern/  通过行号和模式查询匹配行,如 3,/disk/
    x,y!    查询不包含指定行号x和y的行

基本sed编辑命令:
    p      打印匹配行                      c/    用新文本替换定位文本
    =      显示文件行号                    s     使用替换模式替换相应模式
    a/     在定位行号后附加新文本信息        r     从另一个文本中读文本
    i/     在定位行号后插入新文本信息        w     写文本到一个文件
    d      删除定位行                      q     第一个模式匹配完成后退出或立即退出
    l      显示与八进制ASCII代码等价的控制字符        y  传送字符
    n      从另一个文本中读文本下一行,并附加在下一行   {}     在定位行执行的命令组
    g      将模式2粘贴到/pattern n/

基本sed编程举例:
    使用p(rint)显示行: sed -n '2p' temp.txt   只显示第2行,使用选项n
    打印范围:  sed -n '1,3p' temp.txt         打印第1行到第3行
    打印模式:  sed -n '/movie/'p temp.txt     打印含movie的行
    使用模式和行号查询:  sed -n '3,/movie/'p temp.txt   只在第3行查找movie并打印
    显示整个文件:  sed -n '1,$'p temp.txt      $为最后一行
    任意字符:  sed -n '/.*ing/'p temp.txt     注意是.*ing,而不是*ing
    打印行号:  sed -e '/music/=' temp.txt
    附加文本:(创建sed脚本文件)chmod u+x script.sed,运行时./script.sed temp.txt
        #!/bin/sed -f
        /name1/ a/             #a/表示此处换行添加文本
        HERE ADD NEW LINE.     #添加的文本内容
    插入文本: /name1/ a/ 改成 4 i/ 4表示行号,i插入
    修改文本: /name1/ a/ 改成 /name1/ c/ 将修改整行,c修改
    删除文本: sed '1d' temp.txt  或者 sed '1,4d' temp.txt
    替换文本: sed 's/source/OKSTR/' temp.txt     将source替换成OKSTR
             sed 's//$//g' temp.txt             将文本中所有的$符号全部删除
             sed 's/source/OKSTR/w temp2.txt' temp.txt 将替换后的记录写入文件temp2.txt
    替换修改字符串: sed 's/source/"ADD BEFORE" &/p' temp.txt
             结果将在source字符串前面加上"ADD BEFORE",这里的&表示找到的source字符并保存
    sed结果写入到文件: sed '1,2 w temp2.txt' temp.txt
                     sed '/name/ w temp2.txt' temp.txt
    从文件中读文本: sed '/name/r temp2.txt' temp.txt
    在每列最后加文本: sed 's/[0-9]*/& Pass/g' temp.txt
    从shell向sed传值: echo $NAME | sed "s/go/$REP/g"   注意需要使用双引号

快速一行命令:
    's//.$//g'         删除以句点结尾行
    '-e /abcd/d'       删除包含abcd的行
    's/[][][]*/[]/g'   删除一个以上空格,用一个空格代替
    's/^[][]*//g'      删除行首空格
    's//.[][]*/[]/g'   删除句号后跟两个或更多的空格,用一个空格代替
    '/^$/d'            删除空行
    's/^.//g'          删除第一个字符,区别  's//.//g'删除所有的句点
    's/COL/(.../)//g'  删除紧跟COL的后三个字母
    's/^////g'         删除路径中第一个/

///////////////////////////////////////////////////////////////////////

、使用句点匹配单字符    句点“.”可以匹配任意单字符。“.”可以匹配字符串头，也可以是中间任意字符。假定正在过滤一个文本文件，对于一个有1 0个字符的脚本集，要求前4个字符之后为X C，匹配操作如下：. . . .X C. . . .
2、在行首以^匹配字符串或字符序列    ^只允许在一行的开始匹配字符或单词。在行首第4个字符为1，匹配操作表示为：^ . . . 1
3、在行尾以$匹配字符串或字符    可以说$与^正相反，它在行尾匹配字符串或字符， $符号放在匹配单词后。如果在行尾匹配单词j e t 0 1，操作如下：j e t 0 1 $    如果只返回包含一个字符的行，操作如下：^ . $
4、使用*匹配字符串中的单字符或其重复序列    使用此特殊字符匹配任意字符或字符串的重复多次表达式。
5、使用/屏蔽一个特殊字符的含义    有时需要查找一些字符或字符串，而它们包含了系统指定为特殊字符的一个字符。如果要在正则表达式中匹配以* . p a s结尾的所有文件，可做如下操作：/ * / . p a s
6、使用[]匹配一个范围或集合     使用[ ]匹配特定字符串或字符串集，可以用逗号将括弧内要匹配的不同字符串分开，但并不强制要求这样做（一些系统提倡在复杂的表达式中使用逗号），这样做可以增 加模式的可读性。使用“ -”表示一个字符串范围，表明字符串范围从“ -”左边字符开始，到“ -”右边字符结束。假定要匹配任意一个数字，可以使用：[ 0 1 2 3 4 5 6 7 8 9 ]    要匹配任意字母，则使用：[ A - Z a - z ]表明从A - Z、a - z的字母范围。
7、使用/{/}匹配模式结果出现的次数    使用*可匹配所有匹配结果任意次，但如果只要指定次数，就应使用/ { / }，此模式有三种形式，即：
    pattern/{n/} 匹配模式出现n次。
    pattern/{n,/} 匹配模式出现最少n次。
    pattern/{n,m} 匹配模式出现n到m次之间，n , m为0 - 2 5 5中任意整数。
    匹配字母A出现两次，并以B结尾，操作如下：A / { 2 / } B匹配值为A A B    匹配A至少4次，使用：A / { 4 , / } B
    
    
    ===============
替换单引号为空：

可以这样写：
sed 's/'"'"'//g' 

sed 's/'\''//g'

sed s/\'//g





一、初识sed

在部署openstack的过程中，会接触到大量的sed命令，比如

# Bind MySQL service to all network interfaces.
sed -i 's/127.0.0.1/0.0.0.0/g' /etc/mysql/my.cnf
那么这条命令是什么意思？接下来介绍sed命令答案自然就揭晓了。

二、sed简介

sed：是一个编辑器，是一个强大的文件处理工具。

sed作用：用来替换、删除，更新文件中的内容。sed能自动处理一个或多个文件。

sed原理：sed以文本的行为单位进行处理，一次处理一行内容。首先sed把当前处理的行存储在临时缓冲区中（称为模式空间pattern space），接着处理缓冲区中的行，处理完成后，把缓冲区的内容送往屏幕。sed处理完一行就将其从临时缓冲区删除，然后将下一行读入，进行处理和显示，这样不断的重复，直到文件末尾。处理完文件的最后一行后，sed便结束运行。

因为sed是对文件中每行在临时缓冲区中的副本进行编辑，所以原文件内容并没有改变，除非重定向输出。

三、sed命令介绍

#sed [-nefri][命令]

参数说明：

-i:直接修改文件，终端不输出结果。

-n:使用安静（slient）模式，取消默认输出。sed默认会将所有来自stdin的数据输出到终端上。但如果加上-n参数后，不自动打印处理后的结果，只是默默的处理，只有经过sed特殊处理的那一行才被列出来。

-e: --expression直接在命令模式上进行sed的动作编辑。sed -e '...' -e '...' -e '...'

-f:指定sed脚本的文件名。

-r:sed动作支持的是延伸型正规表示法的语法。（默认是基础正规表示法语法）

命令说明：[n1][n2]命令

n1,n2：表示行号，该参数可选，一般表示希望操作的行数，可以是数字，正则表达式或二者混合。

用逗号分隔的两个行数表示这两行为起止的行的范围。如1，3表示1,2,3行，美元符号（$）表示最后一行。如何没有指定地址，sed将处理输入文件的所有行。地址通常的写法有：n;n,m;n,$。举例，如果我的操作是需要在3到5行之间进行的，则【3,5,[动作行为]】。

命令:

a:新增，在当前行的下一行追加一行文本。

i:插入，在当前行的上一行插入一行文本。

c:替换，以行为单位进行替换，c的后面可以跟字符串，用这些字符串取代n1,n2之间的行。

d:删除，从模式空间删除一行。

p:打印，打印模式空间的行。通常p会与参数【-n】一起使用。

s:替换，通常s命令可以搭配正则表达式！例如1,20s/old/new/g。

四、举例

下面我们拿Emilia的Big Big World中的部分歌词文件song.txt举例，其内容如下：

$ cat song.txt
I'm a big big girl
In a big big world
It's not a big big thing if you leave me
But I do do feel
that I do do will miss you much
Miss you much
1、s替换命令

将song.txt文件中每行的第一个big替换为small:

sed 's/big/small/' song.txt

解释：s：替换命令，/big/:匹配big字符串，/small/：把匹配替换成small。

$ sed 's/big/small/' song.txt
I'm a small big girl
In a small big world
It's not a small big thing if you leave me
But I do do feel
that I do do will miss you much
Miss you much
将song.txt文件中每行所有的big替换为small:

sed 's/big/small/g' song.txt

解释：同上，/g表示一行上替换所有的匹配

$ sed 's/big/small/g' song.txt
I'm a small small girl
In a small small world
It's not a small small thing if you leave me
But I do do feel
that I do do will miss you much
Miss you much
Note:这里也可以使用 sed 's#big#small#g' song.txt  ，紧跟s命令的符号都会被认为是查找串和替换串之间的分隔符，这里使用#，其实无论什么字符串（换行符，反斜线除外），只要紧跟s命令，就成了新的串分隔符。

将song.txt文件中每行第2个big替换为small: sed 's/big/small/2' song.txt

解释：/2表示指定一行中第2个匹配的字段操作

$ sed 's/big/small/2' song.txt 
I'm a big small girl
In a big small world
It's not a big small thing if you leave me
But I do do feel
that I do do will miss you much
Miss you much
2、-i参数直接修改文件内容

上面的sed命令没有改变song.txt，只是把处理后的内容输出，如果要写回文件，可以使用重定向。

$ sed 's\big\small\g' song.txt >song.bak
或使用-i直接修改文件内容：sed -i 's\big\small\g' song.txt

$ sed -i 's\big\small\g' song.txt 
$ cat song.txt
I'm a small small girl
In a small small world
It's not a small small thing if you leave me
But I do do feel
that I do do will miss you much
Miss you much
sed 的[-i]参数可以直接修改文件内容，该功能非常有用！

举例来说，如果有一个100万行的文件，要在第100行加某些文字。此时使用vim可能会疯掉！因为文件太大了打不开！但是通过sed直接修改/取代的功能，根本不需要打开文件就能完成任务。和vim相比sed就像会魔术一样，vim要打开文件-操作文件-关闭文件，sed直接隔空就对文件操作了，非常方便。

正因为sed -i 功能强大，可以直接修改原始文件，也是个危险的动作，需小心使用。

3、-e参数编辑命令，进行多行匹配

-e是编辑命令，用于sed执行多个编辑任务的情况下。在下一行开始编辑前，所有的编辑动作都将应用到模式缓冲区中的行上。因为是逐行进行多重编辑（即每个命令都在模式空间的当前行上执行），所以编辑命令的顺序会影响结果。

如果我们要一次替换多个模式，

sed "1,3s/big/small/g; 4,5s/do/don't/g" song.txt

解释：第一个模式：把第一行到第三行的big替换成small；第二个模式：把第四行到第五行的do替换成don't。

$ sed "1,3s/big/small/g; 4,5s/do/don't/g" song.txt
I'm a small small girl
In a small small world
It's not a small small thing if you leave me
But I don't don't feel
that I don't don't will miss you much
Miss you much
上面的命令等价于：sed  -e "1,3s/big/small/g" -e "4,5s/do/don't/g" song.txt 

$ sed  -e "1,3s/big/small/g" -e "4,5s/do/don't/g" song.txt  
I'm a small small girl
In a small small world
It's not a small small thing if you leave me
But I don't don't feel
that I don't don't will miss you much
Miss you much
4、删除命令d：删除匹配的行

命令d删除匹配的输入行，sed先将输入行从文件复制到模式空间里，然后对该行执行sed命令，最后将模式空间的内容显示在屏幕上。如果是命令d，当前模式空间里的输入行会被删除，不被显示。

利用匹配删除行

删除song.txt文件中包含“Miss”的行 ：sed '/Miss/d' song.txt

$ sed '/Miss/d' song.txt 
I'm a big big girl
In a big big world
It's not a big big thing if you leave me
But I do do feel
that I do do will miss you much
下面是利用行号删除行的例子

删除song.txt文件中的第1行：sed '1d' song.txt

$ sed '1d' song.txt 
In a big big world
It's not a big big thing if you leave me
But I do do feel
that I do do will miss you much
Miss you much
删除song.txt文件中2到5行：sed '2,5d' song.txt  

$ sed '2,5d' song.txt 
I'm a big big girl
Miss you much
删除song.txt文件中第3行之后的行：sed '3,$d' song.txt

$ sed '3,$d' song.txt 
I'm a big big girl
In a big big world
5、插入命令a，在当前行后追加一行

a添加新文本到文件中当前行（即读入模式空间中的行）的后面。

在song.txt文件中第3行后插入一行并直接作用于song.txt：sed '3a AAAAAAAAAAAAAAAAAAAAAAA' song.txt 【用空格作为分隔符】

$ sed -i '3a AAAAAAAAAAAAAAAAAAAAAAA' song.txt 
$ cat song.txt
I'm a big big girl
In a big big world
It's not a big big thing if you leave me
AAAAAAAAAAAAAAAAAAAAAAA
But I do do feel
that I do do will miss you much
Miss you much
在匹配'Miss'的行后插入一行：sed '/Miss/a AAAAAAAAAAAAAAAAAAAAAAAAAA' song.txt 【用空格作为分隔符】

$ sed '/Miss/a AAAAAAAAAAAAAAAAAAAAAAAAAA' song.txt                                
I'm a small small girl
In a small small world
It's not a small small thing if you leave me
But I do do feel
that I do do will miss you much
Miss you much
AAAAAAAAAAAAAAAAAAAAAAAAAA
在song.txt最后插入一行：sed '$a append line' song.txt

$ sed '$a append line' song.txt 
I'm a big big girl
In a big big world
It's not a big big thing if you leave me
But I do do feel
that I do do will miss you much
Miss you much
append line
6、插入命令i，在当前行前插入一行

i添加新文本到文件中当前行（即读入模式空间中的行）的前面。

在song.txt文件中第3行后插入一行：sed '3i#iiiiiiiiiiiiiiiiiiiiiiiiiii' song.txt  【#作为分隔符】

$ sed '3i#iiiiiiiiiiiiiiiiiiiiiiiiiii' song.txt 
I'm a small small girl
In a small small world
iiiiiiiiiiiiiiiiiiiiiiiiiii
It's not a small small thing if you leave me
But I do do feel
that I do do will miss you much
Miss you much
在song.txt文件中匹配'Miss'的行前面插入一行：sed '/Miss/i iiiiiiiiiiiiiii' song.txt【空格作为分隔符】

$ sed '/Miss/i iiiiiiiiiiiiiii' song.txt 
I'm a small small girl
In a small small world
It's not a small small thing if you leave me
But I do do feel
that I do do will miss you much
iiiiiiiiiiiiiii
Miss you much
7、新增多行（i行前插入，a行后追加，其他地方一样，以a为例）

插入相同的行，在第2行到第5行之后均插入一行:sed '2,5a append one line ' song.txt

$ sed '2,5a append one line ' song.txt 
I'm a big big girl
In a big big world
append one line 
It's not a big big thing if you leave me
append one line 
But I do do feel
append one line 
that I do do will miss you much
append one line 
Miss you much
插入不同的行，以\n换行

在第2行后面插入三行：sed '2a append one line \nappend two line \nappend three line' song.txt

liuxiaoyan@development:~/mytest$ sed '2a append one line \nappend two line \nappend three line' song.txt 
I'm a big big girl
In a big big world
append one line 
append two line 
append three line
It's not a big big thing if you leave me
But I do do feel
that I do do will miss you much
Miss you much
8、c命令以行为单位替换

 把第2到第5行的内容替换为2-5content：sed '2,5c 2-5content' song.txt

$ sed '2,5c 2-5content' song.txt 
I'm a big big girl
2-5content
Miss you much
9、p命令显示模式空间的内容

以行为单位显示，

显示song.txt文件中的第2到第5行: sed -n '2,5p' song.txt

$ sed -n '2,5p' song.txt 
In a big big world
It's not a big big thing if you leave me
But I do do feel
that I do do will miss you much
搜索匹配显示：

显示匹配'Miss'的行：sed -n '/Miss/p' song.txt

$ sed -n '/Miss/p' song.txt 
Miss you much
10、-n参数取消默认输出

上面的p命令显示时，用到-n参数，因为sed默认把输入行打印在屏幕上：sed '/Miss/p' song.txt

liuxiaoyan@development:~/mytest$ sed '/Miss/p' song.txt 
I'm a big big girl
In a big big world
It's not a big big thing if you leave me
But I do do feel
that I do do will miss you much
Miss you much
Miss you much
所以要打印选定内容，用-n配合p来使用。

五、简单正则表达式

^:行首定位符。

$:行尾定位符。

\<：词首定位符。

\>：词尾定位符。

.:匹配除换行以外的单个字符。

*:匹配0个或多个前导字符。

[]:匹配字符集合里的任一字符。

[^] :匹配不在指定字符集合里的任一字符。

 

更深入了解可参考以下资料：

[《shell基础十二篇》](http://bbs.chinaunix.net/forum.php?mod=viewthread&tid=452942)

[sed 简明教程- 酷壳陈皓](http://coolshell.cn/articles/9104.html)


