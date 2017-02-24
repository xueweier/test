---
layout: post
title: github的辅助项目们
category: tech
tags: github git opensource
---

![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201702/github.jpg)

时常在github项目上看到类似下面这样的小标签,用于展示项目的一些信息。

![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201702/20170221202753.jpg)

今天稍微接触了一下这些(小)项目。目前还没有在blog上使用，过段时间会尝试一下。

# Shields.io

提供高质量Github项目进度角标图片的API.[shields.io][shields.io] 。引用起来也非常简单

    <div id="container">
        <img src="http://progressed.io/bar/28?title=progress" alt="">
        <img src="http://progressed.io/bar/30?title=nice" alt="">
        <img src="http://progressed.io/bar/60?title=极客标签" alt="">
        <img src="https://img.shields.io/teamcity/http/teamcity.jetbrains.com/s/bt345.svg" alt="">
        <img src="https://img.shields.io/pypi/dw/Django.svg" alt="">
        <img src="https://img.shields.io/badge/soul-GBTAG-red.svg" alt="">
    </div>
    <script type="text/javascript" src="http://cdn.gbtags.com/jquery/1.11.1/jquery.min.js"></script>



显示出来就是这个样子：

<div id="container">
    <img src="http://progressed.io/bar/28?title=progress" alt="">
    <img src="http://progressed.io/bar/30?title=nice" alt="">
    <img src="http://progressed.io/bar/60?title=极客标签" alt="">
    <img src="https://img.shields.io/teamcity/http/teamcity.jetbrains.com/s/bt345.svg" alt="">
    <img src="https://img.shields.io/pypi/dw/Django.svg" alt="">
    <img src="https://img.shields.io/badge/soul-GBTAG-red.svg" alt="">
</div>
<script type="text/javascript" src="http://cdn.gbtags.com/jquery/1.11.1/jquery.min.js"></script>

总的说来就是显示项目构建、下载、版本号、sns情况和其他五花八门的信息显示图标。

# gitter.im

[Gitter.im][gitter.im]是一款支持Markdown的针对开发者的即时通讯软件。

* Gitter.im 基于 Github 进行构建，紧密地集成到您的 organisations, repositories, issues 和 activity。
* Gitter.im还提供与 Trello, Jenkins, Travis CI, Heroku, Sentry, BitBucket, HuBoard, Logentries, Pagerduty 以及 Sprintly 的集成。同样支持自定义的 webhook ，为集成提供了开源库以及灵活的API。
* 支持MarkDown语法
* 免费用户就可拥有 无限制的公开及私密聊天室数量
* 免费用户就可拥有 无限制的历史聊天记录
* 拥有Macos，Linux，Windows，苹果IOS，安卓Andriod APP客户端，还有几十个第三方APP。

# Go Report Card

[Go_Report_Card][Go_Report_Card],一个可以为你的开源 Go 代码生成质量报告的网站，挺不错的，利用的常见的几个工具，golint, go vet等。

# microbadger

[microbadger][microbadger]是一个管理你的Docker镜像的工具。

# microbadger

[microbadger][microbadger]是一个管理你的Docker镜像的工具。

# travis-ci.org

[Travis CI][travis-ci] 是目前新兴的开源持续集成构建项目，它与jenkins，GO的很明显的特别在于采用yaml格式，简洁清新独树一帜。目前大多数的github项目都已经移入到Travis CI的构建队列中。Travis-CI 使用 PostgreSQL 数据库。


# 参考资料

* [kcptun][kcptun]
* [thefuck][thefuck]
* [shields.io][shields.io]
* [gitter.im][gitter.im]
* [Go_Report_Card][Go_Report_Card]
* [microbadger][microbadger]
* [travis-ci][travis-ci]

[shields.io]: http://shields.io
[kcptun]: https://github.com/xtaci/kcptun/blob/master/README-CN.md
[thefuck]: https://github.com/nvbn/thefuck
[gitter.im]: https://gitter.im
[Go_Report_Card]: https://goreportcard.com
[microbadger]: https://microbadger.com
[travis-ci]: https://travis-ci.org/getting_started
