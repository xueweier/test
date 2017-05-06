---
layout: post
title:  Hubot 脚本与开发文档 中文
category: tech
tags: linux
---

![](/assets/img/hubot.jpg)

每次看英文文档都有点头疼，做了一些简要的翻译给自己看。 
原文请看<https://hubot.github.com/docs/scripting/>

安装好hubot后，根目录下会生成一个 scripts 目录，里面有个可用的 demo 文件`example.coffee`，大致如下：

    # Description:
    #   Example scripts for you to examine and try out.
    #
    # Notes:
    #   They are commented out by default, because most of them are pretty silly and
    #   wouldn't be useful and amusing enough for day to day huboting.
    #   Uncomment the ones you want to try and experiment with.
    #
    #   These are from the scripting documentation: https://github.com/github/hubot/blob/master/docs/scripting.md

    module.exports = (robot) ->

     robot.hear /kelutest/i, (res) ->
       res.send "Badgers? BADGERS? WE DON'T NEED NO STINKIN BADGERS"
      #
      # robot.respond /open the (.*) doors/i, (res) ->
      #   doorType = res.match[1]
      #   if doorType is "pod bay"
      #     res.reply "I'm afraid I can't let you do that."
      #   else
      #     res.reply "Opening #{doorType} doors"

要使得你编写的script生效，需要满足一下三个条件：

* 在scripts文件夹中
* .coffee 或 .js 文件
* 导出一个方法

        module.exports = (robot) ->
          # your code here

# 接收和回复

    module.exports = (robot) ->
      robot.hear /badger/i, (res) ->
        res.send "Badgers? BADGERS? WE DON'T NEED NO STINKIN BADGERS"
    
      robot.respond /open the pod bay doors/i, (res) ->
        res.reply "I'm afraid I can't let you do that."
    
      robot.hear /I like pie/i, (res) ->
        res.emote "makes a freshly baked pie"

* hear 所有匹配信息
* send 发送消息
* respond 群组消息中只处理@自己的信息
* reply 群组消息中回复特定人的消息

# 给指定群组或用户的消息

可以使用messageRoom函数发送到指定的房间或用户，可以明确地指定用户名（对于管理员/管理员），或者使用响应对象将私人消息发送到原始发件人。

      robot.respond /I don't like Sam-I-am/i, (res) ->
        room =  'joemanager'
        robot.messageRoom room, "Someone does not like Dr. Seus"
        res.reply  "That Sam-I-am\nThat Sam-I-am\nI do not like\nthat Sam-I-am"

      robot.hear /Sam-I-am/i, (res) ->
        room =  res.envelope.user.name
        robot.messageRoom room, "That Sam-I-am\nThat Sam-I-am\nI do not like\nthat Sam-I-am"

# 捕获数据

res.match 存有 match 传入消息与正则表达式的结果。这是一个数组，索引起始是0。比如：

      robot.respond /open the (.*) doors/i, (res) ->
        doorType = res.match[1]
        if doorType is "pod bay"
          res.reply "I'm afraid I can't let you do that."
        else
          res.reply "Opening #{doorType} doors"
          
# 进行HTTP调用

Hubot可以集成使用第三方API。 通过在robot.http的node-scoped-http-client的robot.http 。 最简单的情况如下：

       robot.http("https://midnight-train")
         .get() (err, res, body) ->
           # your code here
           
未完待续           