---
layout: post
title: Slack/Hubot
category: tech
tags: slack hubot github maintenance node npm
---





想拥有一个自己的hubot也有一段时间了。

虽然很早就开始用slack，都是用来配合ifttt耍的，还不知道这么一个大杀器。后来订阅了湾区日报。看了一些文章后才发现的，确实是个好东西。工作忙里偷闲，按照slack API里 [Slack Developer Kit for Hubot][slack-hubot-api] 的内容一步一步走来。目前对里面的一些产品插件都还不了解，先记录下来，以后再慢慢补充了。

# 1.安装node和npm环境

可以访问我的[gist](https://gist.github.com/kelvinblood/fef5a31e69b099c3a0225a12481923d7)下载。
<script src="https://gist.github.com/kelvinblood/fef5a31e69b099c3a0225a12481923d7.js"></script>

	chmod a+x NodejsInstall.sh

然后就要经过漫长的等待（我的机器性能一般，大概是1个小时）。
安装完成后，node就安装好了。在安装node的同时，npm也安装到了你的服务器上。通过下面的命令查看node和npm的版本号信息。

	node -v
	npm -v

由于npm更新的速度会比node更快一些，所以运行下面的命令可以保证你的npm保持最新。

	npm install npm@latest -g

# 1.安装slack/Hubot kit

使用下面的命令快速安装[Yeoman][Yeoman] 和 [hubot][hubot] 。Yeoman可以辅助我们快速安装hubot。

	npm install -g yo generator-hubot

安装好环境后，在我们感兴趣的目录下，可以开始新建我们的hubot项目了。

	mkdir my-awesome-hubot && cd my-awesome-hubot
	yo hubot --adapter=slack

如果你是以root用户安装的话，记得要给相关的文件夹赋权限。
同时你还需要去hubot页面生成你的机器人[API Token][Integration_setting]。在获得api token之后，你可以运行以下命令跑起来了。

    HUBOT_SLACK_TOKEN=xoxb-YOUR-TOKEN-HERE ./bin/hubot --adapter slack
    
    
![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201701/filehelper_1484244539165_20.png)
    
待续。。


参考资料：

* [node.js](https://nodejs.org/en/download/)
* [Slack/Hubot - 简书](http://www.jianshu.com/p/e5015327f900)
* [湾区日报的第一个“员工”：Slack/Hubot - 湾区日报](https://wanqu.co/b/8/2015-08-19-slack-hubot.html)
* [slackapi/hubot-slack - github](https://github.com/slackapi/hubot-slack)
* [hubot.github.com - hubot][hubot]
* [github/hubot-scripts - hubot](https://github.com/github/hubot-scripts/tree/master/src/scripts)
* [Slack Developer Kit for Hubot - SlackAPI][slack-hubot-api]
* [Start automating your business tasks with Slack - howdy.ai](https://blog.howdy.ai/what-will-the-automated-workplace-look-like-495f9d1e87da#.d7jn5l8x9)
* [The New York Times built a Slack bot to help decide which stories to post to social media - niemanlab](http://www.niemanlab.org/2015/08/the-new-york-times-built-a-slack-bot-to-help-decide-which-stories-to-post-to-social-media/)
* [How 7 news organizations are using Slack to work better and differently - niemanlab](http://www.niemanlab.org/2015/07/how-7-news-organizations-are-using-slack-to-work-better-and-differently/)
* [The Most Important Startup’s Hardest Worker Isn’t a Person - wired.com](https://www.wired.com/2015/10/the-most-important-startups-hardest-worker-isnt-a-person/)
* [Slack Is Overrun With Bots. Friendly, Wonderful Bots - wired.com](https://www.wired.com/2015/08/slack-overrun-bots-friendly-wonderful-bots/)
* [zhihubot 搭建 - Hello, SA](http://blog.hellosa.org/2012/02/22/zhihubot.html)


[Yeoman]: http://yeoman.io/
[hubot]: https://hubot.github.com/
[Integration_setting]: https://my.slack.com/apps/A0F7YS25R-bots
[slack-hubot-api]: https://slackapi.github.io/hubot-slack/
