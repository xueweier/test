---
layout: post
title: 嵌入式平台选择：树莓派 or BeagleBone Black（BBB）
category: tech
tags: rss linux BeagleBone-Black Raspberry-Pi
---

本文转载自[极客范](http://www.geekfan.net)，由 [小道空空](http://www.geekfan.net/author/wangqing/) 翻译自 [Michael Leonard](http://makezine.com/magazine/how-to-choose-the-right-platform-raspberry-pi-or-beaglebone-black/)。因为最近刚入了bbb，这篇文章总结的也很好，还有就是极客范也停止更新两个月了，所以转载过来。以下是转载内容：

已经有很多文章比较过Arduino、树莓派和BeagleBone Black（BBB），但本文的侧重点不同。我相信大家都会认为Arduino和另外两者明显属于不同的阵营，因为Arduino的用途完全不一样。我曾试图去寻找这样一篇文章但最终没有找到：它全面的比较树莓派和BBB的优缺点并分析各自的最佳适用领域。因此，我决定自己写一篇。
本文首先简要的介绍每个平台，然后从以下几方面深入的比较它们：

* 概况
* 拆箱
* 初次使用
* 接口
* 处理器
* 图形处理
* 音频
* 能耗
* 可扩展性
* 硬件易复制性
* 社区
* 让我们开始吧！

## 树莓派简介

Arduino是微控制器领域的开拓者，它开启了“制造者”革命；而了不起的树莓派则真正开始了微控制器革命。
对于公众来说，树莓派是第一个便宜（35美金）、易用的单片计算机。树莓派的创造者发现年长一代的学生出于需要对计算机技术都比较精通。但年轻一代的学生在这方面则逊色很多，他们对计算机技术的了解离他们所需要掌握的差很远。于是树莓派这个既便宜但性能又比较强大的微型计算机诞生了。它使得年轻一代的学生可以很方便的接触和深入学习计算机技术。
如果你想更多的了解树莓派，那么我推荐你去阅读官方的“关于”和“常见问题”网页。树莓派诞生的背后故事还是很鼓励人心和值得一读的。

## BBB简介

BBB是易用微处理器领域的后来者。虽然它错过了推向市场的最佳时间，但它在产品的性能上得到了弥补。BBB继承了BeagleBoard产品家族的血统：体积小、性能强大、可扩展性强（便于工程师和艺术家等开发自己的创新项目）。
BeagleBoard家族最初是为了给业余爱好者提供一个相对低价的开发平台而设计的。这个平台包含了一个强大的新的片上系统（System on Chip, SoC)设备。最初的BeagleBoard目前售价125美元；它的继任者BeagleBoard-xM售价145美元。虽然它们功能强大，但是其相对“昂贵”的价格却无法吸引人去大量的购买。BeagleBoard小组在BeagleBoard-xM之后开发了BeagleBone，后者本质上是前者的精简版。虽然BeagleBone起点不错，但是其89美金的售价还是无法吸引众多业余爱好者们。BeagleBoard小组最终在2012年下半年发布了BeagleBone的升级版——BeagleBone Black（BBB）。从下面这幅图中你会看出为什么BeagleBoard小组给它起了这个名字。

BBB的正面

BBB继承了BeagleBone的体积并增加了相当多的有用功能，因此它也变成一个各方面都更加优秀的产品。最不可思议的是它的售价——仅45美金！
如果你想更多的了解BeagleBone和BBB，你可以访问其官方社区或制造商的社区主页。这是深入了解这些平台复杂细节的最佳方式。这也使得你更好的评估究竟BBB适不适合你的项目。

那么究竟是选择树莓派还是BBB？

到现在为止，我们对这两个平台都有了初步的了解。接下来我将客观的从各个方面去比较这它们，你可以从这些比较中去选择适合自己开发需求的平台。如果你发现问题或者觉得我漏掉了某些方面，你可以在本文后面留言。记得文明留言就行。

### 概况
下面的表格总结了树莓派（Rev.B）和BBB（Rev.A5B）的各项规格参数。从这里我们可以快速了解各个平台的性能。这个表格只比较了两者发货时的版本。后续文章将深入比较其本身及支撑其发展的生态系统。

### 开箱
我当时购买的树莓派被包装在一个普通的白色纸箱中，没有任何标记和配件。现在的树莓派则被包装在一个相对漂亮的盒子中。
我的BBB则是2013年参加TI实习生设计大赛时免费拿到的。它也被包装在一个专业的盒子中，并包含了一个mini-USB线和一张小的介绍卡。
获胜者：平手

### 初次使用
初次使用树莓派是比较费力的。它没有提供USB线，所以你必须自己买一个。此外，树莓派没有预装操作系统。你必须自己下载操作系统、烧录到SD卡中、然后用SD卡来启动它。
初次使用BBB则容易的多。你通过自带的USB线将它连接到电脑上之后它就自动启动起来了。虽然你可能需要安装驱动程序，但与树莓派相比，这要容易的多。
获胜者：BBB

### 总花费
这项比较会有点主观，因为每个人的实际情况不一样。如果你已经有了SD卡、USB线、HDMI线和键盘，那么树莓派不会给你带来额外的花费。
对于BBB来说，你不需要去购买额外的配件。但如果你想扩展它的功能，那你也许需要去购买MicroSD卡和micro-HDMI线。
此外，因为树莓派有两个USB口，你可能不需要一个USB HUB就可以顺利工作。但对于BBB，你可能需要购买一个USB HUB来同时使用键盘和鼠标（如果你用的不是无线键盘和鼠标的话）。
对我来说，BBB要比树莓派便宜些。但这部分需要考虑的因素很多，所有这里由你自己来决定哪个平台的总花费更低些。
获胜者：平手

### 接口
BBB总计有92个不同的接口（46个引脚）。虽然某些接口被预留了，但是大部分的接口可以通过重新配置来使用。下面是手册中列出的一些可能的接口：

3 I2C buses
CAN bus
SPI bus
4 timers
5 serial ports
65 GPIO pins
8 PWM outputs
7 analog inputs (1.8V max 12 bit A/D converters)
这些接口的存在使得BBB变得非常强大。我不知道还有哪个如此便宜的平台在这样的体积下还能提供如此丰富的接口。这些接口使得开发众多的BBB应用变得非常现实。
树莓派则只有26个引脚。这些引脚可以提供如下所示的接口：

8 GPIO pins
1 UART interface
1 SPI bus
1 I2C bus
这些不多的接口对于基于I2C、SPI或者UART的项目来说足够用了。树莓派的真正魅力在另一方面，我们稍后讨论它。
获胜者：BBB（毫无疑问）

### 处理器
处理器也许是决定平台运行速度的唯一重要因素。BBB的处理器运行速度为1GHz；树莓派则为700MHz。
为了方便进一步比较两者的性能，我们假设树莓派的处理器被超频到和德州仪器的AM3359一样的频率。
接着比较处理器的架构。树莓派的处理器采用的是老的ARMv6指令集，而BBB的处理器采用的是当前嵌入式系统中最流行的ARMv7指令集。
采用当今广泛使用的指令集的处理器可以被更多的软件支持。例如，一些操作系统已经不支持在ARMv6指令集上运行，例如，Ubuntu在2012年4月放弃了对ARMv6指令集的支持。
ARMv7相对与ARMv6指令集的另一个优势在于，使用ARMv7的处理器的实际性能更加强劲。ARMv7相对与ARMv6的优势还有很多，比如一些显著的改进：实现了超标量架构、包含了SIMD操作指令、改进了分支预测算法从而极大的提高了某些性能。
具体的讲，即使BBB和树莓派的处理器工作在同一频率，前者的运行速度也几乎是后者的两倍。（数据来源1：ARM A8运行速度为2000MIPS/MHz；数据来源2：ARM 11运行速度为1250MIPS/MHz）
获胜者：BBB

### 图形处理
树莓派在图形处理方面表现非常突出。由于集成了Videocore视频处理器，树莓派可以解码1080P的视频流、渲染OpenGL和甚至于运行Minecraft。除了令人印象深刻的图形处理，树莓派还提供了全尺寸的HDMI接口和用于低质量的混合视频输出接口。
上述这些都是BBB无法与之媲美的。BBB虽然有内嵌的图形处理能力，但是其性能有限，从而不支持1080P。它也提供了一个micro-HDMI视频接口用于连接显示器或电视。虽然通过一些插件板可以扩展其性能，但还是无法和树莓派的Videocore系统相提并论。
获胜者：树莓派

### 音频
在音频方面其实没有太多要比较的。BBB提供了可以用作音频输出的micro-HDMI接口；树莓派则提供了micro-HDMI和3.5mm的音频插口。所以树莓派要略胜一筹。
需要指出的是，现在市场上已经可以买到支持3.5mm音频输入和输出的BBB插件板。但因为它不是BBB的默认配置，所以我认为在这个类别中树莓派获胜。
获胜者：树莓派

### 功耗
说实话，这方面能找到的可靠数据少之又少。BBB的参考手册提供了一些数据；但是对于树莓派来说，很多人给出的数据差别很大，所以我也不确定哪些较为真实。其中显得最为可靠的数据表明树莓派的功耗比BBB要低一些。
如果你有关于树莓派或BBB更为可靠的功耗数据，请在后面的评论中留言。
获胜者：树莓派（基于不是很可靠的数据得出的结论）

### 可扩展性
我必须承认，当我一开始写这篇文章的时候，我认为BBB在可扩展性方面必胜无疑。这时因为当时我在设计自己的BBB插件板，而我已经知道有大量的BBB插件板存在。但当时我对树莓派的插件板数量并没有概念。需要指出的是，这里的插件板指的是可以增加BBB或树莓派功能的板子，而不是指数据线等各种附件。
首先我们看看BBB的插件板情况。在CircuitCo的官方网站上，我看到如下比较吸引我的插件板：

Breadboard、prototype和breakout插件板——这三种可以使得你很容易测试新加入BeagleBone的部件；
DVI插件板——允许你把BBB连接到具有DVI接口的显示器;
VGA插件板——允许你把BBB连接到具有VGA接口的显示器;
HDMI插件板——允许你把BBB连接到具有HDMI接口的设备。这个插件板最初是为BeagleBone设计的；但如果你不喜欢BBB提供的Micro-HDMI接口，你也可以把HDMI插件板用到BBB上;
LCD插件板——有几个不同的LCD插件板可选。通过它们你可以很容易的在BeagleBone上增加LCD显示屏;
照相机插件板——为BBB增加一个3.1MP像素的照相机。配合LCD插件板，你可以拥有自己的手持照相机;
音频插件板——包含了3.5mm的音频输出和输入接口;
电机插件板——包含的TI电机驱动可以驱动8个直流电机;
电源插件板——如果你需要经常移动你的开发板，那么你可能会用到它;
上述的列表并没有包含所有的插件板。列出的这些只是我认为会被广泛用到的。此外，还有其它一些更为专业的插件板，如ROV（在OpenROV项目中用于控制水下机器人传输实时流媒体）和Ninja（用于几乎使得一切变得自动化的Ninja平台中）。
看完上述的BBB插件板列表，你可以会问树莓派在这方面怎么和BBB竞争。我当时也这样问自己。事实上，树莓派的插件板特别稀少，而且没有一个好的“官方”列表来总结目前已知的树莓派插件板。
我找到的大多数树莓派插件板不是“breakout”板就是原型板。这些板子虽然有一定的用途，但并不具备一些杀手级的特点，而且也不专属于树莓派。
Cooking Hacks设计了一款“专属于”树莓派的插件板，如下图所示。通过这个插件，很多Arduino的扩展部件可以直接用于树莓派。

获胜者：BBB（PS：如果你计划购买树莓派但又打算使用Arduino的插件板，那么你或许应该直接购买Arduino）

### 硬件易复制性
这个类别对于本文的大多数读者来说可能并不重要。但对于技术用户或那些想最大化精简项目设计中用到的硬件的人来说却至关重要。树莓派和BBB都严重依赖于开源社区，让我们看看究竟哪一个平台更加开放些。
树莓派很不幸是基于私有的处理器平台。这意味着你无法获得其详细的数据，除非通过以下方法：

与Broadcom签订一个非公开的协议
向Broadcom提供一份商业计划
承诺批量购买这些处理器
虽然从网络可以搜到一些关于如何访问BCM2835寄存器的资料，但据我所知，关于处理器引脚的详细资料却无法搜到。作为对比，BBB使用的处理器的详细介绍和用户手册均可以从TI的相应产品页面上找到，而且处理器的最低购买数量也没有任何限制。
除了处理器，树莓派基金会还和RS和Farnell集团签订了独家生产协议，这意味着其电路原理图是严格保密的。
如果你想设计自己的树莓派衍生品或者想知道树莓派的各个部件是如何连接在一起的，那么Eben提供了树莓派Rev.B版本的电路图。但你还是需要向Broadcom承诺批量购买处理器。
相比较而言，BBB的所有资料，包括布局、电路图和参考文档都可以从BBB的wiki页面找到——那里包含了制造BBB所需的一切资料。
获胜者：BBB

### 社区
尽管我尽了最大努力，我还是无法找到每个平台社区大小的可靠数据。但因为树莓派截止2013年4月已售出一百万套，所以我认为树莓派要更加流行。媒体关于树莓派的报道也更多些。
这些考虑对于不熟悉Linux系统或者电子设计的人来说是非常有意义的。平台使用的人越多，意味着你可以搜的相关帮助和信息就越多。
Google深度搜索显示虽然BBB的变得越来越流行，但与树莓派相关的网页流量仍然是BBB的13倍之多。
获胜者：树莓派

## 总结

我们已经详细的比较了树莓派和BBB的各方面特性，下面将总结每个平台的适用领域。

### BBB更加适用的领域
连接大量传感器的项目——BBB提供的众多接口可以很好的满足这方面的需求；
需要高速处理能力但对体积也严格要求的项目——例如，那个包含了33个树莓派节点的集群项目如果使用BBB，那么花费会更低、而且性能会更强；
打算商用的项目——树莓派的封闭性使得你构建自己需要的最小系统变得很难；而基于开源的BBB，你可以很容易构建自己的最小系统；
嵌入式系统学习平台——虽然树莓派在嵌入式学习领域已经根深蒂固，但我认为BBB更适合用于嵌入式系统学习；
仅仅需要其“运行”的项目——BBB“即买即用”的特性（不需要自己去安装系统）可以为你节省很多时间。

### 树莓派更加适用的领域
多媒体项目——树莓派具有强大的图形处理能力并提供了丰富的多媒体接口;
社区驱动的项目——如果你的项目比较依赖社区的帮助，那么你应该选择具有活跃社区的树莓派；如果你不需要太多的社区帮助，那么你应该选择BBB，因为很多基于树莓派的项目可以很容易的移植到BBB上;
具备图形界面的学习平台——因为BBB在视频方面的处理能力不及树莓派，所以如果你打算在Linux图形界面下学习嵌入式开发，你可以选择树莓派。

### 两者均适用的领域
网络连接相关的项目——如果你的项目是向服务器更新数据或将其用作服务器，那么两个平台任选其一即可;
只想玩玩嵌入式系统——两个平台均可。

我希望此文可以对那些在购买树莓派还是BBB之间犹豫的人提供一些帮助。如果你还是无法确定但你又是“土豪”的话，我建议你两者都买。每个平台都用自己的特长，你可以用它们做不同的项目。



原文链接： Michael Leonard 翻译： 极客范 - 小道空空

译文链接： http://www.geekfan.net/5246/

[ 转载请保留原文出处、译者和译文链接。]
