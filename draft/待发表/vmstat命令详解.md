### （总结）Linux监控工具vmstat命令详解

一、前言

很显然从名字中我们就可以知道vmstat是一个查看虚拟内存（Virtual Memory）使用状况的工具，但是怎样通过vmstat来发现系统中的瓶颈呢？在回答这个问题前，还是让我们回顾一下Linux中关于虚拟内存相关内容。

二、虚拟内存原理

在系统中运行的每个进程都需要使用到内存，但不是每个进程都需要每时每刻使用系统分配的内存空间。当系统运行所需内存超过实际的物理内存，内核会释放某些进程所占用但未使用的部分或所有物理内存，将这部分资料存储在磁盘上直到进程下一次调用，并将释放出的内存提供给有需要的进程使用。

在Linux内存管理中，主要是通过“调页Paging”和“交换Swapping”来完成上述的内存调度。调页算法是将内存中最近不常使用的页面换到磁盘上，把活动页面保留在内存中供进程使用。交换技术是将整个进程，而不是部分页面，全部交换到磁盘上。

分页(Page)写入磁盘的过程被称作Page-Out，分页(Page)从磁盘重新回到内存的过程被称作Page-In。当内核需要一个分页时，但发现此分页不在物理内存中(因为已经被Page-Out了)，此时就发生了分页错误（Page Fault）。

当系统内核发现可运行内存变少时，就会通过Page-Out来释放一部分物理内存。经管Page-Out不是经常发生，但是如果Page-out频繁不断的发生，直到当内核管理分页的时间超过运行程式的时间时，系统效能会急剧下降。这时的系统已经运行非常慢或进入暂停状态，这种状态亦被称作thrashing(颠簸)。

三、vmstat详解

1.用法

vmstat [-a] [-n] [-S unit] [delay [ count]]
vmstat [-s] [-n] [-S unit]
vmstat [-m] [-n] [delay [ count]]
vmstat [-d] [-n] [delay [ count]]
vmstat [-p disk partition] [-n] [delay [ count]]
vmstat [-f]
vmstat [-V]

-a：显示活跃和非活跃内存

-f：显示从系统启动至今的fork数量 。

-m：显示slabinfo

-n：只在开始时显示一次各字段名称。

-s：显示内存相关统计信息及多种系统活动数量。

delay：刷新时间间隔。如果不指定，只显示一条结果。

count：刷新次数。如果不指定刷新次数，但指定了刷新时间间隔，这时刷新次数为无穷。

-d：显示磁盘相关统计信息。

-p：显示指定磁盘分区统计信息

-S：使用指定单位显示。参数有 k 、K 、m 、M ，分别代表1000、1024、1000000、1048576字节（byte）。默认单位为K（1024 bytes）

-V：显示vmstat版本信息。
2.使用说明



		r 表示运行队列(就是说多少个进程真的分配到CPU)，我测试的服务器目前CPU比较空闲，没什么程序在跑，当这个值超过了CPU数目，就会出现CPU瓶颈了。这个也和top的负载有关系，一般负载超过了3就比较高，超过了5就高，超过了10就不正常了，服务器的状态很危险。top的负载类似每秒的运行队列。如果运行队列过大，表示你的CPU很繁忙，一般会造成CPU使用率很高。
		
		b 表示阻塞的进程,这个不多说，进程阻塞，大家懂的。
		
		swpd 虚拟内存已使用的大小，如果大于0，表示你的机器物理内存不足了，如果不是程序内存泄露的原因，那么你该升级内存了或者把耗内存的任务迁移到其他机器。
		
		free   空闲的物理内存的大小，我的机器内存总共8G，剩余3415M。
		
		buff   Linux/Unix系统是用来存储，目录里面有什么内容，权限等的缓存，我本机大概占用300多M
		
		cache cache直接用来记忆我们打开的文件,给文件做缓冲，我本机大概占用300多M(这里是Linux/Unix的聪明之处，把空闲的物理内存的一部分拿来做文件和目录的缓存，是为了提高 程序执行的性能，当程序使用内存时，buffer/cached会很快地被使用。)
		
		si  每秒从磁盘读入虚拟内存的大小，如果这个值大于0，表示物理内存不够用或者内存泄露了，要查找耗内存进程解决掉。我的机器内存充裕，一切正常。
		
		so  每秒虚拟内存写入磁盘的大小，如果这个值大于0，同上。
		
		bi  块设备每秒接收的块数量，这里的块设备是指系统上所有的磁盘和其他块设备，默认块大小是1024byte，我本机上没什么IO操作，所以一直是0，但是我曾在处理拷贝大量数据(2-3T)的机器上看过可以达到140000/s，磁盘写入速度差不多140M每秒
		
		bo 块设备每秒发送的块数量，例如我们读取文件，bo就要大于0。bi和bo一般都要接近0，不然就是IO过于频繁，需要调整。
		
		in 每秒CPU的中断次数，包括时间中断
		
		cs 每秒上下文切换次数，例如我们调用系统函数，就要进行上下文切换，线程的切换，也要进程上下文切换，这个值要越小越好，太大了，要考虑调低线程或者进程的数目,例如在apache和nginx这种web服务器中，我们一般做性能测试时会进行几千并发甚至几万并发的测试，选择web服务器的进程可以由进程或者线程的峰值一直下调，压测，直到cs到一个比较小的值，这个进程和线程数就是比较合适的值了。系统调用也是，每次调用系统函数，我们的代码就会进入内核空间，导致上下文切换，这个是很耗资源，也要尽量避免频繁调用系统函数。上下文切换次数过多表示你的CPU大部分浪费在上下文切换，导致CPU干正经事的时间少了，CPU没有充分利用，是不可取的。
		
		us 用户CPU时间，我曾经在一个做加密解密很频繁的服务器上，可以看到us接近100,r运行队列达到80(机器在做压力测试，性能表现不佳)。
		
		sy 系统CPU时间，如果太高，表示系统调用时间长，例如是IO操作频繁。
		
		id  空闲 CPU时间，一般来说，id + us + sy = 100,一般我认为id是空闲CPU使用率，us是用户CPU使用率，sy是系统CPU使用率。
		
		wt 等待IO CPU时间。


例子1：每3秒输出一条结果



字段说明：

Procs（进程）：

r: 运行队列中进程数量，这个值也可以判断是否需要增加CPU。（长期大于1）
b: 等待IO的进程数量

Memory（内存）：

swpd: 使用虚拟内存大小

注意：如果swpd的值不为0，但是SI，SO的值长期为0，这种情况不会影响系统性能。
free: 空闲物理内存大小
buff: 用作缓冲的内存大小
cache: 用作缓存的内存大小

注意：如果cache的值大的时候，说明cache处的文件数多，如果频繁访问到的文件都能被cache处，那么磁盘的读IO bi会非常小。

Swap：

si: 每秒从交换区写到内存的大小，由磁盘调入内存
so: 每秒写入交换区的内存大小，由内存调入磁盘

注意：内存够用的时候，这2个值都是0，如果这2个值长期大于0时，系统性能会受到影响，磁盘IO和CPU资源都会被消耗。有些朋友看到空闲内存（free）很少的或接近于0时，就认为内存不够用了，不能光看这一点，还要结合si和so，如果free很少，但是si和so也很少（大多时候是0），那么不用担心，系统性能这时不会受到影响的。

IO：（现在的Linux版本块的大小为1kb）

bi: 每秒读取的块数
bo: 每秒写入的块数

注意：随机磁盘读写的时候，这2个值越大（如超出1024k)，能看到CPU在IO等待的值也会越大。

系统：

in: 每秒中断数，包括时钟中断。
cs: 每秒上下文切换数。

注意：上面2个值越大，会看到由内核消耗的CPU时间会越大。

CPU（以百分比表示）：

us: 用户进程执行时间百分比(user time)

注意： us的值比较高时，说明用户进程消耗的CPU时间多，但是如果长期超50%的使用，那么我们就该考虑优化程序算法或者进行加速。

sy: 内核系统进程执行时间百分比(system time)

注意：sy的值高时，说明系统内核消耗的CPU资源多，这并不是良性表现，我们应该检查原因。

wa: IO等待时间百分比

注意：wa的值高时，说明IO等待比较严重，这可能由于磁盘大量作随机访问造成，也有可能磁盘出现瓶颈（块操作）。

id: 空闲时间百分比

例子2：显示活跃和非活跃内存



使用-a选项显示活跃和非活跃内存时，所显示的内容除增加inact和active外，其他显示内容与例子1相同。

字段说明：

Memory（内存）：

inact: 非活跃内存大小（当使用-a选项时显示）
active: 活跃的内存大小（当使用-a选项时显示）

总结：

目前说来，对于服务器监控有用处的度量主要有：

r（运行队列）
pi（页导入）
us（用户CPU）
sy（系统CPU）
id（空闲）
注意：如果r经常大于4 ，且id经常少于40，表示cpu的负荷很重。如果bi，bo 长期不等于0，表示内存不足。

通过VMSTAT识别CPU瓶颈：
r（运行队列）展示了正在执行和等待CPU资源的任务个数。当这个值超过了CPU数目，就会出现CPU瓶颈了。

Linux下查看CPU核心数的命令：
cat /proc/cpuinfo|grep processor|wc -l

当r值超过了CPU个数，就会出现CPU瓶颈，解决办法大体几种：

1. 最简单的就是增加CPU个数和核数
2. 通过调整任务执行时间，如大任务放到系统不繁忙的情况下进行执行，进尔平衡系统任务
3. 调整已有任务的优先级

通过vmstat识别CPU满负荷：

首先需要声明一点的是，vmstat中CPU的度量是百分比的。当us＋sy的值接近100的时候，表示CPU正在接近满负荷工作。但要注意的是，CPU 满负荷工作并不能说明什么，Linux总是试图要CPU尽可能的繁忙，使得任务的吞吐量最大化。唯一能够确定CPU瓶颈的还是r（运行队列）的值。

通过vmstat识别RAM瓶颈：

数据库服务器都只有有限的RAM，出现内存争用现象是Oracle的常见问题。

首先用free查看RAM的数量：
[oracle@oracle-db02 ~]$ free
total       used       free     shared    buffers     cached
Mem:       2074924    2071112       3812          0      40616    1598656
-/+ buffers/cache:     431840    1643084
Swap:      3068404     195804    2872600

当内存的需求大于RAM的数量，服务器启动了虚拟内存机制，通过虚拟内存，可以将RAM段移到SWAP DISK的特殊磁盘段上，这样会 出现虚拟内存的页导出和页导入现象，页导出并不能说明RAM瓶颈，虚拟内存系统经常会对内存段进行页导出，但页导入操作就表明了服务器需要更多的内存了， 页导入需要从SWAP DISK上将内存段复制回RAM，导致服务器速度变慢。

解决的办法有几种：

1. 最简单的，加大RAM；
2. 改小SGA，使得对RAM需求减少；
3. 减少RAM的需求。（如：减少PGA）

参考文档，本人做了相关修改和说明：

http://hi.baidu.com/imlidapeng/blog/item/51872329329ab8335243c1c9.html

http://qa.taobao.com/?p=2269


Linux vmstat命令实战详解
vmstat命令是最常见的Linux/Unix监控工具，可以展现给定时间间隔的服务器的状态值,包括服务器的CPU使用率，内存使用，虚拟内存交换情况,IO读写情况。这个命令是我查看Linux/Unix最喜爱的命令，一个是Linux/Unix都支持，二是相比top，我可以看到整个机器的CPU,内存,IO的使用情况，而不是单单看到各个进程的CPU使用率和内存使用率(使用场景不一样)。

一般vmstat工具的使用是通过两个数字参数来完成的，第一个参数是采样的时间间隔数，单位是秒，第二个参数是采样的次数，如:

root@ubuntu:~# vmstat 2 1
procs -----------memory---------- ---swap-- -----io---- -system-- ----cpu----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa
 1  0      0 3498472 315836 3819540    0    0     0     1    2    0  0  0 100  0
2表示每个两秒采集一次服务器状态，1表示只采集一次。

实际上，在应用过程中，我们会在一段时间内一直监控，不想监控直接结束vmstat就行了,例如:

复制代码
root@ubuntu:~# vmstat 2  
procs -----------memory---------- ---swap-- -----io---- -system-- ----cpu----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa
 1  0      0 3499840 315836 3819660    0    0     0     1    2    0  0  0 100  0
 0  0      0 3499584 315836 3819660    0    0     0     0   88  158  0  0 100  0
 0  0      0 3499708 315836 3819660    0    0     0     2   86  162  0  0 100  0
 0  0      0 3499708 315836 3819660    0    0     0    10   81  151  0  0 100  0
 1  0      0 3499732 315836 3819660    0    0     0     2   83  154  0  0 100  0
复制代码
这表示vmstat每2秒采集数据，一直采集，直到我结束程序，这里采集了5次数据我就结束了程序。

好了，命令介绍完毕，现在开始实战讲解每个参数的意思。

r 表示运行队列(就是说多少个进程真的分配到CPU)，我测试的服务器目前CPU比较空闲，没什么程序在跑，当这个值超过了CPU数目，就会出现CPU瓶颈了。这个也和top的负载有关系，一般负载超过了3就比较高，超过了5就高，超过了10就不正常了，服务器的状态很危险。top的负载类似每秒的运行队列。如果运行队列过大，表示你的CPU很繁忙，一般会造成CPU使用率很高。

b 表示阻塞的进程,这个不多说，进程阻塞，大家懂的。

swpd 虚拟内存已使用的大小，如果大于0，表示你的机器物理内存不足了，如果不是程序内存泄露的原因，那么你该升级内存了或者把耗内存的任务迁移到其他机器。

free   空闲的物理内存的大小，我的机器内存总共8G，剩余3415M。

buff   Linux/Unix系统是用来存储，目录里面有什么内容，权限等的缓存，我本机大概占用300多M

cache cache直接用来记忆我们打开的文件,给文件做缓冲，我本机大概占用300多M(这里是Linux/Unix的聪明之处，把空闲的物理内存的一部分拿来做文件和目录的缓存，是为了提高 程序执行的性能，当程序使用内存时，buffer/cached会很快地被使用。)

si  每秒从磁盘读入虚拟内存的大小，如果这个值大于0，表示物理内存不够用或者内存泄露了，要查找耗内存进程解决掉。我的机器内存充裕，一切正常。

so  每秒虚拟内存写入磁盘的大小，如果这个值大于0，同上。

bi  块设备每秒接收的块数量，这里的块设备是指系统上所有的磁盘和其他块设备，默认块大小是1024byte，我本机上没什么IO操作，所以一直是0，但是我曾在处理拷贝大量数据(2-3T)的机器上看过可以达到140000/s，磁盘写入速度差不多140M每秒

bo 块设备每秒发送的块数量，例如我们读取文件，bo就要大于0。bi和bo一般都要接近0，不然就是IO过于频繁，需要调整。

in 每秒CPU的中断次数，包括时间中断

cs 每秒上下文切换次数，例如我们调用系统函数，就要进行上下文切换，线程的切换，也要进程上下文切换，这个值要越小越好，太大了，要考虑调低线程或者进程的数目,例如在apache和nginx这种web服务器中，我们一般做性能测试时会进行几千并发甚至几万并发的测试，选择web服务器的进程可以由进程或者线程的峰值一直下调，压测，直到cs到一个比较小的值，这个进程和线程数就是比较合适的值了。系统调用也是，每次调用系统函数，我们的代码就会进入内核空间，导致上下文切换，这个是很耗资源，也要尽量避免频繁调用系统函数。上下文切换次数过多表示你的CPU大部分浪费在上下文切换，导致CPU干正经事的时间少了，CPU没有充分利用，是不可取的。

us 用户CPU时间，我曾经在一个做加密解密很频繁的服务器上，可以看到us接近100,r运行队列达到80(机器在做压力测试，性能表现不佳)。

sy 系统CPU时间，如果太高，表示系统调用时间长，例如是IO操作频繁。

id  空闲 CPU时间，一般来说，id + us + sy = 100,一般我认为id是空闲CPU使用率，us是用户CPU使用率，sy是系统CPU使用率。

wt 等待IO CPU时间。


IOSTAT输出结果解析
1. /proc/partitions

iostat 的数据的主要来源是 /proc/partitions，所以需要先看看
/proc/partitions 中有些什么。

# cat /proc/partitions
major minor #blocks name rio rmerge rsect ruse wio wmerge wsect wuse running use aveq

3 0 19535040 hda 12524 31127 344371 344360 12941 25534 308434 1097290 -1 15800720 28214662
3 1 7172991 hda1 13 71 168 140 0 0 0 0 0 140 140
3 2 1 hda2 0 0 0 0 0 0 0 0 0 0 0
3 5 5116671 hda5 100 477 665 620 1 1 2 30 0 610 650
3 6 265041 hda6 518 92 4616 2770 257 3375 29056 143880 0 46520 146650
3 7 6980211 hda7 11889 30475 338890 340740 12683 22158 279376 953380 0 509350 1294120

major: 主设备号。3 代表 hda。
minor: 次设备号。7 代表 No.7 分区。
#blocks: 设备总块数 (1024 bytes/block)。19535040*1024 => 20003880960(bytes) ~2G
name: 设备名称。如 hda7。

rio: 完成的读 I/O 设备总次数。指真正向 I/O 设备发起并完成的读操作数目，
也就是那些放到 I/O 队列中的读请求。注意很多进程发起的读操作
(read())很可能会和其他的操作进行 merge，不一定每个 read() 调用
都引起一个 I/O 请求。
rmerge: 进行了 merge 的读操作数目。
rsect: 读扇区总数 (512 bytes/sector)

ruse: 从进入读队列到读操作完成的时间累积 (毫秒)。上面的例子显示从开机
开始，读 hda7 操作共用了约340秒。

wio: 完成的写 I/O 设备总次数。
wmerge: 进行了 merge 的写操作数目。
wsect: 写扇区总数
wuse: 从进入写队列到写操作完成的时间累积 (毫秒)

running: 已进入 I/O 请求队列，等待进行设备操作的请求总数。上面的例子显
示 hda7 上的请求队列长度为 0。

use: 扣除重叠等待时间的净等待时间 (毫秒)。一般比 (ruse+wuse) 要小。比
如 5 个读请求同时等待了 1 毫秒，那么 ruse值为5ms, 而 use值为
1ms。use 也可以理解为I/O队列处于不为空状态的总时间。hda7 的I/O
队列非空时间为 509 秒，约合8分半钟。

aveq: 在队列中总的等待时间累积 (毫秒) (约等于ruse+wuse)
[oracle@ora9i ~]$ iostat -x
Linux 2.6.9-78.ELsmp (ora9i.rui)        08/03/2009
avg-cpu: %user   %nice    %sys %iowait   %idle
           0.10    0.28    3.18    0.55   95.88
Device:    rrqm/s wrqm/s   r/s   w/s rsec/s wsec/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await svctm %util
hdc          0.01   0.00 0.01 0.00    0.05    0.00     0.03     0.00     8.31     0.00    1.51   1.51   0.00
sda          0.17   3.54 1.57 2.63   43.42   49.40    21.71    24.70    22.07     0.02    4.34   2.27   0.96
sda1         0.14   3.54 1.57 2.63   43.34   49.40    21.67    24.70    22.06     0.02    4.34   2.27   0.96
sda2         0.03   0.00 0.00 0.00    0.04    0.00     0.02     0.00    28.38     0.00    6.09   2.69   0.00
sdb          0.03   0.00 0.00 0.00    0.12    0.01     0.06     0.00    43.85     0.00    7.37   3.45   0.00
sdb1         0.02   0.00 0.00 0.00    0.06    0.01     0.03     0.00    33.11     0.00   10.49   4.64   0.00
Device:
                     This column gives the device (or partition) name, which is dis-
                     played as hdiskn with 2.2 kernels, for the nth device. It is dis-
                     played as devm-n with 2.4 kernels, where m is the major number of
                     the device, and n a distinctive number. With newer kernels, the
                     device name as listed in the /dev directory is displayed.
              tps
                     Indicate the number of transfers per second that were issued to
                     the device. A transfer is an I/O request to the device. Multiple
                     logical requests can be combined into a single I/O request to the
                     device. A transfer is of indeterminate size.
              Blk_read/s
                     Indicate the amount of data read from the drive expressed in a
                     number of blocks per second. Blocks are equivalent to sectors
                     with 2.4 kernels and newer and therefore have a size of 512
                     bytes. With older kernels, a block is of indeterminate size.
              Blk_wrtn/s
                     Indicate the amount of data written to the drive expressed in a
                     number of blocks per second.
              Blk_read
                     The total number of blocks read.
              Blk_wrtn
                     The total number of blocks written.
              kB_read/s
                     Indicate the amount of data read from the drive expressed in
                     kilobytes per second. Data displayed are valid only with kernels
                     2.4 and newer.
              kB_wrtn/s
                     Indicate the amount of data written to the drive expressed in
                     kilobytes per second. Data displayed are valid only with kernels
                     2.4 and newer.
               kB_read
                     The total number of kilobytes read. Data displayed are valid only
                     with kernels 2.4 and newer.
              kB_wrtn
                     The total number of kilobytes written. Data displayed are valid
                     only with kernels 2.4 and newer.
              rrqm/s
                     The number of read requests merged per second that were issued to
                     the device.
              wrqm/s
                     The number of write requests merged per second that were issued
                     to the device.
              r/s
                     The number of read requests that were issued to the device per
                     second.
              w/s
                     The number of write requests that were issued to the device per
                     second.
              rsec/s
                     The number of sectors read from the device per second.
              wsec/s
                     The number of sectors written to the device per second.
              rkB/s
                     The number of kilobytes read from the device per second.
              wkB/s
                     The number of kilobytes written to the device per second.
              avgrq-sz
                     The average size (in sectors) of the requests that were issued to
                     the device.
              avgqu-sz
                     The average queue length of the requests that were issued to the device.
              await
                     The average time (in milliseconds) for I/O requests issued to the
                     device to be served. This includes the time spent by the requests
                     in queue and the time spent servicing them.
              svctm
                     The average service time (in milliseconds) for I/O requests that
                     were issued to the device.
              %util
                     Percentage of CPU time during which I/O requests were issued to
                     the device (bandwidth utilization for the device). Device satura-
                     tion occurs when this value is close to 100%.
如果 %util 接近 100%，说明产生的I/O请求太多，I/O系统已经满负荷，该磁盘
可能存在瓶颈。

svctm 一般要小于 await (因为同时等待的请求的等待时间被重复计算了)，
svctm 的大小一般和磁盘性能有关，CPU/内存的负荷也会对其有影响，请求过多
也会间接导致 svctm 的增加。await 的大小一般取决于服务时间(svctm) 以及 
I/O 队列的长度和 I/O 请求的发出模式。如果 svctm 比较接近 await，说明 
I/O 几乎没有等待时间；如果 await 远大于 svctm，说明 I/O 队列太长，应用
得到的响应时间变慢，如果响应时间超过了用户可以容许的范围，这时可以考虑
更换更快的磁盘，调整内核 elevator 算法，优化应用，或者升级 CPU。

队列长度(avgqu-sz)也可作为衡量系统 I/O 负荷的指标，但由于 avgqu-sz 是
按照单位时间的平均值，所以不能反映瞬间的 I/O 洪水。


3. I/O 系统 vs. 超市排队

举一个例子，我们在超市排队 checkout 时，怎么决定该去哪个交款台呢? 首当
是看排的队人数，5个人总比20人要快吧? 除了数人头，我们也常常看看前面人
购买的东西多少，如果前面有个采购了一星期食品的大妈，那么可以考虑换个队
排了。还有就是收银员的速度了，如果碰上了连钱都点不清楚的新手，那就有的
等了。另外，时机也很重要，可能 5 分钟前还人满为患的收款台，现在已是人
去楼空，这时候交款可是很爽啊，当然，前提是那过去的 5 分钟里所做的事情
比排队要有意义 (不过我还没发现什么事情比排队还无聊的)。

I/O 系统也和超市排队有很多类似之处:

r/s+w/s 类似于交款人的总数
平均队列长度(avgqu-sz)类似于单位时间里平均排队人的个数
平均服务时间(svctm)类似于收银员的收款速度
平均等待时间(await)类似于平均每人的等待时间
平均I/O数据(avgrq-sz)类似于平均每人所买的东西多少
I/O 操作率 (%util)类似于收款台前有人排队的时间比例。

我们可以根据这些数据分析出 I/O 请求的模式，以及 I/O 的速度和响应时间。


4. 一个例子

# iostat -x 1
avg-cpu: %user %nice %sys %idle
16.24 0.00 4.31 79.44
Device: rrqm/s wrqm/s r/s w/s rsec/s wsec/s rkB/s wkB/s avgrq-sz avgqu-sz await svctm %util
/dev/cciss/c0d0
0.00 44.90 1.02 27.55 8.16 579.59 4.08 289.80 20.57 22.35 78.21 5.00 14.29
/dev/cciss/c0d0p1
0.00 44.90 1.02 27.55 8.16 579.59 4.08 289.80 20.57 22.35 78.21 5.00 14.29
/dev/cciss/c0d0p2
0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00

上面的 iostat 输出表明秒有 28.57 次设备 I/O 操作: delta(io)/s = r/s +
w/s = 1.02+27.55 = 28.57 (次/秒) 其中写操作占了主体 (w:r = 27:1)。

平均每次设备 I/O 操作只需要 5ms 就可以完成，但每个 I/O 请求却需要等上 
78ms，为什么? 因为发出的 I/O 请求太多 (每秒钟约 29 个)，假设这些请求是
同时发出的，那么平均等待时间可以这样计算:

平均等待时间 = 单个 I/O 服务时间 * ( 1 + 2 + ... + 请求总数-1) / 请求总数

应用到上面的例子: 平均等待时间 = 5ms * (1+2+...+28)/29 = 70ms，和 
iostat 给出的 78ms 的平均等待时间很接近。这反过来表明 I/O 是同时发起的。

每秒发出的 I/O 请求很多 (约 29 个)，平均队列却不长 (只有 2 个 左右)，
这表明这 29 个请求的到来并不均匀，大部分时间 I/O 是空闲的。

一秒中有 14.29% 的时间 I/O 队列中是有请求的，也就是说，85.71% 的时间里 
I/O 系统无事可做，所有 29 个 I/O 请求都在142毫秒之内处理掉了。

delta(ruse+wuse)/delta(io) = await = 78.21 => delta(ruse+wuse)/s =
78.21 * delta(io)/s = 78.21*28.57 = 2232.8，表明每秒内的I/O请求总共需
要等待2232.8ms。所以平均队列长度应为 2232.8ms/1000ms = 2.23，而 iostat 
给出的平均队列长度 (avgqu-sz) 却为 22.35，为什么?! 因为 iostat 中有 
bug，avgqu-sz 值应为 2.23，而不是 22.35。


5. iostat 的 bug 修正

iostat.c 中是这样计算avgqu-sz的:

((double) current.aveq) / itv

aveq 的单位是毫秒，而 itv 是两次采样之间的间隔，单位是 jiffies。必须换
算成同样单位才能相除，所以正确的算法是:

((double) current.aveq) / itv * HZ / 1000

这样，上面 iostat 中输出的 avgqu-sz 值应为 2.23，而不是 22.3。
另外，util值的计算中做了 HZ 值的假设，不是很好，也需要修改。

--- sysstat-4.0.7/iostat.c.orig 2004-07-15 13:31:27.000000000 +0800
+++ sysstat-4.0.7/iostat.c 2004-07-15 13:37:34.000000000 +0800
@@ -370,7 +370,7 @@

nr_ios = current.rd_ios + current.wr_ios;
tput = nr_ios * HZ / itv;
- util = ((double) current.ticks) / itv;
+ util = ((double) current.ticks) / itv * HZ / 1000;
/* current.ticks (ms), itv (jiffies) */
svctm = tput ? util / tput : 0.0;
/* kernel gives ticks already in milliseconds for all platforms -> no need for further scaling */
@@ -387,12 +387,12 @@
((double) current.rd_sectors) / itv * HZ, ((double) current.wr_sectors) / itv * HZ,
((double) current.rd_sectors) / itv * HZ / 2, ((double) current.wr_sectors) / itv * HZ / 2,
arqsz,
- ((double) current.aveq) / itv,
+ ((double) current.aveq) / itv * HZ / 1000, /* aveq is in ms */
await,
/* again: ticks in milliseconds */
- svctm * 100.0,
+ svctm,
/* NB: the ticks output in current sard patches is biased to output 1000 ticks per second */
- util * 10.0);
+ util * 100.0);
}
}
}

一会儿 jiffies, 一会儿 ms，看来 iostat 的作者也被搞晕菜了。

这个问题在 systat 4.1.6 中得到了修正:

* The average I/O requests queue length as displayed by iostat -x was
wrongly calculated. This is now fixed.

但 Redhat 的 sysstat 版本有些太过时了 (4.0.7)。