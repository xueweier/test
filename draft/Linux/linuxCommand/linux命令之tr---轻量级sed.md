# linux命令之tr---轻量级sed

通过使用 tr，您可以非常容易地实现 sed 的许多最基本功能。您可以将 tr 看作为 sed 的（极其）简化的变体：它可以用一个字符来替换另一个字符，或者可以完全除去一些字符。您也可以用它来除去重复字符。这就是所有 tr 所能够做的。 

tr用来从标准输入中通过替换或删除操作进行字符转换。tr主要用于删除文件中控制字符或进行字符转换。使用tr时要转换两个字符串：字符串1用于查询，字符串2用于处理各种转换。tr刚执行时，字符串1中的字符被映射到字符串2中的字符，然后转换操作开始。

必须使用引用或中括号, 这样做才是合理的. 引用可以阻止shell重新解释出现 在tr命令序列中的特殊字符. 中括号应该被引用起来防止被shell扩展.

一、最常用选项的tr命令格式为

    tr -c -d -s -t ["string1_to_translate_from"] ["string2_to_translate_to"] < input-file

说明：

	-c, --complement    反选设定字符。也就是符合 string1_to_translate_from 的部份不做处理，不符合的剩余部份才进行转换
	-d, --delete        删除指令字符
	-s, --squeeze-repeats    缩减连续重复的字符成指定的单个字符
	-t, --truncate-set1        削减 string1_to_translate_from 指定范围，使之与 string2_to_translate_from 设定长度相等
	--help                显示程序用法信息
	--version            显示程序本身的版本信息
	input-file            是转换文件名。虽然可以使用其他格式输入，但这种格式最常用。

二、字符范围

指定字符串1或字符串2的内容时，只能使用单字符或字符串范围或列表:

	[a-z] a-z内的字符组成的字符串
	[A-Z] A-Z内的字符组成的字符串
	[0-9] 数字串
	\octal 一个三位的八进制数，对应有效的ASCII字符
	[O*n] 表示字符O重复出现指定次数n。因此[O*2]匹配OO的字符串
	
	tr中特定控制字符的不同表达方式
	速记符含义八进制方式
	\a Ctrl-G  铃声\007
	\b Ctrl-H  退格符\010
	\f Ctrl-L  走行换页\014
	\n Ctrl-J  新行\012
	\r Ctrl-M  回车\015
	\t Ctrl-I  tab键\011
	\v Ctrl-X  \030

三、实例

1. 将文件file中出现的"abc"替换为"xyz"
	
	[xt@butbueatiful command]$ cat test.txt 
	abcd
	asbcd
	[xt@butbueatiful command]$ tr "abc" "xyz" < test.txt 
	xyzd
	xsyzd
	[xt@butbueatiful command]$ 
 
【注意】这里，凡是在test.txt中出现的"a"字母，都替换成"x"字母，"b"字母替换为"y"字母，"c"字母替换为"z"字母。而不是将字符串"abc"替换为字符串"xyz"
 
2. 使用tr命令“统一”字母大小写

小写 ---> 大写
[xt@butbueatiful command]$ cat test.txt 
abcd
asbcd
[xt@butbueatiful command]$ tr [a-z] [A-Z] < test.txt 
ABCD
ASBCD
[xt@butbueatiful command]$ 

小写 <--- 大写
[xt@butbueatiful command]$ cat test.txt
ABCD
ASBCD
[xt@butbueatiful command]$ tr [A-Z] [a-z] < test.txt
abcd
asbcd
[xt@butbueatiful command]$ 

小写 <---> 大写
[xt@butbueatiful command]$ cat test.txt
aaBBCc
[xt@butbueatiful command]$ tr 'a-zA-Z' 'A-Za-z' < test.txt 
AAbbcC
[xt@butbueatiful command]$ 
 
3. 把文件中的数字0-9替换为a-j

[xt@butbueatiful command]$ cat test.txt
ABCD
ASBCD
12310
[xt@butbueatiful command]$ tr [0-9] [a-j] < test.txt 
ABCD
ASBCD
bcdba
[xt@butbueatiful command]$

4. 删除文件test.txt中出现的"abc"字符

[xt@butbueatiful command]$ cat test.txt 
abcd
asbcd
12310
[xt@butbueatiful command]$ tr -d "abc" < test.txt 
d
sd
12310
[xt@butbueatiful command]$ 
 
【注意】这里，凡是在file文件中出现的'a','b','c'字符都会被删除, 而不是紧紧删除出现的"abc”字符串。
 
5. 删除文件file中出现的换行'\n'、制表'\t'字符

[xt@butbueatiful command]$ sed -n l test.txt
ab\tcd$
asbcd$
12310$
[xt@butbueatiful command]$ tr -d "\n\t" < test.txt 
abcdasbcd12310[xt@butbueatiful command]$
 
不可见字符都得用转义字符来表示的，这个都是统一的。
 
6. 删除“连续着的”重复字母 只保留第一个

[xt@butbueatiful command]$ cat  test.txt
aaaabbbbbcccc
aaaabbbbbcccc
[xt@butbueatiful command]$ tr -s [a-zA-Z] < test.txt
abc
abc
[xt@butbueatiful command]$ 
 
7. 删除空行

[xt@butbueatiful command]$ cat test.txt
aaa

bbb

ccc
[xt@butbueatiful command]$ tr -s "\n" < test.txt 
aaa
bbb
ccc
[xt@butbueatiful command]$ 

8. 删除Windows文件“造成”的'^M'字符

[xt@butbueatiful command]$ sed -n l test.txt
abc\r$
abcdef\r$
[xt@butbueatiful command]$ tr -d "\r" < test.txt > new
[xt@butbueatiful command]$ sed -n l new 
abc$
abcdef$
[xt@butbueatiful command]$ 

或

[xt@butbueatiful command]$ tr -s "\r" "\n" > test.txt
 
【注意】这里-s后面是两个参数"\r"和"\n"，用后者替换前者
 
9. 用空格符\040替换制表符\011

[xt@butbueatiful command]$ sed -n l test.txt
aaa\tbbb\t\tccc$
aaa\tbbb\t\tccc$
aaa\tbbb\t\tccc$
[xt@butbueatiful command]$ tr -s "\011" "\040" < test.txt
aaa bbb ccc
aaa bbb ccc
aaa bbb ccc
[xt@butbueatiful command]$ tr -s "\011" "\040" < test.txt > new
[xt@butbueatiful command]$ sed -n l new 
aaa bbb ccc$
aaa bbb ccc$
aaa bbb ccc$
[xt@butbueatiful command]$

10. 把路径变量中的冒号":" 替换成换行符"\n"
 
[xt@butbueatiful command]$ echo $PATH | tr -s ":" "\n"


11. 让转换参数做1对1的有效对应

[xt@butbueatiful command]$ cat test.txt
a
ab
abc
abcd efgh ijkl
[xt@butbueatiful command]$ tr 'a-z' '012' < test.txt
0
01
012
0122 2222 2222
[xt@butbueatiful command]$

这个显然不是我们想要的结果，要避免或限制这种效应，可以加上 -t 选项

[xt@butbueatiful command]$ tr -t 'a-z' '012' < test.txt
0
01
012
012d efgh ijkl
[xt@butbueatiful command]$ 
因 -t 选项，使对应关系改变成 a 转换成 0, b 转换成 1, c 转换成 2, d-z 因 string2_to_translate_from 没有设定而不做任何转换对应。

12. 加密解密

加密
[xt@butbueatiful command]$ cat test.txt 
a
ab
abc
abcd efgh ijkl
[xt@butbueatiful command]$ tr 'a-z' 'qazwsxedcrfvtgbyhnujmikolp' < test.txt > new 
[xt@butbueatiful command]$ cat new 
q
qa
qaz
qazw sxed crfv
[xt@butbueatiful command]$ 

为了举例方便，密码字母集我只设定小写字母。如果 真想用的话，最好将整个 ASCII 全部代替会比较理想。同时为了安全起见，指令不可在交谈模式下使用，因为程序有可能正被观察中，同时也必须注意history功能是否储存的密码字母集。 最好的方式是将程序写成 script 来执行，当然密码字母集不可包含在其中。

解密
只需将正确的密码字母集放到 SET1 将替换的程序反过来即可...

[xt@butbueatiful command]$ tr 'qazwsxedcrfvtgbyhnujmikolp' 'a-z' < new
a
ab
abc
abcd efgh ijkl
[xt@butbueatiful command]$ 

这种加密法适合 100 个字母以内的简短讯息使用，绝对可以蒙过一些小Q蛋，只是碰上频率分析法专家会被笑而已。