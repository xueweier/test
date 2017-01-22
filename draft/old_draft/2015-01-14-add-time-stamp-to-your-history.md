---
layout: post
title: linux输入历史的history加上时间戳
category: linux
---

在`.bashrc`中添加语句`HISTTIMEFORMAT="%Y-%m-%d %H:%M:%S "`，`history`就可以保存时间了。

但是.bash_history里并没有加上这个时间戳。其实这个时间记录是保存在当前shell进程内存里的，如果你logout并且重新登录的话会发现你上次登录时执行的那些命令的时间戳都为同一个值，即当时logout时的时间。 

故而在/etc/profile内加入这句话就可以了。<code>export HISTTIMEFORMAT="%F %T `whoami` "</code>。