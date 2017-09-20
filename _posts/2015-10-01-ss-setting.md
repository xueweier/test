---
layout: post
title: ss的设置
category: tech
tags: proxy shadowsocks
---

* 在[Windows](#windows)下的设置
* 在[Mac](#mac)下的设置
* 在[iPhone](#iphone)下的设置(中国区不行)
* 在[Android](#android)下的设置

<span id="windows"></span>
## 在Windows下的设置
需要在win7以及以上的系统中使用，不支持XP。

客户端:  
[链接一](https://github.com/shadowsocks/shadowsocks-csharp/releases/download/2.5.6/Shadowsocks-win-2.5.6.zip) 或者 [链接二](http://d.pr/15FBT)

2.运行`shadowsocks.exe`
3.右下角找到程序图标，右键图标，“服务器”--“编辑服务器”，如下图，设置好shadowsocks的账号信息，加密方式选择`aes-256-cfb`，点确定；

![shadowsocks_win_01.png](https://cdn.kelu.org/blog/2015/10/blog_29832-46181f83d46e0d4f.png)

4.再次右键程序图标，勾选“启用系统代理”。

![shadowsocks_win_02.png](https://cdn.kelu.org/blog/2015/10/blog_29832-00d735589b1de3b4.png)

5.接下来，该干嘛干嘛去(*ゝω・)


<span id="mac"></span>
## 在Mac下的设置
客户端：
[Shadowsocks for Mac](http://d.pr/1c1mK) - Shadowsocks GUI designed for OS X 10.7+

配置的过程和win是一样的，设置好以后，打开浏览器上网就OK了。
![image](https://cdn.kelu.org/blog/2015/10/blog_Shadowsocks-GUI-Mac-Menu.png)

![image](https://cdn.kelu.org/blog/2015/10/blog_Shadowsocks-GUI-Mac-Screenshot.png)


<span id="iphone"></span>
## 在iPhone下的设置
ps:目前这个应用在中国区没有办法下载，所以中国区的童鞋绕行使用VPN吧。

在appstore搜索下载`shadowsocks`([点击直接进入下载](https://itunes.apple.com/cn/app/shadowsocks/id665729974?mt=8)），app打开后就是一个浏览器，设置方法和windows一样。相比Android
版，iOS版只支持浏览器，有点弱爆了的感觉。iOS上还是VPN更方便。

<img style="width:500px" src="https://cdn.kelu.org/blog/2015/10/blog_Shadowsocks-iOS.png">

<span id="android"></span>
## 在Android下的设置
安卓下的“Shadowsocks”（[GooglePlay下载](https://play.google.com/store/apps/details?id=com.github.shadowsocks) [其它链接](http://d.pr/1cKli)），下载后无需root，设置好服务器和帐号信息后即可直接使用。

![image](https://cdn.kelu.org/blog/2015/10/blog_Shadowsocks-Android.jpg)

与iOS版本不同，android版是以VPN的方式运行的，也就是说不仅支持浏览器，而且支持其他App。
