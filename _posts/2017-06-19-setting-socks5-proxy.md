---
layout: post
title:  设置 socks5/http 代理，可用于git和shell终端
category: tech
tags: proxy
---

# 普通系统代理

普通的系统代理，一般代理软件都会设置好了。如果设置不成功，可以按照如下方式手动设置。

Windows 如下：

![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201707/QQ20170703-222642.png)

![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201707/QQ20170703-222614.png)

![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201707/QQ20170703-222749.png)

Mac 如下:

![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201707/屏幕截图 2017-07-03 22.28.09.png)

![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201707/QQ20170703-222859.png)

![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201707/QQ20170703-222930.png)


# git

* 设置HTTP协议

    设置
    
     如果你的代理是 socks5
     
        git config --global http.proxy 'socks5://127.0.0.1:1080' 
        git config --global https.proxy 'socks5://127.0.0.1:1080'
     
     如果是 http
     
        git config --global http.proxy "http://127.0.0.1:6667"
        git config --global https.proxy "http://127.0.0.1:6667"
        
     取消设置  
        
        git config --global --unset http.proxy
        git config --global --unset https.proxy
        
* 设置SSH协议

    新建/编辑 `~/.ssh/config` 文件
    
        # 如果用默认端口，这里是 github.com，如果想用443端口，这里就是 ssh.github.com 
        # 详见 https://help.github.com/articles/using-ssh-over-the-https-port/
        Host github.com
        HostName github.com
        User git

        # 如果是 HTTP 代理，使用下面这行，并把 proxyport 改成自己的 http 代理的端口
        ProxyCommand socat - PROXY:127.0.0.1:%h:%p,proxyport=6667

        # 如果是 socks5 代理，则把下面这行取消注释，并把 6666 改成自己 socks5 代理的端口
        ProxyCommand nc -v -x 127.0.0.1:1080 %h %p

# 终端 terminal

在 .bashrc 或 .zshrc 中设置如下内容

    alias setproxy="export ALL_PROXY=socks5://127.0.0.1:1080"
    alias unsetproxy="unset ALL_PROXY"
    aliaa ip="curl -i http://ip.cn"

在使用是手动调用这些命令进行设置。

或者使终端总是使用代理：

    export http_proxy="socks5://127.0.0.1:1086"
    export https_proxy="socks5://127.0.0.1:1086"

重启 terminal 生效。可以通过curl -i http://ip.cn查看IP改变来测试是否生效

![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201707/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-07-04%20%E4%B8%8B%E5%8D%888.01.53.png)

# 参考资料

* [设置 Git 代理](https://imciel.com/2016/06/28/git-proxy/)
* [macOS 终端走代理（科学上网）](https://juejin.im/entry/5821840cd203090055134cc0)
* [设置socks5代理](http://www.jianshu.com/p/ff4093ed893f)