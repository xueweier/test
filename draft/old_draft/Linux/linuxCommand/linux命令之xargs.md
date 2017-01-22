# linux命令之xargs

xargs是传递参数的一个过滤器, 也是组合多个命令的一个工具. 它把一个数据流分割为一些足够小的块, 以方便过滤器和命令进行处理。

通过管道传递给xargs以后，换行和空白将被空格取代.

	ls | xargs -p -l gzip 使用gzips压缩当前目录下的每个文件, 每次压缩一个, 并且在每次压缩 前都提示用户.
		ls | xargs -n 8 echo以每行8列的形式列出当前目录下的所有文件.		find / -type f -print0 | xargs -0 grep -liwZ GUI | xargs -0 rm -f # 删除任何包含"GUI"的文件
		grep -rliwZ GUI / | xargs -0 rm -f  # 删除任何包含"GUI"的文件
		ls . | xargs -i -t cp ./{} $1
			-t 是 "verbose" (输出命令行到stderr) 选项. -i 是"替换字符串"选项.		{} 是输出文本的替换点. 这与在"find"命令中使用{}的情况很相像.		列出当前目录下的所有文件(ls .),		将 "ls" 的输出作为参数传递到 "xargs"(-i -t 选项) 中, 然后拷贝(cp)这些参数({})到一个新目录中($1).		最终的结果和下边的命令等价,		除非有文件名中嵌入了"空白"字符.

xargs是给命令传递参数的一个过滤器，也是组合多个命令的一个工具。它把一个数据流分割为一些足够小的块，以方便过滤器和命令进行处理。通常情况下，xargs从管道或者stdin中读取数据，但是它也能够从文件的输出中读取数据。xargs的默认命令是echo，这意味着通过管道传递给xargs的输入将会包含换行和空白，不过通过xargs的处理，换行和空白将被空格取代。

xargs 是一个强有力的命令，它能够捕获一个命令的输出，然后传递给另外一个命令，下面是一些如何有效使用xargs 的实用例子。

1. 当你尝试用rm 删除太多的文件，你可能得到一个错误信息：/bin/rm Argument list too long. 用xargs 去避免这个问题

find ~ -name ‘*.log’ -print0 | xargs -0 rm -f


2. 获得/etc/ 下所有*.conf 结尾的文件列表，有几种不同的方法能得到相同的结果，下面的例子仅仅是示范怎么实用xargs ，在这个例子中实用 xargs将find 命令的输出传递给ls -l

# find /etc -name "*.conf" | xargs ls –l


3. 假如你有一个文件包含了很多你希望下载的URL, 你能够使用xargs 下载所有链接

# cat url-list.txt | xargs wget –c


4. 查找所有的jpg 文件，并且压缩它

# find / -name *.jpg -type f -print | xargs tar -cvzf images.tar.gz


5. 拷贝所有的图片文件到一个外部的硬盘驱动

# ls *.jpg | xargs -n1 -i cp {} /external-hard-drive/directory
