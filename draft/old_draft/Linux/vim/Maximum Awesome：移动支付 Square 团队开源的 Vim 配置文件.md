Maximum Awesome：移动支付 Square 团队开源的 Vim 配置文件
JeOamJeOam 3.1k 2013年12月20日 发布
推荐 2 推荐
收藏 5 收藏，1.8k 浏览
译者注：文章的"我们"指 Square 的团队，本文描述了他们开源在 Github 上的一份非常流行的 Vim 配置文件

Xcode 和 Vim 都是非常流行的软件。很久以来，Square Vim 的狂热者已经把 Vim 的设置、快捷方式、插件等编译到一个单独的仓库里面，我们热爱地把这仓库称为：Maximum Awesome。而今天（译者注：文章写于 2013.8.28 ），我们把这个仓库开源了！我们希望任何使用 OS X 的人都能在几分钟内上手 Vim！（译者注：配置文件稍作修改就可以用于其他系统）
请输入图片描述

为什么要这样做？

我们在 Square 经常结对编程（pair program），这是解决复杂问题、带领新成员融入团队和试验新想法的好方法。通过使用基本一致的配置文件，我们就不需要在别人的电脑上重新学一次快捷方式了 -- 一切都被标准化了。这帮助我们减少了大量的摩擦，而得以让我们把精力集中到代码上。

高亮

Maximum Awesome 支持很多你在一个完整的 IDE 里面会期望的那些功能：语法高亮、代码补全、错误信息高亮等等。但 Maximum Awesome 不仅仅是这些！你也可以从下面这些我最爱的插件和快捷方式开始体验：
* 共享的剪贴板：Vim 的寄存器和 OS X 的剪贴板是保持同步的，所以我能像原生的程序那样移动代码
* Command-T 插件：对于那些使用 Sublime 或 TextMate 的人来说，这样的超能力一定早已经熟悉了。当你在使用 Vim 时，使用这个快捷方式 ,t，仅需要打几个字母就可以打开你想打开的文件了。
* NERDTree 插件：浏览一个项目的文件结构、移动文件、新建文件等等，全都不需要离开 Vim。使用 ,d 可以调用"抽屉"(drawer)，或者使用 ,f 打开当前文件 NERDTree。
* Git 整合：fugitive 插件覆盖了大部分的 git 命令，我喜欢 Vim 特有的 :Gblame 和 :Gdiff 插件。通过 :Gblame 可以容易地明白谁写了文件的那一部分，通过 :Gdiff 可以得到一个并排的比较。
* 快速注释代码：使用 \\\ 可以快速注释掉一行，使用 \\可以注释掉选取的区域

里面还包含了些 Vim 没有的插件。Maximum Awesome 来源于iTerm 2 (一个终端的代替品)，一个 tmux 的配置，还有 Solarized color scheme。尽管这些仅仅是表面。转到 README 可以知道更详细的列表。

安装

在你的 Mac 上，Maximum Awesome 会自动为你设置一切。只需要运行下面的命令：

git clone https://github.com/square/maximum-awesome.git && cd maximum-awesome && rake
这会在你的 home 目录下创建一个指向这个仓库的符号链接，这样就可以通过 git pull && rake 轻松地更新了。如果你已经有了 Vim、tmux 的配置文件，它们会被备份。例如，你原来的 .vim 目录会被备份为 .vim.bak 目录。如果你想合并已有的设置，可以去阅读 "定制（Customizing）" 的内容。

如果在安装上有问题，可以在 Github 上建一个 Issue， 我们会尽快处理。

定制

在你的 home 目录下，Maximum Awesome 会创建一个 .vimrc.local 文件，你可以在这个文件定制你 Vim 喜好。然而，我们也欢迎能包含你为自己的配置文件所做的改变，共同为大家改善 Vim 的用法，所以，欢迎 fork 我们的项目，然后发出一些 pull 请求。

玩得开心

不管你是那些穿着 hjkl T-shirt 的人（译者注：意指非常熟悉 vim 用法，因为 h/j/k/l 是 vim 的快捷健），还是刚刚接触到 Vim，我们希望 Maximum Awesome 能帮助他们更容易地写代码。祝码得开心！

