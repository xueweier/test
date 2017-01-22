# fail2ban 防止暴力破解各种程序密码

        fail2ban可以监视你的系统日志，然后匹配日志的错误信息（正则式匹配）执行相应的屏蔽动作（一般情况下是调用防火墙屏蔽），如:当有人在试探你的SSH、SMTP、FTP密码，只要达到你预设的次数，fail2ban就会调用防火墙屏蔽这个IP。

debian下安装fail2ban 


apt-get install fail2ban

      安装完成后主要的配置文件在/etc/fail2ban目录下，里面有两个文件和两个目录：

      fail2ban.conf 配置文件里面定义了fail2ban记录的日志等级、日志文件的位置以及socket，默认配置如下：       
 loglevel = 3
logtarget = /var/log/fail2ban.log
socket = /var/run/fail2ban/fail2ban.sock
       jail.conf 里面定义了对那些服务进行监控，以及使用的一些策略
       jail.conf 里面开头是默认全局配置块[DEFAULT]，默认配置如下：
 [DEFAULT]
#忽略哪些IP，可以是具体IP、CIDR类型的地址，多个IP用空格分开
ignoreip = 127.0.0.1

#设置IP被锁住的时间，单位为秒
bantime  = 600

#检测时间，在此时间内超过规定的次数会激活fail2ban
findtime  = 600

#尝试的次数
maxretry = 3

#日志检测机器，有"gamin", "polling" and "auto"三种模式。
backend = polling

#发送报警邮件的地址
destemail = root@localhost #默认的动作执行行为，在action.d目录下有各种行为策略，默认是iptables-#multiport
banaction = iptables-multiport

#0.8.1版本后fail2ban默认用sendmail MTA
mta = sendmail

#默认使用tcp协议
protocol = tcp

#定义了各种行动的参数
#banaction参数在action.d目录下具体定义，name port protocol 也可以自己定义
#只禁止IP
action_ = %(banaction)s[name=%(__name__)s, port="%(port)s", protocol="%(protocol)s]
#即禁止IP又发送email
action_mw = %(banaction)s[name=%(__name__)s, port="%(port)s", protocol="%(protocol)s]
              %(mta)s-whois[name=%(__name__)s, dest="%(destemail)s", protocol="%(protocol)s]
#禁止IP、发送email、报告有关日志			  
action_mwl = %(banaction)s[name=%(__name__)s, port="%(port)s", protocol="%(protocol)s]
               %(mta)s-whois-lines[name=%(__name__)s, dest="%(destemail)s", logpath=%(logpath)s]

#如果没有定义行为，则默认的行为为action，可选择action_,action_mw, action_mwl 等		
action = %(action_)s

默认配置文件含有此模块
#定义子模块名
[ssh]
#是否激活
enabled = true
#定义port，可以是数字端口号表示，也可以是字符串表示
port= ssh
#过滤规则，在filter.d目录下定义
filter	= sshd
#检测日志的路径
logpath  = /var/log/auth.log
#尝试的次数，覆盖了全局配置的
maxretry = 6
#banaction 在action.d目录下定义，此参数值会替换action中选用的默认行为中定义的banaction参数
banaction = iptables-allports
#注意 port protocol banaction 可以不用分开定义，直接使用action定义也可以，例如：
#action   = iptables[name=SSH, port=ssh, protocol=tcp]
#在子模块中定义的port protocol banaction 都会在action_ action_mw, action_mwl中替换成具体的设置值。

       filter.d 目录里面定义的是根据日志文件进行过滤的规则，主要是利用正则匹配出现错误的关键字。

       action.d目录里面是根据过滤的规则对相应的IP采取什么样的行为

       debian下默认安装完成后会在/etc/fail2ban目录底下有一个jail.conf配置文件，里面已经加入了很多常用的检测服务，不过里面的参数讲解不详细，如果想知道关于配置文件的更多信息可以查看/usr/share/doc/fail2ban/examples/jail.conf

      fail2ban-server 命令：启动fail2ban服务
            -b 在后台启动
            -f 在前台启动
            -s 指定socket file

     fail2ban-client 命令：编辑和配置服务
            fail2ban-client [OPTIONS] <COMMAND>
            OPTIONS 不做介绍
            COMMAND
            start <jail> 启动服务和所有的规则，或者某个规则
            reload <jail> 重新加载所有配置或者单独加载某个jail的配置
            stop <jail> 停止服务和所有的规则或者某个规则
            status <jail> 查看目前服务运行情况或者某个规则
            set/get loglevel <LEVEL> 设置或者查看日志等级
            set <JAIL> addignoreip/delignoreip <IP>  在忽略IP列表中添加或者删除某个IP


     faillog 命令：显示日志中记录的登陆失败的记录
           -a 显示日志中所有的失败的记录
           -l 设置登录失败后被锁定的秒数
           -r 重置日志中的登陆失败的信息

下面举个SSH防暴力破解的例子：
jail.conf中SSH模块的配置如下：
[ssh]
enabled = true
port	= 22222
filter	= sshd
logpath  = /var/log/auth.log
maxretry = 6
测试如下：

1 fail2ban启动在10.1.1.174，在10.1.1.175上登陆10.1.1.74，在10.1.1.175连续六次故意输错密码：
 

2 在10.1.1.174上查看相关信息
 
如图显示10.1.1.175已经被屏蔽

3 fail2ban 自动生成的iptables规则
 

4 在10.1.1.175继续登陆10.1.1.174
 

5 10分钟后查看日志，发现10.1.1.175已经被解禁
 


     fail2ban不止可以防止SSH破解，还可以防止很多类似的应用，大家可以继续摸索。