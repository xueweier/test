# linux命令之exec

Linux exec与重定向 (2011-05-13 14:15:04)转载▼
标签： it linux 命令 exec 重定向	分类： Linux
    exec和source都属于bash内部命令（builtins commands），在bash下输入man exec或man source可以查看所有的内部命令信息。
bash shell的命令分为两类：外部命令和内部命令。外部命令是通过系统调用或独立的程序实现的，如sed、awk等等。内部命令是由特殊的文件格式（.def）所实现，如cd、history、exec等等。
在说明exe和source的区别之前，先说明一下fork的概念。
    fork是linux的系统调用，用来创建子进程（child process）。子进程是父进程(parent process)的一个副本，从父进程那里获得一定的资源分配以及继承父进程的环境。子进程与父进程唯一不同的地方在于pid（process id）。
    环境变量（传给子进程的变量，遗传性是本地变量和环境变量的根本区别）只能单向从父进程传给子进程。不管子进程的环境变量如何变化，都不会影响父进程的环境变量。
 
	 tmp_fifofile="/tmp/$$.fifo"
	mkfifo $tmp_fifofile      # 新建一个fifo类型的文件
	exec 6<>$tmp_fifofile      # 将fd6指向fifo类型
	rm $tmp_fifofile
	 done

wait # 等待所有的后台子进程结束
exec 6>&- # 关闭df6


exit 0
 
此程序中的命令
mkfifo tmpfile

和linux中的命令
mknod tmpfile p

效果相同。区别是mkfifo为POSIX标准，因此推荐使用它。该命令创建了一个先入先出的管道文件，并为其分配文件标志符6。管道文件是进程之间通信的一种方式，注意这一句很重要
exec 6<>$tmp_fifofile      # 将fd6指向fifo类型

如果没有这句，在向文件$tmp_fifofile或者&6写入数据时，程序会被阻塞，直到有read读出了管道文件中的数据为止。而执行了上面这一句后就可以在程序运行期间不断向fifo类型的文件写入数据而不会阻塞，并且数据会被保存下来以供read程序读出。




shell script:
有两种方法执行shell scripts，一种是新产生一个shell，然后执行相应的shell scripts；一种是在当前shell下执行，不再启用其他shell。
新产生一个shell然后再执行scripts的方法是在scripts文件开头加入以下语句
#!/bin/sh
一般的script文件(.sh)即是这种用法。这种方法先启用新的sub-shell（新的子进程）,然后在其下执行命令。
另外一种方法就是上面说过的source命令，不再产生新的shell，而在当前shell下执行一切命令。
 
source:
source命令即点(.)命令。
在bash下输入man source，找到source命令解释处，可以看到解释”Read and execute commands from filename in the current shell environment and …”。从中可以知道，source命令是在当前进程中执行参数文件中的各个命令，而不是另起子进程(或sub-shell)。
 
exec:
在bash下输入man exec，找到exec命令解释处，可以看到有”No new process is created.”这样的解释，这就是说exec命令不产生新的子进程。那么exec与source的区别是什么呢？
exec命令在执行时会把当前的shell process关闭，然后换到后面的命令继续执行。
 
1. 系统调用exec是以新的进程去代替原来的进程，但进程的PID保持不变。因此，可以这样认为，exec系统调用并没有创建新的进程，只是替换了原来进程上下文的内容。原进程的代码段，数据段，堆栈段被新的进程所代替。
一个进程主要包括以下几个方面的内容:
(1)一个可以执行的程序
(2) 与进程相关联的全部数据(包括变量，内存，缓冲区)
(3)程序上下文(程序计数器PC,保存程序执行的位置)
 
2. exec是一个函数簇，由6个函数组成，分别是以excl和execv打头的。
执行exec系统调用，一般都是这样，用fork()函数新建立一个进程，然后让进程去执行exec调用。我们知道，在fork()建立新进程之后，父进各与子进程共享代码段，但数据空间是分开的，但父进程会把自己数据空间的内容copy到子进程中去，还有上下文也会copy到子进程中去。而为了提高效率，采用一种写时copy的策略，即创建子进程的时候，并不copy父进程的地址空间，父子进程拥有共同的地址空间，只有当子进程需要写入数据时(如向缓冲区写入数据),这时候会复制地址空间，复制缓冲区到子进程中去。从而父子进程拥有独立的地址空间。而对于fork()之后执行exec后，这种策略能够很好的提高效率，如果一开始就copy,那么exec之后，子进程的数据会被放弃，被新的进程所代替。
 
3. exec与system的区别
(1) exec是直接用新的进程去代替原来的程序运行，运行完毕之后不回到原先的程序中去。
(2) system是调用shell执行你的命令，system=fork+exec+waitpid,执行完毕之后，回到原先的程序中去。继续执行下面的部分。
总之，如果你用exec调用，首先应该fork一个新的进程，然后exec. 而system不需要你fork新进程，已经封装好了。
 
 
	exec I/O重定向详解及应用实例
	1、 基本概念（这是理解后面的知识的前提，请务必理解）
	a、 I/O重定向通常与 FD有关，shell的FD通常为10个，即 0～9；
	b、 常用FD有3个，为0（stdin，标准输入）、1（stdout，标准输出）、2（stderr，标准错误输出），默认与keyboard、monitor、monitor有关；
	 
	c、 用  来改变送出的数据信道(stdout, stderr)，使之输出到指定的档案；
	e、 0 是  与 1> 是一样的；
	f、 在IO重定向 中，stdout 与 stderr 的管道会先准备好，才会从 stdin 读进资料；
	g、 管道“|”(pipe line):上一个命令的 stdout 接到下一个命令的 stdin;
	h、 tee 命令是在不影响原本 I/O 的情况下，将 stdout 复制一份到档案去;
	i、 bash（ksh）执行命令的过程：分析命令－变量求值－命令替代（``和$( )）－重定向－通配符展开－确定路径－执行命令；
	j、 ( ) 将 command group 置于 sub-shell 去执行，也称 nested sub-shell，它有一点非常重要的特性是：继承父shell的Standard input, output, and error plus any other open file descriptors。
	k、 exec 命令：常用来替代当前 shell 并重新启动一个 shell，换句话说，并没有启动子 shell。使用这一命令时任何现有环境都将会被清除。exec 在对文件描述符进行操作的时候，也只有在这时，exec 不会覆盖你当前的 shell 环境。
	2、cmd &n 使用系统调用 dup (2) 复制文件描述符 n 并把结果用作标准输出
	&- 关闭标准输出
	n&- 表示将 n 号输出关闭
	上述所有形式都可以前导一个数字，此时建立的文件描述符由这个数字指定而不是缺省的 0 或 1。如：
	... 2>file 运行一个命令并把错误输出(文件描述符 2)定向到 file。
	... 2>&1 运行一个命令并把它的标准输出和输出合并。(严格的说是通过复制文件描述符 1 来建立文件描述符 2 ，但效果通常是合并了两个流。)
	我们对 2>&1详细说明一下 ：2>&1 也就是 FD2＝FD1 ，这里并不是说FD2 的值 等于FD1的值，因为 > 是改变送出的数据信道，也就是说把 FD2 的 “数据输出通道” 改为 FD1 的 “数据输出通道”。如果仅仅这样，这个改变好像没有什么作用，因为 FD2 的默认输出和 FD1的默认输出本来都是 monitor，一样的！
	但是，当 FD1 是其他文件，甚至是其他 FD 时，这个就具有特殊的用途了。请大家务必理解这一点。
	3、 如果 stdin, stdout, stderr 进行了重定向或关闭, 但没有保存原来的 FD, 可以将其恢复到 default 状态吗?
	*** 如果关闭了stdin，因为会导致退出，那肯定不能恢复。
	*** 如果重定向或关闭 stdout和stderr其中之一，可以恢复，因为他们默认均是送往monitor（但不知会否有其他影响）。如恢复重定向或关闭的 stdout： exec 1>&2 ，恢复重定向或关闭的stderr：exec 2>&1。
	*** 如果stdout和stderr全部都关闭了，又没有保存原来的FD，可以用：exec 1>/dev/tty 恢复。
	4、 cmd >a 2>a 和 cmd >a 2>&1 为什么不同？
	cmd >a 2>a ：stdout和stderr都直接送往文件 a ，a文件会被打开两遍，由此导致stdout和stderr互相覆盖。
	cmd >a 2>&1 ：stdout直接送往文件a ，stderr是继承了FD1的管道之后，再被送往文件a 。a文件只被打开一遍，就是FD1将其打开。
	我想：他们的不同点在于：
	cmd >a 2>a 相当于使用了两个互相竞争使用文件a的管道；
	而cmd >a 2>&1 只使用了一个管道，但在其源头已经包括了stdout和stderr。
	从IO效率上来讲，cmd >a 2>&1的效率应该更高！
	exec 0exec 1>outfilename # 打开文件outfilename作为stdout
	exec 2>errfilename # 打开文件 errfilename作为 stderr
	exec 0&- # 关闭 FD1
	exec 5>&- # 关闭 FD5
	[linuxidc.com@linuxidc.com shell]$ cat 1
	11 22 33 44 55
	66 22 33 11 33
	324 25 63 634 745
	[linuxidc.com@linuxidc.com shell]$ cat 2
	> 1.txt
	exec 4<&1
	exec 1> 1.txt
	while read  line
	do
	echo $line
	done < 1
	exec 1<&4
	exec 4>&-
	[linuxidc.com@linuxidc.com shell]$ sh ./2
	[linuxidc.com@linuxidc.com shell]$ cat 1.txt
	11 22 33 44 55
	66 22 33 11 33
	324 25 63 634 745
	[linuxidc.com@linuxidc.com shell]$