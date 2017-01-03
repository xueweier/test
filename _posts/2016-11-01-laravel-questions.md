---
layout: post
title: 在laravel遇到的疑难杂症
category: tech
tags: php laravel
---

Q:
Exceptions started to appear in all views, and when I try to run composer update, it always ends up with

{"error":{"type":"ErrorException","message":"array_merge(): Argument #2 is not an array","file":"\/laravel\/framework\/src\/Illuminate\/Foundation\/ProviderRepository.php","line":188}}


A:
After a lot of searching and exploring each file in the 'app' folder, it appears that one file was corrupt

Delete app/storage/meta/services.json and re-run composer update and this should solve it.


参考资料：

* [Laravel: array_merge(): Argument #2 is not an array error - Stack Overflow](http://stackoverflow.com/questions/25575217/laravel-array-merge-argument-2-is-not-an-array-error)