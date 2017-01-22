# Linux命令之grep

命令行参数：grep -i pattern files ：不区分大小写地搜索。默认情况区分大小写，grep -n pattern files: 打印匹配的行并追加行号.grep -v pattern files 打印不包含pattern的行.grep -l pattern files ：只列出匹配的文件名，grep -L pattern files ：列出不匹配的文件名，注意，对于grep -L pattern ./* , 假如当前子目录mydir中含有符合匹配条件的a.txt,但列不匹配文件名时却有mydir; 如果改为grep -L -r pattern ./* 才不会列出mydir.grep -w pattern files ：只匹配整个单词，而不是字符串的一部分（如匹配‘magic’，而不是‘magical’），grep -C number pattern files ：匹配的上下文分别显示[number]行，grep 'pattern1\| pattern2' files ：显示匹配 pattern1 或 pattern2 的行,注意, 要双引号或单引号，以及反斜杠。grep pattern1 files | grep pattern2 ：显示既匹配 pattern1 又匹配 pattern2 的行。
正则表达式遵循的几个重复的操作

? 最多匹配一次
* 匹配零次或者任意多次
+ 匹配一次以上
{n} 匹配n次
{n,} 最少匹配n次
{,m} 最多匹配m次
{n,m} 匹配n到m次



从文件内容查找匹配指定字符串的行：
$ grep "被查找的字符串" 文件名
从文件内容查找与正则表达式匹配的行：
$ grep –e “正则表达式” 文件名
查找时不区分大小写：
$ grep –i "被查找的字符串" 文件名
查找匹配的行数：
$ grep -c "被查找的字符串" 文件名
从文件内容查找不匹配指定字符串的行：
$ grep –v "被查找的字符串" 文件名
从根目录开始查找所有扩展名为.log的文本文件，并找出包含”ERROR”的行
find / -type f -name "*.log" | xargs grep "ERROR"



1.从单个文件中搜索指定的字串
grep的基础用法是如下例的从指定的文件中搜索特定的字串。

语法：
grep "literal_string" filename
$ grep "this" demo_file
this line is the 1st lower case line in this file.
Two lines above this line is empty.
And this is the last line.
2. 在多个文件中检索指定的字串
语法：
grep "string" FILE_PATTERN
先拷贝demo_file为demo_file1。grep的结果在符合条件的行前将包括文件名。当文件名包含元字符时，linux shell会将匹配的所有文件作为输入到grep中去。
$ cp demo_file demo_file1

$ grep "this" demo_*
demo_file:this line is the 1st lower case line in this file.
demo_file:Two lines above this line is empty.
demo_file:And this is the last line.
demo_file1:this line is the 1st lower case line in this file.
demo_file1:Two lines above this line is empty.
demo_file1:And this is the last line.
3. 用 grep -i 进行大小写无关的搜索
语法：
grep -i "string" FILE

也是一个基本用法，对搜索的字串忽略大小写，因此下例中匹配“the”, “THE” and “The”。

$ grep -i "the" demo_file
THIS LINE IS THE 1ST UPPER CASE LINE IN THIS FILE.
this line is the 1st lower case line in this file.
This Line Has All Its First Character Of The Word With Upper Case.
And this is the last line.
4. 使用用正则表达式
语法：
grep "REGEX" filename

如果你能有效地利用正则表达式，这是个很有用的特点。在下面的例子中，搜索全部以“lines”开始以“empty”结束的字串，如搜索“lines[之间任意字]empty” ，并且忽略大小写。

$ grep -i "lines.*empty" demo_file
Two lines above this line is empty.
正则表达式遵循的几个重复的操作

? 最多匹配一次
* 匹配零次或者任意多次
+ 匹配一次以上
{n} 匹配n次
{n,} 最少匹配n次
{,m} 最多匹配m次
{n,m} 匹配n到m次
5. 用grep -w搜索整个词，而不是词中的部分字串
使用-w选项搜索一个单词，并且避免搜索到词中的部分字串。

下例搜索"is"。如果不加-w选项，将显示“is”, “his”, “this” 等所有包含“is”的行。

$ grep -i "is" demo_file
THIS LINE IS THE 1ST UPPER CASE LINE IN THIS FILE.
this line is the 1st lower case line in this file.
This Line Has All Its First Character Of The Word With Upper Case.
Two lines above this line is empty.
And this is the last line.

下例使用了-w选项，请注意结果中不包含 “This Line Has All Its First Character Of The Word With Upper Case”, 虽然 “This”中包含“is”。

$ grep -iw "is" demo_file
THIS LINE IS THE 1ST UPPER CASE LINE IN THIS FILE.
this line is the 1st lower case line in this file.
Two lines above this line is empty.
And this is the last line.
6. 使用grep -A, -B and -C显示之前、之后、前后的几行
当使用grep搜索大文件时，显示匹配行附近的多行数据是一个很有用的功能。


创建如下文件

$ cat demo_text
4. Vim Word Navigation

You may want to do several navigation in relation to the words, such as:

 * e - go to the end of the current word.
 * E - go to the end of the current WORD.
 * b - go to the previous (before) word.
 * B - go to the previous (before) WORD.
 * w - go to the next word.
 * W - go to the next WORD.

WORD - WORD consists of a sequence of non-blank characters, separated with white space.
word - word consists of a sequence of letters, digits and underscores.

Example to show the difference between WORD and word

 * 192.168.1.1 - single WORD
 * 192.168.1.1 - seven words.
6.1 显示匹配行之后的N行

-A

语法：
grep -A  "string" FILENAME

下例显示匹配行和之后的3行数据

$ grep -A 3 -i "example" demo_text
Example to show the difference between WORD and word

* 192.168.1.1 - single WORD
* 192.168.1.1 - seven words.
6.2显示匹配行之前的N行

-B

语法：
grep -B  "string" FILENAME

下例显示匹配行和之前的2行数据

$ grep -B 2 "single WORD" demo_text
Example to show the difference between WORD and word

* 192.168.1.1 - single WORD
6.3显示匹配行前后的N行

-C 显示之前的n行，之后的n行数据.

$ grep -C 2 "Example" demo_text
word - word consists of a sequence of letters, digits and underscores.

Example to show the difference between WORD and word

* 192.168.1.1 - single WORD
7.通过GREP_OPTIONS高亮显示搜索的字串
如果你希望搜索的字串高亮显示在结果中，可以试用以下的办法。

通过修改GREP_OPTIONS对搜索字串高亮显示。

$ export GREP_OPTIONS='--color=auto' GREP_COLOR='100;8'

$ grep this demo_file
this line is the 1st lower case line in this file.
Two lines above this line is empty.
And this is the last line.
8. 用grep -r递归搜索全部的文件
如果想查找当前目前以及其子目录的全部文件时，可以使用 -r 选项。如下例

$ grep -r "ramesh" *
9. 使用grep -v进行不匹配
可以使用-v选项显示不匹配搜索字串的行。下例显示demo_text文件中不包含“go”的行

$ grep -v "go" demo_text
4. Vim Word Navigation

You may want to do several navigation in relation to the words, such as:

WORD - WORD consists of a sequence of non-blank characters, separated with white space.
word - word consists of a sequence of letters, digits and underscores.

Example to show the difference between WORD and word

* 192.168.1.1 - single WORD
* 192.168.1.1 - seven words.
10. 显示不匹配全部模式的行
语法：
grep -v -e "pattern" -e "pattern"
创建如下例子文件

$ cat test-file.txt
a
b
c
d

$ grep -v -e "a" -e "b" -e "c" test-file.txt
d
11.用grep -c 统计匹配的行数
语法：
grep -c "pattern" filename
$ grep -c "go" demo_text
6

统计不匹配的行数

$ grep -v -c this demo_file
4
12. 用grep -l 只显示文件名
$ grep -l this demo_*
demo_file
demo_file1
13. 只显示匹配的字串
缺省显示匹配字串的所在行，可以使用-o选项只显示匹配的字串。这项功能当使用正则表达式时比较有用处。

$ grep -o "is.*line" demo_file
is line is the 1st lower case line
is line
is is the last line
14. 显示匹配的位置
语法：
grep -o -b "pattern" file
$ cat temp-file.txt
12345
12345

$ grep -o -b "3" temp-file.txt
0:3
6:3

注意: 以上输出显示的不是行内的位置，而是整个文件中的字节byte位置

15. 用 grep -n 在输出时显示行号
行号从1开始

$ grep -n "go" demo_text
5: * e - go to the end of the current word.
6: * E - go to the end of the current WORD.
7: * b - go to the previous (before) word.
8: * B - go to the previous (before) WORD.
9: * w - go to the next word.
10: * W - go to the next WORD.