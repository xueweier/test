---
layout: post
title: 在Linux上使用dropbox
category: linux
---

需要在Linux服务器和本地下载东西时，ftp什么的也可行，但我常常在服务器上下载pt资源，每次都下载到本地也是麻烦。索性直接扔到网盘让本地自动同步还更方便。

说到网盘，脑子里闪现出来的只有谷歌、微软和dropbox了。不过能同时支持Linux和Mac的，就只剩下dropbox了。虽然dropbox空间比较小，不过以前有做过活动，我的dropbox有8G的空间，所以还是蛮足够了的。

安装的方法只看dropbox的官网上就有说明了。

![image](http://7vigrt.com1.z0.glb.clouddn.com/blog_屏幕快照%202015-10-11%20下午1.26.40.png)


通过命令行安装无外设模式的 Dropbox
Dropbox 守护程序可在所有 32 位与 64 位 Linux 服务器上正常运行。若要安装，请在 Linux 终端运行下列命令。

	32-bit:
	
	cd ~ && wget -O - "https://www.dropbox.com/download?plat=lnx.x86" | tar xzf -
	64-bit:
	
	cd ~ && wget -O - "https://www.dropbox.com/download?plat=lnx.x86_64" | tar xzf -
接着，从新建的 .dropbox-dist 文件夹运行 Dropbox 守护程序。

	~/.dropbox-dist/dropboxd
	
如果是首次在服务器上运行 Dropbox，系统会要求您将类似于下面的链接复制并粘贴到运行的浏览器中，以便创建一个新的帐户或将服务器附加到现有帐户上。

	https://www.dropbox.com/cli_link?host_id=XXXXXXXXXXXXXXXXXXX


操作完成后，系统会在您的主目录中创建 Dropbox 文件夹。下载这个 [Python](http://d.pr/1fPnG) 脚本，通过命令行控制 Dropbox。为了方便访问，我把这个脚本放入Dropbox目录下，使用下面的命令将快捷方式添加到系统中：

	echo "alias dr='python ~/Dropbox/dropbox.py'" >> ~/.bashrc

之后就可以很方便的使用`dr`这个命令对dropbox进行操作了，直接输入`dr`就可以看到相关的命令了。

![image](http://7vigrt.com1.z0.glb.clouddn.com/blog_WeChat_1444541977.jpeg)

* [Dropbox 官网说明](https://www.dropbox.com/zh_CN/install?os=lnx)

