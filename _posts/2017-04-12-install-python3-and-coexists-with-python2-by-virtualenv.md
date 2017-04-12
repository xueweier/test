---
layout: post
title: python3 安装与使用virtualenv建立虚拟环境
category: tech
tags: python
---

![](/assets/img/python.jpg)

就目前来说，所有 Linux 发行版自带的python都是python2，由于python3与2不兼容，所以python3的推进一直不顺利。然而我也不想直接踢掉系统的python2环境，哪天需要的时候不还得搞一遍回来么？好在python官方提供了virtualenv环境，可以无缝使用不同版本间的Python。

最近开始部署 python 应用了。于是记录一下配置 python3 环境的步骤。

# 编译安装Python3.6

直接选择了最新的python版本： [官网下载](https://www.python.org/downloads/source/)

    cd /tmp
    wget https://www.python.org/ftp/python/3.6.1/Python-3.6.1.tgz
    tar -xzvf Python-3.6.1.tgz
    cd Python-3.6.1
    mkdir /usr/local/python3.6
    ./configure --prefix=/usr/local/python3.6
    make
    make install
    
然后 python3 就安装好了。这次安装会将pip也一并安装了。

# 安装 Python2 的 pip

    cd /tmp
    wget https://bootstrap.pypa.io/get-pip.py
    python get-pip.py
 
# 安装virtualenv

    pip install virtualenv
    
# 使用virtualenv   

### 创建虚拟环境

    cd /var/local
    virtualenv -p /usr/local/python3.6/bin/python3.6 xx
    
指定使用python3.6创建一个项目虚拟环境.

### 激活虚拟环境

    cd xx                    # 切换至项目目录
    source ./bin/activate

### 退出虚拟环境

    deactivate      # 退出项目的 virtualenv 虚拟环境.
    
    
# 参考资料
    
* [在Debian上编译安装Python3.4及使用virtualenv建立虚拟环境](https://xiaoguo.net/post/Install-Python3-and-virtualenv-in-debian.html)    