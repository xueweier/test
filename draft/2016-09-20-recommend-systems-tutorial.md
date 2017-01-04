---
layout: post
title: 推荐系统
category: life
tags: front-end js vuejs
---

业界是有几款开源的推荐引擎框架，简单看了一下，个人把他分为两类，“简单的”与“大型的”。“简单的”有RecDB、EasyRec等等，说它“简单”，是因为部署和使用确实够简单，RecDB是基于PostgreSQL开发的，用SQL的语法来做推荐。EasyRec是用Java+MySQL开发的，下载个war包就能部署，但MySQL的初始化好像有点复杂。这两个框架在数据量小的时候确实非常方便，但数据量慢慢增加的时候怎么办呢？我并不知道他们支持不支持数据库的分片，就算持支，这两个数据库的分片都不是简单的事


参考资料：

* [博客推荐系统: 基于内容相似性的推荐 ( 第二部分) - InfoQ](hhttp://www.infoq.com/cn/articles/blog-recommendation-system-part02)

* [探索推荐引擎内部的秘密，第 1 部分: 推荐引擎初探 - IBM](https://www.ibm.com/developerworks/cn/web/1103_zhaoct_recommstudy1/)
* [探索推荐引擎内部的秘密，第 2 部分: 深入推荐引擎相关算法 - 协同过滤 - IBM](http://www.ibm.com/developerworks/cn/web/1103_zhaoct_recommstudy2/index.html)
* [探索推荐引擎内部的秘密，第 3 部分: 深入推荐引擎相关算法 - 聚类 - IBM](http://www.ibm.com/developerworks/cn/web/1103_zhaoct_recommstudy3/index.html)
* [RecSys – ACM Recommender Systems](https://recsys.acm.org/)
* [真正统治世界的十大算法](http://blog.jobbole.com/70639/)
* [傅里叶分析之掐死教程（完整版） - 知乎专栏](https://zhuanlan.zhihu.com/p/19763358)
* [BreezeDeus](http://breezedeus.github.io/)
* [基于内容的推荐（Content-based Recommendations） - BreezeDeus](http://www.cnblogs.com/breezedeus/archive/2012/04/10/2440488.html)
* [王安琪 - 博客园](http://www.cnblogs.com/wgp13x/)
* [实时推荐系统的3种方式 - OPEN 开发经验库](http://www.open-open.com/lib/view/open1434610424895.html)
* [推荐系统中所使用的混合技术介绍  - OPEN 开发经验库](http://www.open-open.com/lib/view/open1406710265718.html)
* [推荐系统 - OPEN开发经验库](http://www.open-open.com/lib/tag/推荐系统)
* [推荐引擎 - OPEN开发经验库](http://www.open-open.com/lib/list/391?o=v)
* [推荐引擎 - 开源软件库 - 开源中国社区](http://www.oschina.net/project/tag/424/recommended-engine)
* [JustinWu个人主页 - 伯乐在线](http://www.jobbole.com/members/mybreeze77/)