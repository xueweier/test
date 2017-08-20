---
layout: post
title:  Linux 生成 source-list 源的快速方式
category: tech
tags: vps linux
---

今天在用阿里云的 Debian 8.6 的系统，一开始 apt-get 就提示源不存在，404了。 （摊手
 
然后发现了这个快速生成 source-list 的网站： <https://debgen.simplylinux.ch/>
 
感觉很像是以前的 jquery 简化版，通过选择一些 jquery 的组件，生成最小化的 jquery 文件。确实不错。

甚至还可以选择第三方组件。

![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201706/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-06-25%20%E4%B8%8B%E5%8D%889.40.39.png)

 我选择了最原始的选项，最后得出的代码如下：

    #------------------------------------------------------------------------------#
    #                   OFFICIAL DEBIAN REPOS                    
    #------------------------------------------------------------------------------#

    ###### Debian Main Repos
    deb http://deb.debian.org/debian/ oldstable main contrib non-free
    deb-src http://deb.debian.org/debian/ oldstable main contrib non-free

    deb http://deb.debian.org/debian/ oldstable-updates main contrib non-free
    deb-src http://deb.debian.org/debian/ oldstable-updates main contrib non-free

    deb http://deb.debian.org/debian-security oldstable/updates main
    deb-src http://deb.debian.org/debian-security oldstable/updates main

    deb http://ftp.debian.org/debian wheezy-backports main
    deb-src http://ftp.debian.org/debian wheezy-backports main
    
    
 ps：今天给博客侧边栏加了我的产品这个选项卡。原图在这里，^_^。   
    
![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201706/b4f963380cd79123802d6e23ac345982b3b78015.jpg)
