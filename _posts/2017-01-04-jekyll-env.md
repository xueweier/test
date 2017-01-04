---
layout: post
title: jekyll环境配置
category: tech
tags: jekyll tech github mac windows
---

昨天配好了Mac下的开发环境。记录下一些问题。

# Windows 下配置Jekyll
### 安装 Ruby 和 Ruby DevKit

<http://rubyinstaller.org/downloads/>
安装好 Ruby，把 DevKit 解压到某个目录，运行：

    ruby dk.rb init
    ruby dk.rb install
    
如果Ruby环境变量没有添加，后把安装目录下的 Bin 目录添加到系统Path环境变量即可。

### 安装 Jekyll

    gem install jekyll
    
### 安装 Python
<https://www.python.org/>

使用2.7的版本，安装完成后把 Python 安装目录和 Scripts 目录添加到系统Path环境变量中。

### 安装 Pygments
<https://pypi.python.org/pypi/setuptools#windows>

    python distribute_setup.py
    easy_install Pygments



# Mac 下配置Jekyll

Mac 下配置 Jekyll 相对简单得多，按照官网的方式即可[jekyllcn.com](http://jekyllcn.com)

    gem install jekyll bundler
    jekyll new my-awesome-site
    cd my-awesome-site
    bundle install
    bundle exec jekyll serve
    #打开浏览器 http://localhost:4000

### 问题定位

1. jekyll-paginate


    Q：It looks like you don't have jekyll-paginate or one of its dependencies installed
A：
    在 Gemfile 文件中添加如下内容：
    
    gem 'jekyll-paginate', group: :jekyll_plugins

    

参考资料：

* [Running Jekyll on Windows](http://www.madhur.co.in/blog/2011/09/01/runningjekyllwindows.html)
* [jekyllcn.com](http://jekyllcn.com)
* [jekyll-paginate issue](https://github.com/jekyll/jekyll/issues/4124)
