三种方法

1.修改ssh参数
SSH自动断开连接的原因

用putty/SecureCRT连续3分钟左右没有输入, 就自动断开, 然后必须重新登陆, 很麻烦.

在网上查了很多资料, 发现原因有多种, 环境变量TMOUT引起,ClientAliveCountMax和ClientAliveInterval设置问题或者甚至是防火墙的设置问题. 所以可以这么尝试:

echo $TMOUT
如果显示空白,表示没有设置, 等于使用默认值0, 一般情况下应该是不超时. 如果大于0, 可以在如/etc/profile之类文件中设置它为0.


vi /etc/ssh/sshd_config

1）找到 ClientAliveInterval参数，如果没有就自己加一行

数值是秒，比如你设置为120 ，则是2分钟
ClientAliveInterval 120

2）ClientAliveCountMax
指如果发现客户端没有相应，则判断一次超时，这个参数设置允许超时的次数。如3 、5等自定义。

修改两项参数后如下：

ClientAliveInterval 120
ClientAliveCountMax 0   ###在不允许超时次数
重新加载sshd服务。退出客户端，再次登陆即可验证。

2、 配置客户端

vi /etc/ssh/ssh_config

然后找到里面的
ServerAliveInterval

参数，如果没有，你同样自己加一个就好了，参数意义相同，都是秒数，比如5分钟等。

ServerAliveInterval 300



2.SSH断开后让程序继续执行

博客分类： Linux
SSH 
Shell支持作用控制，有以下命令：
 
command& 让进程在后台运行
jobs 查看当前在后台运行的进程
fg %n 让后台运行的进程n到前台来，这里的n为job number，不是pid
bg %n 让进程n到后台去，或让后台暂停的进程继续运行，n同上
ctrl+z 将一个正在前台执行的命令放到后台，并且暂停
如果当前已经有进程在前台运行了，就可以先用ctrl+z挂起进程，将其转移到后台，再用bg %n让其继续运行。
 
如果后台的任务号有2个，[1],[2]。如果当第一个后台任务顺利执行完毕，第二个后台任务还在执行中时，当前任务便会自动变成后台任务号码“[2]”的后台任务。所以可以得出一点，即当前任务是会变动的。当用户输入“fg”、“bg”和“stop”等命令时，如果不加任何引号，则所变动的均是当前任务。
 
另外ps aux，kill等不做过多说明。



3.进程保持——SSH中screen命令的使用

有时候会碰到这样的情况，用SSH远程到一个linux主机进行一些操作，有时候这些操作要花很长的时间，这样就会出现一些问题，你运行SSH客户端的电脑就不能关了或如果出现网络中断，则当前连接就会中断，就算是你重新打开SSH，也只会打开一个新的session，无法恢复之前的session。
这个时候，你就需要Screen这个工具了，它可以解决这个问题。在安装了Screen之后，在putty中就可以直接使用啦。
常用screen参数：

screen -S yourname -> 新建一个叫yourname的session
screen -ls -> 列出当前所有的session
screen -r yourname -> 回到yourname这个session
screen -d yourname -> 远程detach某个session
screen -d -r yourname -> 结束当前session并回到yourname这个session

使用方法：

screen
//以下^A表示同按“Ctrl + A”键
^A c #Create，开出新的 window
^A n #Next，切换到下个 window
^A p #Previous，前一个 window
^A ^A #在两个 window 间切换
^A w #Windows，列出已开启的 windows 有那些
^A 0…9 #切换到第 0..9 个 window
^A t #Time，显示目前的时间，与系统的 load
^A K #kill window，强制关掉目前的 window
^A ? #Help，显示简单说明
^A d #detach，将目前的 screen session (可能含有多个 windows) 丢到背景执行
当按了 ^A d 把 screen session detach 掉后，会回到还没进 screen 时的状态，此时在 screen session每个window 内跑的 process (无论是前景/背景)都在继续执行，即使 logout 也不影响。
screen -ls #显示所有的 screen sessions
screen -r [keyword] #选择一个 screen session 回来,恢复离线的screen作业,单独输入 screen -r 也行的.
补充说明：
screen是一个多重视窗管理程序，此处所谓的视窗，是指一个全屏幕的文字模式画面。通常只有在使用telnet登入主机或是使用老式的终端机时，才有可能用到screen程序。




