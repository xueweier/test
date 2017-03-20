---
layout: post
title: Jekyll 环境配置
category: tech
tags: jekyll dev github mac windows
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

# 问题定位

### jekyll-paginate

Q：It looks like you don't have jekyll-paginate or one of its dependencies installed
    
A：在 Gemfile 文件中添加如下内容：
    
    gem 'jekyll-paginate', group: :jekyll_plugins

### Gemfile

Q：在 Mac 下运行得好好的，Windows 下不能运行，报错。

    C:/Ruby23-x64/lib/ruby/2.3.0/rubygems/core_ext/kernel_require.rb:55:in `require': cannot load such file -- bundler (LoadError)
        from C:/my_pp/Ruby23-x64/lib/ruby/2.3.0/rubygems/core_ext/kernel_require.rb:55:in `require'
        from C:/my_pp/Ruby23-x64/lib/ruby/gems/2.3.0/gems/jekyll-3.2.1/lib/jekyll/plugin_manager.rb:34:in `require_from_bundler'
        from C:/my_pp/Ruby23-x64/lib/ruby/gems/2.3.0/gems/jekyll-3.2.1/exe/jekyll:9:in `<top (required)>'
        from C:/my_pp/Ruby23-x64/bin/jekyll:23:in `load'
        from C:/my_pp/Ruby23-x64/bin/jekyll:23:in `<main>'

A：Windows 下删除 Gemfile 文件即可。
    
    
# 常用命令

安装完成后，使用

    jekyll serve --incremental
    
运行项目，只改变修改的文章内容，加快运行速度。毕竟平时也就修改文章内容，不会改变系统配置。

参考资料：

* [Running Jekyll on Windows](http://www.madhur.co.in/blog/2011/09/01/runningjekyllwindows.html)
* [jekyllcn.com](http://jekyllcn.com)
* [jekyll-paginate issue](https://github.com/jekyll/jekyll/issues/4124)
* [Jekyll/Liquid API 语法文档 - Alfred Sun](http://alfred-sun.github.io/blog/2015/01/10/jekyll-liquid-syntax-documentation/)
* [Liquid 基础篇 - 波波的个人主页](https://lsbbd.github.io/2016/08/26/liquid-basic/#liquid)
* [add possibility to convert string to number](https://github.com/Shopify/liquid/issues/130)
* [给jekyll增加自定义分页](http://www.oushit.com/technology/2014/11/03/add-postpager-for-jekyll.html)