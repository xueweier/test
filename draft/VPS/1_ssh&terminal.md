# ssh
***

### 登陆ssh
	ssh root@104.200.24.213
### 添加新用户
	useradd -d /home/kelu -m kelu
	passwd kelu
	
	linux 查看所有用户组、用户信息 
	groups kelu
	
### 添加用户sudo权限
	vim /etc/sudoers
### 设置shell
	vim /etc/passwd


### ssh配置
	vim /etc/ssh/sshd_config
	port 22 改成自定义的端口

	# Authentication:
	LoginGraceTime 120
	PermitRootLogin no
	StrictModes yes
	
	<!--PermitRootLogin改成no，禁止root登陆ssh-->
	
### 重启ssh服务
	service ssh restart
之后可以再弄个蜜罐或者denyhosts

### 设置密钥登陆
	server:
		mkdir .ssh
	local:
		cd ~/.ssh
		rm known_hosts
		scp -P 1344 kelu.pub kelu@104.200.24.213:/home/kelu/.ssh
		-P是指远方端口，一定要跟在scp后边。

	server:
		mv kelu.pub authorized_keys  //或者
		cat -n kelu.pub > authorized_keys
		sudo chmod 600 au*

	/etc/ssh/sshd_config 
	将RSAAuthentication 和 PubkeyAuthentication 后面的值都改成yes，保存。

	server sshd restart

### 密钥登陆
	ssh -i kelu -p 1344 kelu@104.200.24.213

	或配置config文件
	Host kelu.org
    	HostName 104.200.24.213
    	Port 1344
    	User kelu
    	IdentityFile ~/.ssh/kelu

# 如果有dropbox保存备份的话，这个时候就开始啦

# terminal

### cd后自动ls

`vi .bashrc`，在最后添加

	cdl() {
	    cd "${1}";
	    pwd;
	    ls;
	}
	alias cd=cdl

### 修改.inputrc

设置终端大小写不敏感

	echo "set completion-ignore-case on">>~/.inputrc
	
或者在`/etc/inputrc`里添加，如何生效表示不了解，我直接重启的= =

# vim

`vim .vimrc`

	set encoding=utf-8
	set fileencodings=utf-8,shift-jis,cp936,latin1
	set nocompatible " 关闭VI兼容模式
	set expandtab " 将tab扩展为空格
	set guifont=Monaco:h14 "字体
	set tabstop=4
	set softtabstop=4
	set shiftwidth=4
	set autoindent
	set cindent
	set nu " 显示行号
	colo django " 主题配色
	set autochdir "自动切换工作目录
	syntax on " 语法高亮
	filetype plugin indent on "开启插件
	set so=4 " 设置光标距离上下边界的距离
	set hidden " 允许在有未保存的修改时切换缓冲区
	set ruler " 开启右下角光标位置显示
	set showcmd " 在窗口右下角显示完整命令已输入部分
	set cursorline " 高亮光标所在行
	set ignorecase " 忽略大小写匹配
	set incsearch " 开启输入时的搜索
	set magic " 用于模式匹配的，建议开启
	set hlsearch " 搜索关键词高亮
	set cmdheight=2 " 设置命令行高度
	setlocal noswapfile " 关闭临时文件
	set wildmenu " 启用文本模式的菜单
	set guioptions-=m " 关闭菜单栏
	set guioptions-=T " 关闭工具栏
	set laststatus=2
	set statusline=YUKI.N>\ こんにちは\ %m%r\ %=
	set statusline+=\ %{&ff}\ %Y\ 0x%02.2B
	set statusline+=\ %-21(%11(%l/%L%),%-3v\ %P%)
	set textwidth=76
