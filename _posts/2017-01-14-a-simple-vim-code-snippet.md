---
layout: post
title: vim换行及删除空行
category: tech
tags: vim
---

![](https://cdn.kelu.org/blog/2017/01/687474703a2f2f692e696d6775722e636f6d2f486e7047472e706e67.png)

两个简单的命令，但是好容易忘记Orz。正则表达式用的还不熟练。在选择模式下,

* 空格替换为换行

        :%s/ /\r/g 

* 删除空行 

        :g/^$/d

参考资料：

* [blog - CSDN](http://blog.csdn.net/tterminator/article/details/51610778)
