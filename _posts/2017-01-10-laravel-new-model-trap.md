---
layout: post
title: Laravel 新建 Model 的小坑
category: tech
tags: php laravel trap
---

Laravel 是 Taylor Otwell 开发的一款基于 PHP 语言的 Web 开源框架。由于 Laravel 具备 Rails 敏捷开发等优秀特质，并结合了 PHP 强大的扩展包（Composer）生态与 PHP 开发者广大的受众群，让 Laravel 在发布之后的短短几年时间得到了极其迅猛的发展。

通过 [Google Trends][google_trends] 提供的趋势图可以看出，Laravel 框架在过去十年，其增长速度在各类 PHP 框架中都是有史以来最快的，这也从正面直接反映出了 Laravel 的强大，以及其未来非常可观的发展前景。

![图1](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201701/QQ%E5%9B%BE%E7%89%8720170111175437.png)

使用Laravel也有1年多的时间了。偶尔会出现点小问题，也总算是解决了。今后会慢慢总结一些。现在先说说昨天晚上遇到的坑。





# 新建 model 的小坑

![图2](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201701/QQ%E5%9B%BE%E7%89%8720170111175246.png)

Laravel 的 Eloquent ORM 提供了漂亮、简洁的 ActiveRecord 实现来和数据库进行交互。每个数据库表都有一个对应的「模型」可用来跟数据表进行交互。

然而当 new 一个新对象，并save到数据库时（第1第2步），如果不重新去数据库中 find 出来（第3步），则 Eloquent 中很多默认赋值的属性将不存在！比如说 created_at 等默认属性。


[google_trends]: https://www.google.com/trends/explore?date=2006-08-16%202017-01-11&q=yii,CodeIgniter,Cakephp,Laravel,%2Fm%2F09cjcl
