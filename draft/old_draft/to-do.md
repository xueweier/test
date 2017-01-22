2015-1-19

linux命令之watch
exec
监控各种服务器状态。


copy了两个脚本。
安装了iftop

http://www.geekfan.net/5558/
的代码css













在Linux中有很多的流量监控工具，它们可以监控、分类网络流量，以花哨的图形用户界面提供实时流量分析报告。大多数这些工具（例如：ntopng ,  iftop ）都是基于libpcap 库的 ，这个函数库是用来截取流经网卡的数据包的，可在用户空间用来监视分析网络流量。尽管这些工具功能齐全，然而基于libpcap库的流量监控工具无法处理高速（Gb以上）的网络接口，原因是由于在用户空间做数据包截取的系统开销过高所致。

在本文中我们介绍一种简单的Shell 脚本，它可以监控网络流量而且不依赖于缓慢的libpcap库。这些脚本支持Gb以上规模的高速网络接口，如果你对“汇聚型”的网络流量感兴趣的话，它们可统计每个网络接口上的流量。

脚本主要是基于sysfs虚拟文件系统，这是由内核用来将设备或驱动相关的信息输出到用户空间的一种机制。网络接口的相关分析数据会通过“/sys/class/net/<ethX>/statistics”输出。

举个例子，eth0的网口上分析报告会输出到这些文件中：

    /sys/class/net/eth0/statistics/rx_packets: 收到的数据包数据
    /sys/class/net/eth0/statistics/tx_packets: 传输的数据包数量
    /sys/class/net/eth0/statistics/rx_bytes: 接收的字节数
    /sys/class/net/eth0/statistics/tx_bytes: 传输的字节数
    /sys/class/net/eth0/statistics/rx_dropped: 收包时丢弃的数据包
    /sys/class/net/eth0/statistics/tx_dropped: 发包时丢弃的数据包

这些数据会根据内核数据发生变更的时候自动刷新。因此，你可以编写一系列的脚本进行分析并计算流量统计。下面就是这样的脚本（感谢 joemiller 提供）。第一个脚本是统计每秒数据量，包含接收（RX）或发送（TX）。而后面的则是一个描述网络传输中的接收（RX）发送(TX)带宽。这些脚本中安装不需要任何的工具。

测量网口每秒数据包：






