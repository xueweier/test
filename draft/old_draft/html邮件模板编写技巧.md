# HTML邮件模板编写技巧




发送html邮件的建议：用style写内联的CSS；少用图片；用table实现左右布局或者更复杂的布局；用background元素设置背景图片等。
几乎每个会员制网站都需要通过后台发送邮件来与会员进行沟通，如注册确认、营销推广。这些由站方发给会员的信件，往往纯文本格式已不能满足界面和交互的要求，这时候我们就需要发送HTML页面。由于HTML邮件不是独立的HOST在本站的页面，是寄人篱下的。所以编写HTML邮件与编写HTML页面有很大的不同。因为，各面向网民的主流邮箱都或多或少的会对它们接收到的HTML邮件在后台进行过滤。毫无疑问，JS代码是被严格过滤掉的，包括所有的事件监听属性，如onclick、onmouseover，这是基于邮件安全性的考虑。不仅如此，CSS代码也会被部分过滤。本人要讲的就是如何编写不被各大主流邮箱过滤的，能正常显示的HTML邮件。
发送html邮件的建议：用style写内联的CSS；少用图片；用table实现左右布局或者更复杂的布局；用background元素设置背景图片等。
首先，我们先来看看邮箱是如何展现HTML邮件的。我本人没有做过邮件系统，况且各大邮箱后台的过滤算法也不是那么容易可以让外人知道的。所以，我们只能通过前端展现，来推测哪些是被邮箱接受的写法，而哪些又是会被过滤掉的。通过对gmail、hotmail、163、sohu、sina几个邮箱的分析，我把邮箱分为两类： 
 第一类包括gmail、hotmail、sohu，这类邮箱，邮件内容是被布局在整个邮箱页面中的某个div中。
 第二类，包括163、sina，这类邮箱，邮件内容被布局在独立的iframe中。 
熟悉HTML的朋友都知道，iframe内容是作为独立的document，与父页面的元素和CSS是互不相干的，几乎可以作为一个独立的页面来对待。而如果如果邮件内容是在div中，那么邮件内容是作为整个邮箱页面的一个组成部分。显然，以iframe作为展现方式的邮箱，对邮件内容就会宽容许多，因为它给了你一个足够独立的表现空间。而div就不是那么客气了。试想一下，如果你在你的邮件里写上这么一句CSS，是不是整个邮箱的展现页面上字体都变成20px而因此乱了套：
<style type="text/css">   
body {font-size:20px}   
</style> 
 <style type="text/css">
body {font-size:20px}
</style> 
 
我们需要写兼容各邮箱的统一邮件模板，那么必然就要避开以上这种外联CSS写法，另外类似于float、position等成非正常内容流的style也会被过滤，假如你写了，很可能会影响到外部邮箱的表现。 
 
下面我列出一些编写原则：
 1、全局规则之一，不要写<style>标签、不要写class，所有CSS都用style属性，什么元素需要什么样式就用style写内联的CSS。 
 2、全局规则之二，少用图片，邮箱不会过滤你的img标签，但是系统往往会默认不载入陌生来信的图片，如果用了很多图片的邮件，在片没有载入的情况下，丑陋无比甚至看不清内容，没耐心的用户直接就删除了。图片上务必加上alt。 
 3、不要在style里面写float、position这些style，因为会被过滤。那么如何实现左右布局或者更复杂的布局呢？用table。
 4、style内容里面background可以设置color，但是img会被过滤，就是说不能通过CSS来设置背景图片了。但是有一个很有意思的元素属性，也叫background，里面可以定义一个图片路径，这是个不错的替代方案，虽然这样功能有限，比如无法定位背景图片了，有总比没有好。例如要给一个单元格加一个背景，必须这样写： 
<td background="http://image1.koubei.com/images/common/logo_koubei.gif"></td> 
<td background="http://image1.koubei.com/images/common/logo_koubei.gif"></td> 
5、div模式的邮箱不支持flash，iframe模式的有待验证。
 
最后提一句，sohu的邮箱很怪异，会在每个文本段后面加一个空格，导致原本正常的排版一行放不下而换行，从而使某些布局错乱。所以，如果你要兼容sohu邮箱的话，遇到一些紧凑的布局就要格外小心了，尽量减少文本段的数量，留足宽度。








http://blog.itpub.net/59792/viewspace-1052646/





 Linux自动下发送HTML格式并带附件的邮件 2011-07-18 10:09:08
分类： Linux
引：
进入BEIDOU组的第一个项目就是实现一个统计报表自动发送邮件的应用，利用Shell脚本来做，期间回顾了awk，sed等文本过滤工具，crontab计划任务，还学会了在Linux下发送HTML邮件附带MS WORD/EXCEL/PPT格式附件的方法，在春节前圆满的完成了任务也算是可以踏踏实实过年了，活虽然小但毕竟可以算作一个小Milestone )
遇到问题：
统计报表实现基本思想，按处理流程顺序
1) 利用scp下载远程线上机器的Log日志文件
2) 利用awk，sed，sort等Linux下命令过滤并且分析日志，生成基本的模板（template）文本。
3) 根据该模板（template）文本统计信息生成HTML格式的邮件正文。
4) 根据该模板（template）文本统计信息生成CVS、TXT、XLS格式的统计信息作为邮件附件。
5) 利用sendmail或者mutt命令发送邮件。
6) 利用crontab计划任务定时发送日报、周报、月报。
问题就出现在步骤5)。开始我尝试利用mutt来实现发送HTML格式正文邮件并且附带附件：
mutt -e "my_hdr content-type:text/html" -s "邮件标题" -a 附件.xls receiver@123.com < mail.html
用outlook做客户端接收邮件，发现附件丢失了，变成了正文里的乱码，如果不加-e "my_hdr content-type:text/html"参数，附件成功又不能显示HTML格式邮件，期间google了各种mutt相关问题官方FAQ都无从知晓为什么，现在看来既有可能是mutt版本没有升级到1.5的一个bug，但自己不是admin也没法装最新版本的mutt，最终选择放弃使用mutt。
解决方法：
编写以下两个函数，其中sendmail()函数配好参数，就可以直接调用了。这样就可以发送带多媒体附件的HTML格式正文的邮件了。在此感谢@lingbing同学的帮助。
#发送多媒体附件的HTML格式正文的函数 (多媒体附件指非txt或者cvs格式的文件，例如excel的xls)
#$1: mail_from
#$2: mail_to 
#$3: subject 
#$4: content mimetype, such as "text/plain"
#$5: content 
#$6: attach mimetype, such as "text/csv"
#$7: attach display name
#$8: attach file path
function SendMailMultiMediaAttach(){
    local MSG_FILE="/tmp/mail.tmp"
 
    echo "From: $1" > $MSG_FILE
    echo "To: $2" >> $MSG_FILE
    echo "Subject: $3" >> $MSG_FILE
    echo "Mime-Version: 1.0" >> $MSG_FILE
    echo 'Content-Type: multipart/mixed; boundary="GvXjxJ+pjyke8COw"' >> $MSG_FILE
    echo "Content-Disposition: inline" >> $MSG_FILE
    echo "" >> $MSG_FILE
    echo "--GvXjxJ+pjyke8COw" >> $MSG_FILE
    echo "Content-Type: $4" >> $MSG_FILE
    echo "Content-Disposition: inline" >> $MSG_FILE
    echo "" >> $MSG_FILE
    echo "$5" >> $MSG_FILE
    echo "" >> $MSG_FILE
    echo "" >> $MSG_FILE
    echo "--GvXjxJ+pjyke8COw" >> $MSG_FILE
    echo "Content-Type: $6" >> $MSG_FILE
	echo "Content-Transfer-Encoding: base64" >> $MSG_FILE
    echo "Content-Disposition: attachement; filename=$7" >> $MSG_FILE
    echo "" >> $MSG_FILE
    echo "" >> $MSG_FILE
    ${BIN_PATH}/base64 -e $8 >> $MSG_FILE
 
    cat $MSG_FILE | /usr/lib/sendmail -t
}
 
##! @TODO: 发送邮件
##! @AUTHOR: zhangxu
##! @VERSION: 1.0
##! @IN: 
##! @OUT: 
function sendMail()
{
	echo "Sending $Subject mail from $From to $To"
 
	from="from@123.com"
	to="receiver@123.com"
	subject="${Subject}"
	content_type="text/html"
	body=`cat $MAIL_HTML`
	attach_type="application/vnd.ms-excel"
	attach_name="${file_title}.xls"
	attach_path="${TEMP_DIR}/${file_title}.xls"
	SendMailMultiMediaAttach "$from" "$to" "$subject" "$content_type" "$body" "$attach_type" "$attach_name" "$attach_path"
 
	echo "Send mail done."
}
要注意以下几点：
1) 多媒体文件对应的格式可以从下面的链接参考，用于替换参数$6的mimetype。http://www.w3schools.com/media/media_mimeref.asp
2) 如何判断自己的附件是不是纯文本的呢？Windows下如果可以用notepad记事本打开，或者Linux下可以用cat显示正常的都是可以用 text/plain的MIME TYPE的，其他的一律需要用1)中提到的对应的编码格式，还要保证又base64编码再发送出去，邮件客户端或者接受者可以base64解码还原。这就是之所以Content-Transfer-Encoding: base64用base64并且要用base64 -e <文件名>编码的原因，base64命令可以Google下并下载。
附：曾经帮助启发过我的链接
http://www.linuxquestions.org/questions/linux-software-2/send-excel-attachment-using-sendmail-233688/

http://neoremind.net/2011/02/linux_sendmail_attachment_mutt/
[@more@]
搞web开发的同志可能碰到过需要在页面里嵌入发送邮件的功能，如果是普通的纯文本的邮件还好，没问题，用asp有好多组件，用cgi也有好工具，比如perl。在perl中使用unix平台下的sendmail可以实现这个目的。Perl中发送纯文本邮件的典型例子如下：
#!/usr/lib/perl
use strict;
my($r_mail) = 'recipients@aaa.net';
my($s_mail) = 'sender@bbb.com';
my($subject) = 'subject';
open(MAIL,'|/usr/lib/sendmail -t');
select(MAIL);
print<<"END_TAG";
To: $r_mail
From: $s_mail
Subject: $subject
邮件内容
END_TAG
有几点要注意，在发送邮件里To, From和接受者邮件地址变量$r_mail以及发送者邮件$s_mail之间
要留一个空格，避免不必要的报错问题（我遇到过，不知道你有没有碰到）。还有那个结束标记
END_TAG如果是文件的最后一行，最好在后面加一两个空行，我曾经碰到没后面的空行perl找不到
END_TAG的情况。还有，不要忘了subject之后的那个空行是必须的，它分开了邮件头和邮件内容。
好，进入正题，如果我们需要发送html格式的邮件呢？如果写成这样
#!/usr/lib/perl
use strict;
my($r_mail) = 'recipients@aaa.net';
my($s_mail) = 'sender@bbb.com';
my($subject) = 'subject';
open(MAIL,'|/usr/lib/sendmail -t');
select(MAIL);
print<<"END_TAG";
To: $r_mail
From: $s_mail
Subject: $subject
<html><body><a href=#>邮件内容</a></body></html>
END_TAG
试试看，在263里源代码全显示出来了，在hotmail中好点，如果邮件是个完整的html邮件，基本上
能够完整的呈现html页面。其实这里头有个MIME类型的问题。详细的MIME资料大家自己上网找吧，
否则扯得太远，我这点水不够倒的。如果这个html邮件没有连接任何图片以及此类的外部内容，那好办，
在邮件头部分加一句Content-type:text/html就可以了。如果使用了中文需要指定一下代码页，直接
在后面在添上charset="gb2312",中间用分号格开。完整代码如下：
#!/usr/lib/perl
use strict;
my($r_mail) = 'recipients@aaa.net';
my($s_mail) = 'sender@bbb.com';
my($subject) = 'subject';
open(MAIL,'|/usr/lib/sendmail -t');
select(MAIL);
print<<"END_TAG";
To: $r_mail
From: $s_mail
Subject: $subject
Content-type:text/html;charset="gb2312"
<html><body><a href=#>邮件内容</a></body></html>
END_TAG
这样一般使用的接收邮件的工具都能看到html格式的邮件了。如果问题再复杂一点，
这个html页面里有图，还有flash，那怎么办？会有办法：把这些图片放在网上，
页面的图片都写全路径链接，这样就根本不需要在邮件里真的带上这些累赘了，并且
还减小了邮件的大小，一举两得！我严重赞同。但是总有碰到不能这样干的时候，所以
继续。html页面的对这些图的链接并不能够让用户收到的邮件里有这些图和flash文件。
看到的html页面是开了天窗的页面。看看MIME类型，有个multipart/mixed的类型能够完
成我们的最终目的，让用户收到的邮件是图文并举的完整页面。首先需要按一定的编码
方法对图片或者flash等文件编码，电子邮件中最常用的是base64编码，还有
quoted-printable编码。找个工具，把图片等需要链入hmtl邮件的文件使用base64编码，
对html邮件则使用quoted-printable编码。然后，在邮件头写
Content-Type: multipart/mixed;boundary="----=_NextPart_000_0008_01C2BCB0.9CF9AE70" name="thanks.gif"
这里的multipart/mixed表示本邮件是混合类型的邮件。接下来的boundary是指定分隔
邮件内容里各不同各部分的标记是什么。这里就是----=_NextPart_000_0008_01C2BCB0.9CF9AE70了。
这个值必须要怎样我不是很清楚，我的理解是在本邮件中能够不与任何编码后的某段
内容相同就可以了。后面那个name可以不要。说起来比较罗索，还是先看代码吧。
下面就是个完整的发送hmtl邮件的例子。
#!/usr/lib/perl
use strict;
my($r_mail) = 'recipients@aaa.net';
my($s_mail) = 'sender@bbb.com';
my($subject) = 'subject';
open(MAIL,'|/usr/lib/sendmail -t');
select(MAIL);
print<<"END_TAG";
To: $r_mail
From: $s_mail
Subject: $subject
Content-Type: multipart/mixed;boundary="----=_NextPart_000_0008_01C2BCB0.9CF9AE70"
This is a multi-part message in MIME format.
------=_NextPart_000_0008_01C2BCB0.9CF9AE70
Content-Type: text/html;charset="gb2312"
Content-Transfer-Encoding: quoted-printable
<HTML><HEAD><TITLE>=D0=BB=D0=BB=C4=FA=B5=C4=B2=CE=D3=EB=A3=A1</TITLE>
<META http-equiv=3DContent-Type content=3D"text/html; charset=3Dgb2312">
<META content=3D"MSHTML 6.00.2800.1126" name=3DGENERATOR></HEAD>
<BODY text=3D#000000 bgColor=3D#ffffff leftMargin=3D0 topMargin=3D0><IMG =
height=3D400=20
src=3D"file:///C:/DEV/perl/images/popup_thanks.gif" =
width=3D400=20
border=3D0></BODY></HTML>
------=_NextPart_000_0008_01C2BCB0.9CF9AE70
Content-Type: image/gif
Content-Transfer-Encoding: base64
Content-Disposition: attachment;filename="thanks.gif"
Content-Location: file:///C:/DEV/perl/images/popup_thanks.gif
R0lGODlhkAGQAfcAAPCyTdvr0cvbCJ8tTt3nAEeRGarPkRSmULrVqrRtLtbeB7CRNdTZ0LGtNaXJ
EaLR7ejz23e1GFir2NNMayaR0VWaLfjMRtnINsfWuYi6aGWsNevKN5PHdLZKMyZLC6smNs3nu8mI
NWetGBIqCOqXNdutkWyRRzZvEqvIK9fJSZGMNrSxSlalGO/36rS5revt6MySScswSperNZWtT9C3
------=_NextPart_000_0008_01C2BCB0.9CF9AE70--
END_TAG
有点长了，慢慢解释吧。这封要发送的hmtl邮件里只有一张图片popup_thanks.gif. 里面有一句话"This is a multi-part message in MIME format.", 放在第一个boundary出现
之前，这是个描述信息，不用管它。然后就是第一个boundary:
------=_NextPart_000_0008_01C2BCB0.9CF9AE70,它告诉用户的邮件程序这里有一部份的内容。注意这里是--boundary,就是说在boundary前面加了两个-,大家还请注意看最后一个boundary,它的前后都加了两个-，表示整个邮件结束。
Content-type:text/html;charset="gb2312" 说明本部分内容的文档类型是html格式的，
Content-Transfer-Encoding: quoted-printable 说明本部分内容使用 quoted-printable 方法
编码的，当然，下面的内容要确实是 quoted-printable 编码的，否则用户就看不到正确的内容了。
邮件内容没什么好说的，然后是下一个 boundary，这里的东西就是我们要的那个popup_thanks.gif了。
看MIME类型是：Content-Type: image/gif 图片一般就用base64编码，所以这里是
Content-Transfer-Encoding: base64 再看下面是一行
Content-Disposition: attachment;filename="thanks.gif" 
这里的attahment表示此图片作为附件，它还可以是inline，那样的话这个图片就会直接在收件人的
邮件程序的邮件显示区域里显示了。filename指定了在附件区域显示什么样的文件名，这里就把
popup_thanks.gif改成了thanks.gif.下面还有一句
Content-Location: file:///C:/DEV/perl/images/popup_thanks.gif
指定文件的原始路径。好像没用啊？其实很重要，注意html文件里连接这个图片的标签里的src是怎么样写
的？这两个之间要是对不上，那末邮件显示的时候，附件里有图，但邮件还是开了天窗了。好了，基本
就是这样。不，还有个问题，做程序的时候，怎样才能得到需要的编码后的文件啊？perl里怎么样做
我不知道，CPAN里也许有这样的package吧，那位对编码熟悉，也可以自己写，不过我做得时候取巧了。
大家用过IE5的另存为.mht文件吗？对了，就是它！把需要发送的html邮件用IE5在本地打开，再另存为
mht文件，所有的编码都得到了，而且图片的链接关系也都是现成的了，其他的按需要调整一下,帖到你的
程序里就万事大吉。更进一步，如果需要做到像263那样，从页面上接收包括正文，接受者以及各种可能的附件
等信息再发送呢？有点复杂了，也不是这里要讨论的，那位高手做过这些东西，可以把经验贡献出来，让
我们一起学习，这篇就是抛砖之作了。

书面版权所有，书刊转载请与本人联系
参考了系列好文《用PHP发送MIME邮件》，里面有较为详细的MIME介绍，强烈建议阅读
致谢此文作者：Kartic Krishnamurthy 和译者：limodou

本篇文章来源于 黑基网-中国最大的网络安全站点 原文链接：http://www.hackbase.com/lib/2007-05-29/21404.html