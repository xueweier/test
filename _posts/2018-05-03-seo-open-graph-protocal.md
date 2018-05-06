---
layout: post
title: seo优化之open-graph-protocal
category: product
tags: seo
---
![](https://cdn.kelu.org/blog/tags/seo.jpg)

# 什么是og

og是一种新的HTTP头部标记，由 Facebook在 2010年F8会议上公布的一套**开放内容协议(Open Graph Protocol)**，任何网页只要遵守该协议，SNS就能从页面上提取最有效的信息并呈现给用户。

可以查看官网进行更深入的了解——<http://ogp.me/

按照百度百科的信息:

> Open Graph通讯协定(Protocol)本身是一种制定一套Metatags的规格，用来标注你的页面，告诉我们你的网页代表哪一类型的现实世界物件。另一伙伴网站，即Amazon旗下的Internet Movie Database(IMDb)，将用这个Open Graph Protocol为每一部电影标注页面。按下IMDb上的“赞”按钮，就会自动把那部电影加入Facebook使用者profile中的“最爱的电影”。

> Facebook已和Yahoo、Twitter合作采用OAuth 2.0认证标准。Graph API翻新了Facebook的平台程序代码，让Facebook里的每个物件都拥有独特的ID。通过Open Graph把其他社交网站建构的网络给连接起来，将创造一个更聪明、更与社交连接、更个人化也更具语意意识的网络。



# OG 的意义

当用户将网页分享到facebook、twitter或者微博的时候，sns网站中的内容是按照我们og属性规定内容呈现的，以此保证信息分享更准确更符合作者所想。

搜索引擎机器人爬取的是我们的页面，即html代码，meta信息尤为关注，所以我们增加的og meta标签是可以别搜索引擎发现并评估权重的，将原有meta信息优化手段同时使用到og:title这种属性值当中，加强meta信息优化内容， 对于权重提升和排名还是很有利的。

# OG的应用

## 基本使用

要将网页转换为图形对象，您需要将基本元数据添加到页面中。每个页面的四个必需属性是：

- `og:title` - 对象标题，例如“The Rock”。
- `og:type`- 对象类型
  - video 视频
  - audio 音频
  - link 链接
  - photo 图片
  - product 产品
- `og:image` - 图片网址。
- `og:url` - 网页的永久URL，例如“http://www.imdb.com/title/tt0117500/”。

以下是[IMDB上Rock](http://www.imdb.com/title/tt0117500/)的Open Graph协议标记：

```
<html prefix="og: http://ogp.me/ns#">
<head>
<title>The Rock (1996)</title>
<meta property="og:title" content="The Rock" />
<meta property="og:type" content="video.movie" />
<meta property="og:url" content="http://www.imdb.com/title/tt0117500/" />
<meta property="og:image" content="http://ia.media-imdb.com/images/rock.jpg" />
...
</head>
...
</html>
```

## 视频video 

og:videosrc 视频资源链接，例如可是播放视频的flash地址

og:width 视频的宽度 可为空 允许多个

og:height 视频的高度 可为空 允许多个

## 音频audio 

og:audiosrc  不可为空 允许多个 音乐资源链接，例如可是播放歌曲的flash地址

og:artist 音乐家 可为空 需唯一

### 普通网页link 

og:abstract 内容摘要 可为空 允许多个

og:contentid 可为空 唯一 内容主体的ID，用来标识当前页面主要内容所处的HTML标签的ID 

### 图片photo 

og:photo 图片列表 可为空 允许多个

og:width 图片宽度 可为空 允许多个

og:height 图片高度 可为空 允许多个

### 商品product 

og:price 产品价格 不可为空 需唯一

og:description 产品描述 不可为空 需唯一

og:nick 店铺名 不可为空 需唯一

og:postfee 运费 不可为空 需唯一

## 其他可选

以下属性对于任何对象都是可选的，通常建议：

- `og:determiner` - 标题之前出现的单词。（a，an，“”，auto）等，默认是“”（空白）。
- `og:locale`- 这些标签所在的区域设置。格式`language_TERRITORY`。默认是`en_US`。
- `og:locale:alternate`- 此页面可用的其他语言环境数组。
- `og:site_name` - 如果网页是大网站的一部分，则应显示整个网站的名称。例如“IMDb”。

例如：

```
<meta property="og:audio" content="http://example.com/bond/theme.mp3" />
<meta property="og:description" 
  content="Sean Connery found fame and fortune as the
           suave, sophisticated British agent, James Bond." />
<meta property="og:determiner" content="the" />
<meta property="og:locale" content="en_GB" />
<meta property="og:locale:alternate" content="fr_FR" />
<meta property="og:locale:alternate" content="es_ES" />
<meta property="og:site_name" content="IMDb" />
<meta property="og:video" content="http://example.com/bond/trailer.swf" />
```



更详细的请参考下文的参考资料。



# 参考资料

* [The Open Graph protocol](http://ogp.me/) 