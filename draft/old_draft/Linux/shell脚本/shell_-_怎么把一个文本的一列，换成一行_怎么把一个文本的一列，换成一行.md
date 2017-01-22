### shell - 怎么把一个文本的一列，换成一行 怎么把一个文本的一列，换成一行
2004-04-23 15:18 pm
来自：Linux文档
现载：Www.8s8s.coM
地址：无名

请教！怎么把一个文本的一列，换成一行？
在vi中能做吗？

sed可以做到

是吗？请教sed怎么实现呢？谢谢！

也就是一个文件是这样的：
oplasttr
dsprjord
accontsup106
alprjinfo
holiday
把它替换成oplasttr dsprjord accontsup106 alprjinfo holiday

cat test.txt | awk '{printf "%s ",$0}'

A=`cat file`;echo $A
这是shell在把命令处理结果赋给变量时的一种特性.也就是``的功能.

[这个贴子最后由hwhcom在 2002/10/17 11:25am 编辑]

如果文件有多个域,把每个域的列换成行,该如何???
file a
a aa
b bb
c cc
转换为
a b c
aa bb cc
斑竹的方法好像就不行

这样的话就可以按照superhoo的方法来做，一列列的处理，然后追加到一个文件中就ok了。
cat fileA | awk '{printf "%s ",$1}' >> fileB
echo >> fileB
cat fileA | awk '{printf "%s ",$2}' >> fileB

用cut也可以
A1=`cat jj|cut -d" " -f1`
A2=`cat jj|cut -d" " -f2`

斑竹的方法是一列，我的方法是两列，要是有不确定列怎么办？
比如文件：fileA(都是左对齐）
a b c d
aa bb
e
ddd f cc
要是象finger结果文件，简化一下，怎么处理了？
# finger
LoginName Tty Idle Where
client *p1 10.1.1.101
client *p0 10.1.1.97
client *p2 10.1.1.98
client *p6 10.1.1.171
client *p7 27 10.1.1.157
client *p8 4 10.1.1.74

思路应该是先对文件扫描，得出最多有几个域，以此作为循环次数，再分别对每列读取。

下面引用由hwhcom在 2002/10/17 11:23am 发表的内容：
如果文件有多个域,把每个域的列换成行,该如何???
file a
a aa
b bb
...



把每个域的列换成行，且每行的域数不确定用shell实现如下：
有点繁，期待简化版！
#!/bin/sh
max=0
while read v
do
nf=`echo "$v"|awk '{print NF}'`
if [ $nf -gt $max ]
then
max=$nf
fi
done <fileA

c=1
while [ $c -le $max ]
do
cat fileA |awk '{printf "%s ",$"'$c'"} END{printf " "}' >>fileB
c=`expr $c + 1`
done

请教 microroad 我试了以下你的程序，发现一个问题，就是那个max参数，
它在while循环里被赋值，但是一出循环它的值又变成了0，这是怎么回事啊？？
这与版本有关吗？？

不应该的。我在shell下都测试过的。
你可在第一个循环中加上echo $nf和echo $max,在用sh -x rowtoline.sh (假定什么的shell script叫rowtoline.sh）调试，看看原因在哪。

继续请教 microroad
我的shell程序是：
#!/bin/sh

TMP=bbb.txt
FILE=aaa.txt
max=0

while read TXT
do
nf=`echo $TXT | awk '{print NF}'`

if [ $nf -gt $max ]
then
max=$nf
fi
done<$FILE

echo $max

LNo=1
while [ $LNo -le $max ]
do
awk '{printf "%s ",$"'$LNo'"} END{printf " "}' $FILE >> $TMP
LNo=$LNo+1
done

文本文件aaa.txt是：
1 aa
2 bb
3 cc eee
4 dd
5 cccccccccc
6 uuuuuuuu

-x的结果是：
TMP=bbb.txt
FILE=aaa.txt
max=0
+ read TXT
+ + awkecho {print NF}1
aa
nf=2
+ [ 2 -gt 0 ]
max=2
+ read TXT
+ + awkecho {print NF}2
bb
nf=2
+ [ 2 -gt 2 ]
+ read TXT
+ + awkecho {print NF}3
cc eee
nf=3
+ [ 3 -gt 2 ]
max=3 -------------------------------------此处的max被赋值为3
+ read TXT
+ + awkecho {print NF}4
dd
nf=2
+ [ 2 -gt 3 ]
+ read TXT
+ + awkecho {print NF}5
cccccccccc
nf=2
+ [ 2 -gt 3 ]
+ read TXT
+ + awkecho {print NF}6
uuuuuuuu
nf=2
+ [ 2 -gt 3 ]
+ read TXT
+ echo 0
0 ----------------------------此处的max又变成了0
LNo=1
+ [ 1 -le 0 ]


请帮忙看一下，谢谢。
（注：系统为SCO UnixWare 7.0.1）

to skydown ：看看下面链接指向的贴子吧
http://www.chinaunix.net/cgi-bin/bbs/topic.cgi?forum=11&topic=112

对，你应该是遇到了楼上所说的陷阱。但这在我这里（irix，shell）是没有楼上帖子所说的陷阱的，幸运的让我奇怪！
另外，你的aaa.txt若不包括第一列的数字，它的最大域数应该只是2，还有第二个while循环中LNo=$LNo+1应该是LNo=`exor $LNo+1`吧。
