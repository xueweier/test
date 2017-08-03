---
layout: post
title:   Selenium 的一些错误总结
category: tech
tags:  python selenium
---
![](/assets/img/python.jpg)

一些错误总结。也不一定解决了，比较乱，就列出来看看，等都解决了再整理下。

* selenium.common.exceptions.WebDriverException: Message: chrome not reachable

	不太懂，没解决。

#  Ubuntu 无界面运行

	xvfb & pyvirtualdisplay 

	$ sudo apt-get install xvfb python-pip
	$ sudo pip install pyvirtualdisplay

	1.  `# 运行 Xvfb`
	2.  `Xvfb  :0  -screen 0  800x600x24  >>  /tmp/Xvfb.out  2>&1  &`
	3.  `export DISPLAY=:0`

# 有什么好的django开源项目值得参考？

这个问题问的太广了。Django好的开源项目很多，在Github上搜索一下就可以找到一堆。

如果不考虑单独的django app，只考虑可以直接拿来用的网站系统的话，有以下这些值得参考：

*   Django CMS ([https://github.com/divio/django-cms](https://link.zhihu.com/?target=https%3A//github.com/divio/django-cms))
*   Satchmo ([http://www.satchmoproject.com/](https://link.zhihu.com/?target=http%3A//www.satchmoproject.com/))
*   Askbot ([http://askbot.com/](https://link.zhihu.com/?target=http%3A//askbot.com/))
*   Sentry ([https://github.com/dcramer/sentry](https://link.zhihu.com/?target=https%3A//github.com/dcramer/sentry))
*   Pinax ([https://github.com/pinax/pinax](https://link.zhihu.com/?target=https%3A//github.com/pinax/pinax))

作者：Adieu
链接：https://www.zhihu.com/question/20176461/answer/14287055
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

```python3
import requests
for i, j in enumerate(imgUrlList):
    with open('E:/{0}.jpg'.format(i), 'wb') as file:
        file.write(requests.get(j).content)

```

用with进行上下文管理不需要人为的关闭文件，一旦程序退出with模块文件会自动关闭。本地地址可以随便换，这里用的是requests库如果没有安装直接命令行安装：

```python3
pip3 install requests

```

enumerate函数的功能简单解释，例如：

```python3
list_ = ['a', 'b', 'c']
for i, j in enumerate(list_):
    print([i, j])

```

输出结果为：

```python3
[0, 'a']
[1, 'b']
[2, 'c']

```

在这里 i 迭代的是list_的索引值，j 迭代的是元素值！

作者：杨航锋
链接：https://www.zhihu.com/question/50119196/answer/119438479
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。


* [Headless Browser Testing with Chrome and Firefox](http://fgimian.github.io/blog/2014/04/06/headless-browser-testing-with-chrome-and-firefox/)
* [Linux 无界面使用 selenium](http://jayi.leanote.com/post/Linux-无界面使用-selenium)
* [Ubuntu下配置Selenium运行环境](http://www.itfanr.cc/2016/10/19/configuration-the-selenium-running-environment-in-ubuntu/)
* [如何在无显示器的ubuntu下跑前端测试](https://my.oschina.net/zjzhai/blog/295288)
* [python获取A股数据列表的例子](http://www.111cn.net/phper/python/90110.htm)
* [搬瓦工VPS安装ubuntu后,配置selenium-chrome环境](http://www.jianshu.com/p/6e23e48ea2fe)
* [Linux配置Selenium+Chrome+Python实现自动化测试](http://zhaoyabei.github.io/2016/08/29/Linux配置Selenium+Chrome+Python实现自动化测试/)
* [如何在无显示器的ubuntu下跑前端测试](https://my.oschina.net/zjzhai/blog/295288)
* [一个 Pythoner的 Awesome List](http://www.jianshu.com/p/3e79f8565ff7)
* [Django 视图与网址进阶](http://code.ziqiangxuetang.com/django/django-views-urls2.html)
* [Bootcamp an open source Django site](http://javayhu.me/blog/2014/09/16/bootcamp-an-open-source-django-site/)
* [GitHub 上有哪些值得关注的 Django 项目？](https://www.zhihu.com/question/24342193)
* [ Python selenium —— selenium与自动化测试成神之路](http://blog.csdn.net/huilan_same/article/details/52559711)
* [Python selenium —— 一定要会用selenium的等待，三种等待方式解读](http://blog.csdn.net/huilan_same/article/details/52544521)
