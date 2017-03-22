---
layout: post
title: shadowsocks的设置
category: tech
tags: proxy shadowsocks
---

* 购买请戳这里 >>>[购买链接](http://wechat.kelu.org/charge)<<<
  
* L2TP协议请戳这里  >>>[L2TP](/tech/2017/03/15/L2TP-VPN-setting.html)<<<

* PPTP协议请戳这里  >>>[PPTP](/tech/2015/02/14/PPTP-VPN-setting.html)<<<

手机平板设备建议使用L2TP，电脑Mac使用Shadowsocks。

Shadowsocks 是当下一个蛮重要的科学上网工具，查询科研论文和开发程序必不可少。学会正确的上网姿势，是成为现代人的很重要的一步哦。

![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201701/QQ%E6%88%AA%E5%9B%BE20170121015337.jpg)

* 在[Windows](#windows)下的设置
* 在[Mac](#mac)下的设置

<span id="windows"></span>

# Windows 基本使用

* 下载程序： [下载链接][ss_w]。 解压到你希望运行程序的路径，例如：`C:\Program Files (x86)\ss`

* 在任务栏找到 Shadowsocks 图标，在 服务器 菜单添加服务器(首次双击运行程序自动打开)
![1](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201701/20170108223605.png)

* 选择 启用系统代理 来启用系统代理。

![2](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201701/20170108223622.png)
![3](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201701/20170108223615.png)


* PAC 模式和全局模式的区别：

    全局模式就是所有请求都通过 Shadowsocks 设置的服务器进行访问.
PAC 模式是智能判断该网站是否需要通过设置的服务器进行访问。



<span id="mac"></span>

# Mac 基本使用

* 下载程序： [下载链接][ss_x]。双击 dmg 文件安装。

* 在任务栏找到 Shadowsocks 图标，在 服务器 菜单添加服务器

![5](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201701/D28973C0-7E48-46BC-997F-6470261382C1.png)

* 选择 启用系统代理 来启用系统代理。

![4](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201701/4BFA4DCB-563A-453B-A4C7-942B25E85858.png)

* PAC 模式和全局模式的区别：

    全局模式就是所有请求都通过 Shadowsocks 设置的服务器进行访问.
PAC 模式是智能判断该网站是否需要通过设置的服务器进行访问。



# 高级使用

## PAC
* 可以编辑 PAC 文件来修改 PAC 设置。Shadowsocks 会监听文件变化，修改后会自动生效。
* 也可以从 GFWList（由第三方维护）更新 PAC 文件或使用在线 PAC URL.

## 服务器自动切换

* 负载均衡：随机选择服务器
* 高可用：根据延迟和丢包率自动选择服务器
* 累计丢包率：通过定时 ping 来测速和选择。如果要使用本功能，请打开菜单里的统计可用性。
* 也可以实现 IStrategy 接口来自定义切换规则，然后给我们发一个 pull request。

[ss_w]: http://wechat.kelu.org/download/kelussW.zip
[ss_x]: http://wechat.kelu.org/download/kelussX.zip
