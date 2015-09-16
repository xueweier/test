# Git服务器的搭建全部功略
http://yihuayixiaoluo.lofter.com/post/1cb37848_2611241
Git For Windows官网 http://code.google.com/p/msysgit/

tortoisegit官网 http://code.google.com/p/tortoisegit/

copssh用于windows下创建SSH服务器  https://www.itefix.no/i2/copssh

Putty    http://www.putty.org/

Gitolite官方网址：http://github.com/sitaramc/gitolite
Gitolite在线文档：http://sitaramc.github.com/gitolite/

Git官网 http://git-scm.com/

gitweb官网  https://git.wiki.kernel.org/index.php/Gitweb

学习TortoiseGit教程用这个TortoiseGit日常使用指南.pdf

Git教程用这个 Git中文教程.pdf

 
一、准备工作：(服务器端操作)

sudo apt-get install highlight vim #安装vim并高亮语法，安装Gitolite的时候要用到。如果已安装了此部可省掉。
二、Ubuntu下安装SSH Server(服务器端操作)

ubuntu默认并没有安装ssh服务，如果通过ssh链接ubuntu，需要自己手动安装ssh-server。判断是否安装ssh服务，可以通过如下命令进行:

ssh localhost

安装命令:

sudo apt-get install openssh-server

系统将自动进行安装，安装完成以后，先启动服务：

sudo /etc/init.d/ssh start 

启动后，可以通过如下命令查看服务是否正确启动

ps -e|grep ssh 

重启sshd的命令

/etc/rc.d/sshd restart
二、Linux下SSH使用rsa认证方式省去输入密码

1、使用本地机器生成密钥(这里使用Git Bash)

#产生 id_rsa, id_rsa.pub

ssh-keygen -t rsa –C “kanghui@mtc.com.cn”

枫@FREEBIRD ~ $ ssh-keygen -t rsa

Generating public/private rsa key pair.

Enter file in which to save the key(/c/Users/枫/.ssh/id_rsa):

Enter passphrase (empty for no passphrase):

Enter same passphrase again:

Your identification has been saved in/c/Users/枫/.ssh/id_rsa.

Your public key has been saved in /c/Users/枫/.ssh/id_rsa.pub.

The key fingerprint is:

22:d4:67:ef:65:ba:8e:07:f7:d1:f9:0a:b1:79:4f:8c枫@FREEBIRD

2、复制生成的id_rsa.pub公钥文件到远程服务器上

# 将 id_rsa.pub 丢到 Server 上, 大家的 publickey 都需要传到 Server 上.

scp ~/.ssh/id_rsa.pub roan@192.168.35.10:/home/roan/.ssh/authorized_keys

再复制一份到服务器的/temp文件夹下面一会gitolite要用。

scp ~/.ssh/id_rsa.pub roan@192.168.35.10:/tmp/MTC.pub#注意MTC.pub在gitolite里面就是你的用户名

 

还有一个命令简体了命令操作（ssh-copy-id），和上面的操作是一样的。

ssh-copy-id 把本地主机的公钥复制到远程主机的authorized_keys文件上。

ssh-copy-id 也会给远程主机的用户主目录（home）和~/.ssh, 和~/.ssh/authorized_keys设置合适的权限 。

ssh-copy-id-i ~/.ssh/id_rsa.pub roan@192.168.35.10

[注: ssh-copy-id 把密钥追加到远程主机的.ssh/authorized_key 上.]

3、将私钥id_rsa生成Putty格式

putty作为远程客户端在putty不能识别直接从服务器拷贝来的私钥，需要使用puttygen.exe进行格式转换

(1)、打开puttygen.exe--> Conversions --> Import Key

(2)、选择拷贝过来的私钥文件id_rsa

(3)、Save privatekey->id_rsa.ppk(保存私钥)

4、打开putty.exe

1)、Session --> HostName (填写服务器地址或者域名)

2)、Connection -->SSH --> Auth (点Browse选择刚生成的id_rsa.ppk)

3)、open

成功打开后出现如下提示：

login as: root

Authenticating with public key"imported-openssh-key"

----------------------------------------------------------------------------------

当然你有可能会遇到这个错误 [因为我遇到了，呵呵]：

Permissions 0755 for '你配置的公钥文件路径'are too open.

这个是因为这几个文件权限设置的有点问题

执行命令： chmod 600 /root/.ssh/authorized_keys
三、Linux 使用 Gitolite 架设 Git Server

想要控管 User / Project 权限, 而且还想要控管 branch / tag 等读写权限, 则需要靠 Gitolite 等套件来协助.

§  gitolite - SSH-basedgatekeeper for git repositories
相关资料准备

§  系统: Ubuntu Linux

§  Server: 192.168.35.10

§  Project name:project_name

§  Gitosis (Git)Repository 位置: /home/git/repositories

§  Group name: git
系统套件安装

§  sudo apt-get install gitolite git-core   

    安装完成会出现此讯息: No adminkey given - not setting up gitolite.

创建服务器上的Git帐号 #创建的git帐号是不能直接登录使用的只能通过命令sudosu – git来操作。

roan@ubuntu:~$ sudo adduser --system --shell /bin/bash --gecos'git version control' --group --disabled-password --home /home/git git

[sudo] password for roan:

正在添加系统用户"git" (UID 116)...

正在添加新组"git" (GID 125)...

正在将新用户"git" (UID 116)添加到组"git"...

创建主目录"/home/git"...
Gitolite Server 架设

1.       ssh roan@192.168.35.10 # 登录到Git Server

2.      sudo su – git  #切换到git帐号

3.      gl-setup /tmp/MTC.pub # 加入管理者的 Public key(公钥). 注意: 此档名即是帐号名称, 不要使用 id_rsa.pub 当档名, 建议用 "帐号.pub" 当档名

命令过程如下   #按大写ZZ两次退出Vim-tiny #按q!可以退出vim按wq保存并退出vim

 git@ubuntu:~$ gl-setup /tmp/MTC.pub

The default settings in the rcfile (/home/git/.gitolite.rc) are fine for most

people but if you wish to makeany changes, you can do so now.

hit enter...

Select an editor.  To change later, run 'select-editor'.

  1. /bin/ed

  2. /bin/nano        <---- easiest

  3. /usr/bin/vim.basic

  4. /usr/bin/vim.tiny

Choose 1-4 [2]: 3

此时要特别注意如果要用Gitweb的话一定要

把$REPO_UMASK =0077; 改成 $REPO_UMASK = 0027; # gets you 'rwxr-x---'。

creating gitolite-admin...

Initialized empty Gitrepository in /home/git/repositories/gitolite-admin.git/

creating testing...

Initialized empty Gitrepository in /home/git/repositories/testing.git/

[master (root-commit) f489c2a]start

 2 files changed, 6 insertions(+)

 create mode 100644 conf/gitolite.conf

 create mode 100644 keydir/mtc.pub

4.      exit
Gitolite Server 设定专案、新增帐号（客户端操作）

1.       Gitolite 的专案权限 / 帐号管理是使用 Git 来管理, 专案名称: gitolite-admin.git

2.      git clone git@192.168.35.10:gitolite-admin # 因为 Gitolite 是用 gitolite-admin.git 来管理, 所以需要抓下来修改、设定(未来所有管理也是如此)

3.      cdgitolite-admin # 会看到下述

§  conf/gitolite.conf# 设定档, 设定谁可以读写哪个专案的 Repository

§  keydir # 目录, 放每个帐号的 public key. 放置的档案命名: user1.pub, user2.pub(user1, user2.. 为帐号名称(档名 = 帐号), 建议使用 "帐号.pub" 当档名)
设定专案权限（客户端操作）

1.       cdgitolite-admin

2.      vimconf/gitolite.conf # 会看到下述, 不要动他, 于最下方设定自己的 Group / 专案名称即可.

repo   gitolite-admin
RW+     =   admin
repo    testing
RW+     =   @all

3.      由此档案新增 / 修改后, commit + push 即可.
建立专案

1.       git clonegitolite@example.com:testing # 对应 gitolite.conf 的 repo testing, 会出现下述讯息

Cloning intotesting...
warning: You appear to have cloned an empty repository.

2.      cd testing

3.      touch readme

4.      git add .

5.       git commit -m'add readme'

6.      git push originmaster
新增帐号

1.       cd gitolite-admin

2.      cp/tmp/user1.pub keydir/user1.pub # 请依照实际帐号命名, 不要取 user1, user2

3.      cp/tmp/user1.pub keydir/user1@machine.pub # 若相同帐号, 则使用 user@machine.pub

4.      cp/tmp/user2.pub keydir/user2.pub

5.       git addkeydir/user1.pub keydir/user1@machine.pub keydir/user2.pub

6.      git commit -m'add user1, user1@machine, user2 public key'

7.       git push
gitolite.conf 更多设定条件

下述摘录自: Gitolite 构建 Git 服务器 - 授权使用者建立属于自己的空间 (User 下面可以建 N 个 Repository), 在此就不记载, 请自行详见: 此文的章节 2.4.3

#取自 2.3.1 授权文件基本语法
@admin = jiangxin wangsheng

repogitolite-admin
RW+    = jiangxin

repoossxp/.+
C       = @admin
RW     = @all

repotesting
RW+                  =   @admin
RW     master        =   junio
RW+     pu           =   junio
RW     cogito$        =   pasky
RW     bw/           =  linus
-                       =   somebody
RW     tmp/           =  @all
RW      refs/tags/v[0-9] =   junio

#取自 2.3.3 ACL
repo testing
RW+   = jiangxin @admin
RW    = @dev @test
R      = @all
gitolite.conf 语法说明 repo 语法

§  repo 语法: <<span style="color:#333333">权限> [零个或多个正规表示式批配的引用] = [...]

§  每条指令必须指定一个权限, 权限可以用下面任何一个权限的关键字: C, R, RW, RW+, RWC, RW+C, RWD, RW+D, RWCD,RW+CD

§  C : 建立

§  R : 读取

§  RW : 读取 + 写入

§  RW+ : 读取 + 写入 + 对 rewind 的 commit 做强制 Push

§  RWC : 授权指令定义 regex (regex 定义的 branch、tag 等), 才可以使用此授权指令.

§  RW+C : 同上, C 是允许建立和 regex 配对的引用 (branch、tag 等)

§  RWD : 授权指令中定义 regex (regex 定义的 branch、tag 等), 才可以使用此授权指令.

§  RW+D : 同上, D 是允许删除和 regex 配对的引用 (branch、tag 等)

§  RWCD : 授权指令中定义 regex (regex 定义的 branch、tag 等), 才可以使用此授权指令.

§  RW+CD : C 是允许建立和 regex 配对的引用 (branch、tag 等), D 是允许删除和 regex 配对的引用 (branch、tag 等)

§  - : 此设定为不能写入, 但是可以读取

§  注: 若 regex 不是以 refs/ 开头, 会自动于前面加上 refs/heads/
群组

§  @all 代表所有人的意思

§  @myteam user1user2 : user1, user2 都是属于 myteam 这个群组

    #testing "owner name" = "one line of description"
    testing "admin" = "adadfsaf one line of description"

常用命令

下述全部都在 gitolite-admin.git 内操作

§  新增帐号

§  cp/tmp/user1.pub keydir/user1.pub # 注意: 档名要取 "帐号.pub"

§  新增专案

1.       vimconf/gitolite.conf # 增加 repo, 例如:

repo testing
RW @all

2.           git clone gitolite@example.com:testing

§  设定专案

§  vimconf/gitolite.conf # 增加 repo, 设定读写群组、使用者的权限
扩展命令1，用于本地已建好的repo关链到远程服务器

1.       cd ~/

2.      mkdir DVBT-TEST

3.      cd DVBT-TEST

4.      git init

5.       git remote add origin git@192.168.35.10:DVBT-TEST.git

 # gitosis 会自行于/srv/gitosis/repositories 新增

6.      touch readme

7.       git add .

8.      git commit -m'initial'

9.      git push originmaster:refs/heads/master # 或 git push origin master

 
扩展命令2，用于查看远程服务器数据库

$ ssh git@192.168.35.10

hello mtc, this is gitolite 2.3-1 (Debian) running ongit 1.7.10.4

the gitolite config gives you the followingaccess:

    R   W      DVBT-TEST

    R   W      gitolite-admin

   @R_ @W_     testing

Connection to 192.168.35.10 closed.
用Gitweb以HTTP方式查看Git代码仓

Gitweb是用来在网页上查看GIT代码库的。没有Gitweb，我们仍然可以使用Git客户端从Gitolite服务器上获取代码。Gitweb演示：http://git.kernel.org/

安装GitWeb

第一步: 安装gitweb和highlight，Gitweb安装在/usr/share/gitweb #这里就是你看到的网页地方的代码

$ sudo apt-get installhighlight gitweb       #自动会安装apache2

第二步：修改gitweb.conf

$ sudo vim /etc/gitweb.conf

#change $projectroot to /home/git/repositories

#change $projects_list to /home/git/projects.list

# AddHighlighting at the end #在网页中查看代码的时候可以高亮语法

$feature{'highlight'}{'default'}= [1];

第三步：修改git仓库文件夹的权限

Changegitolite instance to allow access for gitweb. First append www-data to gitgroup so gitweb can access the repos, then change the permissions for git reposand the projects list, finally restart apache:

sudo usermod -a -G git www-data

sudo chmod g+r/home/git/projects.list

sudo chmod -R g+rx /home/git/repositories

sudo service apache2 restart或者sudo /etc/init.d/apache2 restart #效查一样都是重启apache

第四步：Finallyyou need to tell gitolite which repo you want to show up in gitweb. To do thisedit the gitolite.conf file from the gitolite-admin.git repo:

 

repotesting

  RW+ = @all

  R = gitweb
补充：

the best solution I found for this was to edit the .gitolite.rc file andto change the umask.

1.   the default umask for repositories is0077; change this if you run stuff

2.   like gitweb and find it can't read therepos. Please note the syntax; the

3.   leading 0 is required

#$REPO_UMASK = 0077; # gets you 'rwx------'
#$REPO_UMASK = 0027; # gets you 'rwxr-x---'
$REPO_UMASK = 0022; # gets you 'rwxr-xr-x'

 
Gitweb自定义内容如下：

# useradd start

$feature{'highlight'}{'default'}= [1];

$feature{'highlight'}{'override'}= 1;

$feature{'blame'}{'default'}= [1];

$feature{'blame'}{'override'}= 1;

$feature{'pickaxe'}{'default'}= [1];

$feature{'pickaxe'}{'override'}= 1;

$feature{'search'}{'default'}= [1];

$feature{'search'}{'override'}= 1;

$feature{'grep'}{'default'}= [1];

$feature{'grep'}{'override'}= 1;

$feature{'snapshot'}{'default'}= ['tgz', 'tbz2', 'zip'];

$feature{'snapshot'}{'override'}= 1;

$feature{'avatar'}{'default'}= ['gravatar'];

$feature{'avatar'}{'override'}= 1;

#$fallback_encoding= 'utf-8';

$fallback_encoding= 'EUC-CN';

#$projects_list_description_width = 50;

$site_name= "MTC git Server";

# Title

$home_link_str= 'MTC Projects';

 
Gitweb主题（只是让Gitweb更好看一些，可选择安装） https://github.com/kogakure/gitweb-theme                 