---
layout: post
title: 磁盘空间100%，无法写入数据
category: tech
tags: linux shell du df
---

Q:
Exceptions started to appear in all views, and when I try to run composer update, it always ends up with

{"error":{"type":"ErrorException","message":"array_merge(): Argument #2 is not an array","file":"\/laravel\/framework\/src\/Illuminate\/Foundation\/ProviderRepository.php","line":188}}


A:
After a lot of searching and exploring each file in the 'app' folder, it appears that one file was corrupt

Delete app/storage/meta/services.json and re-run composer update and this should solve it.


参考资料：

* [诡异的磁盘空间100%报警分析得出df -h与du -sh的根本性差别 - 推酷](hhttp://www.tuicool.com/articles/2Iv67r)