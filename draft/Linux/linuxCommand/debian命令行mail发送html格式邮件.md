# debian命令行mail发送html格式邮件
debian命令行mail发送html格式邮件

exim4安装配置

1. 安装

通常debian7会默认安装exim4，除非是精简版本。

如果没有，可以通过执行apt-get install exim4进行安装

2. 配置

dpkg-reconfigure exim4-config
运行上面的命令重新对exim进行配置，注意第一个选项选择：

internet site; mail is sent and received directly using SMTP
后面的直接按默认的一直下一步就可以完成配置了

exim4的配置文件位于/etc/exim4/目录下，update-exim4.conf.conf是其主配置文件

配置内容如下：

dc_eximconfig_configtype=’internet’
dc_other_hostnames=’‘
dc_local_interfaces=‘127.0.0.1’
dc_readhost=’‘
dc_relay_domains=’‘
dc_minimaldns=’false’
dc_relay_nets=’‘
dc_smarthost=’‘
CFILEMODE=‘644’
dc_use_split_config=’false’
dc_hide_mailname=’‘
dc_mailname_in_oh=’true’
dc_localdelivery=’mail_spool’
其中，dc_other_hostnames可以修改为主机名，dc_readhost可以改为邮箱后缀

如果都留空，发送时候发件人会显示成 user@127.0.0.1

配置完后，重启生效

/etc/init.d/exim4 restart
3. 其他设置

用户可以指定发件人的邮件地址，通过修改/etc/email-addresses

test:myemail@tmp.com
将用户test的邮箱绑定为myemail@tmp.com

至于发件人显示名称，如果在/etc/passwd中有设置别名，则以该别名作为发件人名称
比如，当test用户为：

test:x:2172:2172:myname:/home/test:/bin/bash
则发件人显示为：myname<myemail@tmp.com>

发送html邮件

1. mail命令
简单的发送邮件命令为

# 短消息
echo “mail content” | mail -s “mail title” test@mail.com
# 信息文本
mail -s “mail title” test@mail.com <message.txt
利用mail命令发送html格式的邮件：

msg=”<b><div style=’color:red’>HTML Message goes here</div></b>”
title=`echo -e “title\nContent-Type: text/html;charset=utf-8”`
echo $msg | mail -s “$title” test@mail.com
2. sendmail命令

mail_header(){
    echo “To:test@mail.com”
    echo “Subject:title”
    echo “MIME-Version:1.0”
    echo “Content-type:text/html;charset=utf-8”
    echo “$1”
}
msg=”<b><div style=’color:red’>HTML Message goes here</div></b>”
mail_header $msg | /usr/sbin/sendmail -t
生成相应的邮件格式，然后利用sendmail -t发送

需要注意，sendmail位于/usr/sbin，

默认情况下，普通用户直接用sendmail命令执行时会提示找不到命令

必须以/usr/sbin/sendmail来调用命令，而root用户则不用

3. 其他

PHP可以利用mail方法来发送html邮件，指定相应的header参数

<?php

$to = “somebody@example.com, somebodyelse@example.com”;  
$subject = “HTML email”;

$message = ”  
<html>  
<head>  
<title>HTML email</title>  
</head>  
<body>  
<p>This email contains HTML Tags!</p>  
<table>  
<tr>  
<th>Firstname</th>  
<th>Lastname</th>  
</tr>  
<tr>  
<td>John</td>  
<td>Doe</td>  
</tr>  
</table>  
</body>  
</html>  
“;

// 当发送 HTML 电子邮件时，请始终设置 content-type  
$headers = “MIME-Version: 1.0” . “\r\n”;  
$headers .= “Content-type:text/html;charset=utf8” . “\r\n”;

// 更多报头  
$headers .= ‘From: <webmaster@example.com>’ . “\r\n”;  
$headers .= ‘Cc: myboss@example.com’ . “\r\n”;

mail($to,$subject,$message,$headers);  
?>
此外，python的libsmtp库，可以连接到指定的smtp服务器，登陆验证后，发送邮件，具体代码就不贴了。