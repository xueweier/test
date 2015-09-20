---
layout: post
title: Linux的locale设置问题
category: linux
---

今天新开了一个[digital ocean][ocean]的vps，发现了这个提醒：

	
	WARNING! Your environment specifies an invalid locale.
	 This can affect your user experience significantly, including the
	 ability to manage packages. You may install the locales by running:
	
	   sudo apt-get install language-pack-zh
	     or
	   sudo locale-gen zh_CN.UTF-8
	
	To see all available language packs, run:
	   apt-cache search "^language-pack-[a-z][a-z]$"
	To disable this message for all users, run:
	   sudo touch /var/lib/cloud/instance/locale-check.skip
	
输入`dpkg-reconfigure`显示如下提醒

	perl: warning: Setting locale failed.
	perl: warning: Please check that your locale settings:
		LANGUAGE = (unset),
		LC_ALL = (unset),
		LC_CTYPE = "zh_CN.UTF-8",
		LANG = "en_US.UTF-8"
	    are supported and installed on your system.
	perl: warning: Falling back to a fallback locale ("en_US.UTF-8").
	locale: Cannot set LC_CTYPE to default locale: No such file or directory
	locale: Cannot set LC_ALL to default locale: No such file or directory



解决办法是在文件/etc/environment中添加如下内容

	LC_ALL="en_US.utf8"
	
输入`dpkg-reconfigure locale`按照提示选中一下内容

![image](http://7vigrt.com1.z0.glb.clouddn.com/blog_屏幕快照%202015-09-20%20下午5.29.45.png)

![image](http://7vigrt.com1.z0.glb.clouddn.com/blog_屏幕快照%202015-09-20%20下午5.30.05.png)

完成以上步骤后重启系统即可。


--------------
digital ocean是一个新兴的vps运营商，如果你也打算使用，可以使用我的推荐链接注册，这样子你我都将得到10美元。[digital ocean][ocean]


[ocean]:https://www.digitalocean.com/?refcode=f595b7f62cc7
