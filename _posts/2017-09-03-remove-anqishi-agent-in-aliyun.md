---
layout: post
title: 卸载阿里云云盾(安骑士)
category: tech
tags: linux aliyun
---
![](https://cdn.kelu.org/blog/tags/aliyun.jpg)

我有在用阿里云的 ECS。自带的系统镜像里装了带有 root 权限的监控软件阿里云盾。

阿里云盾从诞生之初就陆续出现了一些问题，有两次闹的比较大，一次是误删文件：

![](https://cdn.kelu.org/blog/2017/09/18171441129022.png)

知乎上也有相关的讨论：[如何评论阿里云云盾负责人的这篇《危机时刻，我只心疼我们的客户》？](https://www.zhihu.com/question/35329012)

还有一次就是监控用户数据流量的传闻：

![](https://cdn.kelu.org/blog/2017/09/627df3ecly1fg4d2s8t0uj20tq1rek8a.jpg)

v2ex 上相关的讨论：<https://www.v2ex.com/t/364911>

随后阿里云官方也做出了回应：[《关于数据安全保护的声明》](https://yq.aliyun.com/articles/92120)
	


就我个人来说，服务器上安装的商业性软件，如果不是非常信任的厂家，还是建议卸载掉。这就像一个定时炸弹，我们无法掌控，随时都有可能爆掉。服务器数据的安全，应该掌握在自己手里，而非服务提供商的手中。

所以接下来就删删删了。

1.  执行以下命令下载安骑士 Agent 卸载脚本。

		wget http://update.aegis.aliyun.com/download/uninstall.sh

2.  依次执行以下命令卸载安骑士 Agent。

		chmod +x uninstall.sh
		./uninstall.sh

或者运行这个脚本：

		#!/bin/bash 
		rm -rf /usr/local/aegis 
		for A in $(ps aux | grep Ali | grep -v grep | awk '{print $2}') 
		do 
		  kill -9 $A; 
		done

# 参考资料

*   [安骑士卸载Agent](https://www.alibabacloud.com/help/zh/doc-detail/31777.htm)


