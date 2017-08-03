---
layout: post
title: Python Selenium 获取 html 与模糊匹配属性的方法
category: tech
tags:  python selenium
---
![](/assets/img/python.jpg)

# 获取 html


	element.get_attribute('innerHTML')
	element.get_attribute('outerHTML')


# 模糊匹配属性

使用 XPath：

```
//a[contains(@prop,'Foo')]
```

# 参考资料
* [Get HTML Source of WebElement in Selenium WebDriver using Python](https://stackoverflow.com/questions/7263824/get-html-source-of-webelement-in-selenium-webdriver-using-python)
* [What is the correct XPath for choosing attributes that contain “foo”?](https://stackoverflow.com/questions/103325/what-is-the-correct-xpath-for-choosing-attributes-that-contain-foo)
