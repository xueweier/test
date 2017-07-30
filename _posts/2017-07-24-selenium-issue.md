---
layout: post
title:   Selenium 的一些错误总结
category: tech
tags:  python selenium
---
![](/assets/img/python.jpg)

一些错误总结。也不一定解决了，比较乱，就列出来看看，等都解决了再整理下。

* selenium.common.exceptions.WebDriverException: Message: chrome not reachable

	不太懂，没解决。

#  Ubuntu 无界面运行

	xvfb & pyvirtualdisplay 

	$ sudo apt-get install xvfb python-pip
	$ sudo pip install pyvirtualdisplay

	1.  `# 运行 Xvfb`
	2.  `Xvfb  :0  -screen 0  800x600x24  >>  /tmp/Xvfb.out  2>&1  &`
	3.  `export DISPLAY=:0`

* [Headless Browser Testing with Chrome and Firefox](http://fgimian.github.io/blog/2014/04/06/headless-browser-testing-with-chrome-and-firefox/)
* [Linux 无界面使用 selenium](http://jayi.leanote.com/post/Linux-无界面使用-selenium)
* [Ubuntu下配置Selenium运行环境](http://www.itfanr.cc/2016/10/19/configuration-the-selenium-running-environment-in-ubuntu/)
* [如何在无显示器的ubuntu下跑前端测试](https://my.oschina.net/zjzhai/blog/295288)