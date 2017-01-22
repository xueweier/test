---
layout: post
title: Linux的流、管道和重定向
category: linux
---


在早先曾经发了一篇[《shell入门》](http://blog.kelu.org/linux/2015/01/17/shell-tutorial.html)，对输入输出这一块一段话带过了。在最近用到`>/dev/null 2>&1 `的时候才想起来，总结一下Linux的重定向吧，顺便把流和管道也放一起了。

本文内容部分来源于 [IBM developerWorks 中国](http://www.ibm.com/developerworks/cn/) - [《学习 Linux，101: 流、管道和重定向》](http://www.ibm.com/developerworks/cn/linux/l-lpic1-v3-103-4/)

### 输入输出流

绝大部分 UNIX® 进程(包括图形应用程序，但不包括绝大多数守护程序)至少使用三个文件描述符：标准输入、标准输出和标准错误输出。一般来说，这三个描述符与该进程启动的终端相关联，其中输入为键盘。重定向和管道的目的是重定向这些描述符。

	stdout 是标准输出流，它显示来自命令的输出。它的文件描述符为 1。  
	stderr 是标准错误流，它显示来自命令的错误输出。它的文件描述符为 2。  
	stdin 是标准输入流，它为命令提供输入。它的文件描述符为 0。

### 重定向输出

可以通过两种方法将输出重定向到文件，其中n为文件描述符，也就是1或者2，如果省略它，将执行标准输出1。

	n> 文件名 
		必须具有该文件的写权限。如果该文件不存在，将创建它。如果该文件已经存在，通常将覆盖所有现有内容，并且没有任何警告。
	n>> 文件名
		一样要求您具有该文件的写权限。如果该文件不存在，将创建它。如果该文件已经存在，输出将附加到现有的内容后面。
		
如果将标准输出和标准错误都重定向到一个文件中，那么使用 `&>` 或 `&>>` 。同时还可以使用 `m>&n` 或 `m>>&n` 

	command 2>&1 >output.txt
	command >output.txt 2>&1
	
在第一个命令中，标准错误流定向到了标准输出流，然后只有标准输出流写入output.txt，错误输出还是会显示在屏幕上，而不显示在文本里。
	
下一个命令表示标准输出将被重定向到(>)/dev/null(所有输出到此特殊文件的东西都将被丢弃，即不显示标准输出)，并将标准错误输出(2)重定向到(>)errors 文件。

	ls -R /shared >/dev/null 2>errors

想要完全忽略标准输出或标准错误。为此，将选择的流重定向到空文件 /dev/null。

	command 2>/dev/null


### 重定向输入

	cat<<END>ex-here.sh
	> cat <<-EOF
	> apple
	> EOF
	> ${ht}cat <<-EOF
	> ${ht}pear
	> ${ht}EOF
	> END
	
使用END作为输入结束的标记。

### 管道

	command1 | command2 paramater1 | command3 parameter1 - parameter2 | command4
	
这里我们通过管道将 stdout 导入到 stdin。

Linux 和 UNIX 系统中的管道的优点之一是，与其他流行的操作系统不同，它们的管道不涉及到中间文件。比如下一个解压的命令。

	bunzip2 -c somefile.tar.bz2 | tar -xvf -
	
### 使用输出作为参数

使用管道可以达到接受一个命令的输出，并将它用作另一个命令的输入的效果。

但是如果我们想将一个命令或文件的内容作为另一个命令的参数而不是输入。管道就无能为力了。为此常见下面三种办法：

* xargs 命令
* 带有 -exec 选项的 find 命令
* 命令替换

使用输出作为参数
在前面对管道线的讨论中，您学习了如何接受一个命令的输出，并将它用作另一个命令的输入。反过来，假设您想将一个命令或文件的内容作为另一个命令的参数而不是输入。管道线不能用于实现该目的。三种常见的解决办法是：
xargs 命令
带有 -exec 选项的 find 命令
命令替换
您将首先了解第一个解决办法。我们曾经在清单 9 中创建了一个强制制表符，您可以从中看到命令替换的例子。可以在命令行上使用命令替换，但在脚本中使用它则更常见；您将在本系列的后续文章中更多地了解它和脚本。查看我们的 学习 Linux，101：LPIC-1 路线图 获得本系列所有文章的简介和链接。
使用 xargs 命令
xargs 命令读取标准的输入，然后使用参数作为输入构建和执行命令。如果没有给出命令，那么将使用 echo 命令。清单 12 是使用我们的 text1 文件的基础例子，它包含 3 个行，每行只有两个单词。
清单 12. 使用 xargs
[ian@echidna lpi103-4]$ cat text1
1 apple
2 pear
3 banana
[ian@echidna lpi103-4]$ xargs<text1
1 apple 2 pear 3 banana
为什么 xargs 只有一行输出？默认情况下，xargs 在空格处中断输出，并且每个生成的标记都成为一个参数。不过，当 xargs 构建命令时，它将一次传递尽可能多的参数。您可以使用 -n 覆盖该行为，或使用 --max-args 参数。在清单 13 中，我们使用了这两种方法，并为使用 xargs 添加一个显式的 echo 调用。
清单 13. 使用 xargs 和 echo
[ian@echidna lpi103-4]$ xargs<text1 echo "args >"
args > 1 apple 2 pear 3 banana
[ian@echidna lpi103-4]$ xargs --max-args 3 <text1 echo "args >"
args > 1 apple 2
args > pear 3 banana
[ian@echidna lpi103-4]$ xargs -n 1 <text1 echo "args >"
args > 1
args > apple
args > 2
args > pear
args > 3
args > banana
如果输入包含由单引号或双引号保护的空格，或使用了斜杠进行转义，那么 xargs 将不在遇到这些空格时中断。清单 14 显示了这些空格点。
清单 14. 使用带引号的 xargs
[ian@echidna lpi103-4]$ echo '"4 plum"' | cat text1 -
1 apple
2 pear
3 banana
"4 plum"
[ian@echidna lpi103-4]$ echo '"4 plum"' | cat text1 - | xargs -n 1
1
apple
2
pear
3
banana
4 plum
到目前为止，已经在命令的末尾添加了所有参数。如果您需要在这些参数后面再使用其他参数，可以使用 -I 选项指定一个替换字符串。如果 xargs 将要执行的命令包含有替换字符串，那么将使用参数替换它。进行了替换之后，仅将参数传递给每个命令。不过，将从一整行输出创建参数，而不仅是一个标记。您还可以使用 xargs 的 -L 选项让命令将行当作参数看待，而不是默认的以单个空格分隔的标记。使用 -I 选项表示 -L 1。清单 15 显示了使用 -I 和 -L 选项的例子。
清单 15. 使用带有输入行的 xargs
[ian@echidna lpi103-4]$ xargs -I XYZ echo "START XYZ REPEAT XYZ END" <text1
START 1 apple REPEAT 1 apple END
START 2 pear REPEAT 2 pear END
START 3 banana REPEAT 3 banana END
[ian@echidna lpi103-4]$ xargs -IX echo "<X><X>" <text2
<9      plum><9 plum>
<3      banana><3       banana>
<10     apple><10       apple>
[ian@echidna lpi103-4]$ cat text1 text2 | xargs -L2
1 apple 2 pear
3 banana 9 plum
3 banana 10 apple
尽管我们的例子为了便于演示使用了简单的文本文件，您很少看到包含这样的输入的 xargs。您通常需要处理某些命令生成的大量文件，这些命令包括 ls、find 或 grep。清单 16 显示了一种通过 xargs 将目录清单传递到命令（比如 grep）的方法。
清单 16. 使用带有多个文件的 xargs
[ian@echidna lpi103-4]$ ls |xargs grep "1"
text1:1 apple
text2:10        apple
xaa:1 apple
yaa:1
如果上一个例子中的一个或多个文件名包含空格，那么会发生什么呢？如果您像清单 16 那样使用该命令，那么将得到一个错误。在实际情况中，文件列表可能来自一些源，比如定制脚本或命令，而不是 ls，或者您希望通过其他管道线阶段传递它，以进一步进行过滤。所以您应该使用 grep "1" * 取代以上构造。
对于 ls 命令，您可以使用 --quoting-style 选项强制给导致问题的文件名加上引号或进行转义。另外一种更好的解决办法是使用 xargs 的 -0 选项，从而使用 null 字符串 (\0) 分隔输入参数。尽管 ls 没有提供使用 null 字符串分隔的文件名作为输出的选项，但许多命令都提供这样的选项。
在清单 17 中，我们首先将 text1 复制到 “text 1”，然后显示一些在 xargs 命令中使用包含空格的文件名列表的方法。这些示例仅为了演示概念，因为 xargs 可能更加复杂。尤其是在最后一个例子中， 如果一些文件名已经包含新行字符串，那么将新行字符串转换成 null 字符串将导致错误。在本文的下一个部分中，我们将查看另外一个更加健壮的解决方案，即使用 find 命令生成合适的以 null 字符串分隔的输出。
清单 17. 文件名中包含空格的 xargs
[ian@echidna lpi103-4]$ cp text1 "text 1"
[ian@echidna lpi103-4]$ ls *1 |xargs grep "1" # error
text1:1 apple
grep: text: No such file or directory
grep: 1: No such file or directory
[ian@echidna lpi103-4]$ ls --quoting-style escape *1
text1  text\ 1
[ian@echidna lpi103-4]$ ls --quoting-style shell *1
text1  'text 1'
[ian@echidna lpi103-4]$ ls --quoting-style shell *1 |xargs grep "1"
text1:1 apple
text 1:1 apple
[ian@echidna lpi103-4]$ # Illustrate -0 option of xargs
[ian@echidna lpi103-4]$ ls *1 | tr '\n' '\0' |xargs -0 grep "1"
text1:1 apple
text 1:1 apple
xargs 命令不会构建任意长度的命令。在 Linux 内核 2.26.3 之前，命令的长度是受限制的。针对某个包含大量名称很长的文件的目录的命令，比如 rm somepath/*，可能会失败，返回的消息表明参数列表太长。在更旧的 Linux 系统或 UNIX 系统上仍然存在该限制，因此了解如何使用 xargs 以处理这种问题非常有用。
您可以使用 --show-limits 选项显示 xargs 的默认限制，然后使用 -s 选项将输出命令的长度限制在允许的最大字符串数量之内。查看手册页了解其他未能再次讨论的选项。
使用带有 -exec 选项或 xargs 的 find 命令
在文章 “学习 Linux，101：文件和目录管理” 中，您学习例如如何使用 find 命令根据名称、修改时间、大小或其他特征查找文件。找到匹配的文件集之后，您通常希望对它们执行某些操作：删除、移动和重命名它们等。现在我们看一下 find 命令的 -exec 选项，其功能类似于使用 find 并通过管道将输出指向 xargs。
清单 18. 使用 find 和 -exec
[ian@echidna lpi103-4]$ find text[12] -exec cat text3 {} \;
This is a sentence.  This is a sentence.  This is a sentence.
1 apple
2 pear
3 banana
This is a sentence.  This is a sentence.  This is a sentence.
9       plum
3       banana
10      apple
与前面学习的 xargs 命令相比，它有几个不同之处。
您必须使用 {} 标记文件名在命令中的位置。它不是自动添加在末尾的。
您必须使用转义后的分号终止该命令，比如 \;、';' 或 ";" 都行。
该命令对每个输入文件执行一次。
尝试运行 find text[12] |xargs cat text3 亲自看看区别在哪里。
现在，我将话题转回到文件名中的空格。在清单 19 中我们尝试使用带有 -exec 的 find，而不是带有 xargs 的 ls。
清单 19. 对包含空格的文件名使用 find 和 -exec
[ian@echidna lpi103-4]$ find . -name "*1" -exec grep "1" {} \;
1 apple
1 apple
到目前为止，一切进展顺利。但是不是缺少了什么？哪个文件包含 grep 找到行？缺少了文件名，因为 find 为每个文件调用 grep 一次，而 grep 非常智能，能够知道您是不是仅提供文件名，您不需要它告诉您是哪个文件。
我们也可以改为使用 xargs，但我们已经看到了文件名中包含空格时出现的问题。我们还提到 find 可以生成一个以 null 分隔符分隔的文件名列表，这是 -print0 选项所起的作用。新的 find 可能使用加号（+）取代分号（;）作为分隔符，这允许 find 在一次调用命令时传递尽可能多的名称，类似于 xargs。在这种情况中，仅能使用 {} 一次，并且它必须是该命令的最后一个参数。清单 20 显示了这两种方法。
清单 20. 对包含空格的文件名使用 find 和 xargs
[ian@echidna lpi103-4]$ find . -name "*1" -print0 |xargs -0 grep "1"
./text 1:1 apple
./text1:1 apple
[ian@echidna lpi103-4]$ find . -name "*1" -exec grep "1" {} +
./text 1:1 apple
./text1:1 apple
一般而言，两种方法都是有效的，选择哪种方法由您决定。记住，使用管道导出包含未受保护的空格的内容将导致问题，因此如果您要使用管道将输出导出到 xargs，请使用将 -print0 选项和 find 结合使用，并使用 -0 选项告诉 xargs 接收使用 null 分隔符分隔的输入。其他命令，包括 tar，也支持使用 -0 选项并用 null 分隔符分隔的输入，因此应该对支持该选项的命令使用它，除非您能确保您的输入列表不会造成问题。
最后，我们介绍对文件列表进行操作。在执行删除或重命名文件等重要操作之前，最好彻底地测试列表和仔细测试命令。进行良好的备份也是非常有价值的。
回页首
分离输出
这个小节简单地讨论另一个命令。有时候，您可能希望在屏幕上看到输出，同时保留一个副本。尽管您可以将命令输出重定向到一个窗口中的文件，然后使用 tail -fn1 在另一个屏幕中跟踪输出来实现该目的，但使用 tee 命令要简单得多。
您可以将 tee 和管道一起使用。对标准输出而言，参数是一个或多个文件。-a 选项附加而非覆盖文件。在前面关于管道的讨论中可以看到，必须先将 stderr 重定向到 stdout ，然后再重定向到 tee，如果您需要同时保存两者的话。清单 21 显示 用于将输出保存到文件 f1 和 f2 中的 tee 。
清单 21. 使用 tee 分离 stdout
[ian@echidna lpi103-4]$ ls text[1-3]|tee f1 f2
text1
text2
text3
[ian@echidna lpi103-4]$ cat f1
text1
text2
text3
[ian@echidna lpi103-4]$ cat f2
text1
text2
text3





其它参考来源：

http://www.ibm.com/developerworks/cn/linux/l-lpic1-v3-map/

