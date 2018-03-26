---
layout: post
title: 几个微信公众号开发时的错误代码
category: tech
tags: wechat
---

![](https://cdn.kelu.org/blog/tags/wechat.jpg)

开发公众号也有好一段时间了，说起来开发是蛮简单的。很久没看了，最近遇到了一些问题，小小总结一下。

### 客服接口65400错误

很久没有动过一个微信公众号了，今天发信息的时候发现发不通。看了日志，发现了这个错误——[Wechat][65400] please enable new custom service, or wait for a while if you have enabled hint: [0Lx7ja0168e303]。

想了一下，应该是没有添加客服吧（虽然之前使用没问题，貌似最近公众号升级了导致出现问题）。试着添加了一下客服，稍等5分钟左右，就好了。嗯嗯。

### 客服接口45015错误

	{  
	    "errcode":45015,  
	    "errmsg":"response out of time limit or subscription is canceled hint: [ZE1Uxa0498age8]"  
	}  

原因是当用户微信不活跃时间超过24小时（此时间当前是多少由腾讯定），不会将信息推送到用户微信公众号。

原先我们给用户发送的消息都是使用客服消息发送的，就会导致这种情况。如果是网站的服务通知，可以使用模板进行发送，这个没有时间限制。

当然模板功能也是要申请的，大概需要2天时间。每个账号可以同时使用15个模板。当前每个模板的日调用上限为 10 万次。


参考资料：

* [添加客服帐号时出错65400](http://www.henkuai.com/thread-14242-1-1.html)
* [模板消息接口 - 微信公众平台开发者文档](http://mp.weixin.qq.com/wiki/17/304c1885ea66dbedf7dc170d84999a9d.html)