# Git

### Git配置

	git config --global user.name "robbin"   
	git config --global user.email "fankai#gmail.com"
	git config --global color.ui true
	git config --global alias.co checkout
	git config --global alias.ci commit
	git config --global alias.st status
	git config --global alias.br branch
	git config --global core.editor "mate -w"    # 设置Editor使用textmate
	git config -1 #列举所有配置
 
`用户的git配置文件~/.gitconfig`

### 初始化

	cd /path/to/your/project
	git init
	git remote add origin https://kelvinblood@bitbucket.org/kelvinblood/python.git
	git add .
	git commit -m 'Initial commit with contributors'
	git push -u origin master

### 当前状态

	检查当前文件状态
	$ git status

### 忽略某些文件
	
	`.gitignore`

### 提交更新

	git add . # 跟踪新文件，add file into staged area
	git commit -a -m "注释文字"
	
### 查看提交历史

	git log
	git log -p -2 # 我们常用 -p 选项展开显示每次提交的内容差异，用 -2 则仅显示最近的两次更新
	git log --stat # 仅显示简要的增改行数统计
	其他选项：
	git log --pretty=oneline #用 oneline 将每个提交放在一行显示,还有 short，full 和 fuller 可以用,format，可以定制要显示的记录格式，这样的输出便于后期编程提取分析,例如
	git log --pretty=format:"%h - %an, %ar : %s"

### 撤消操作

	修改最后一次操作
	$ git commit -m ‘initial commit’
	$ git add forgotten_file
	$ git commit —amend
	
	取消已经暂存的文件
	$ git reset HEAD [文件名] # 变成已修改未暂存
	
	取消对文件的修改
	$ git checkout — [文件名] 这个命令有点危险，可以使用stashing 和分支来处理
	
	记住，任何已经提交到 Git 的都可以被恢复。即便在已经删除的分支中的提交，或者用 —amend 重新改写的提交，都可以被恢复（关于数据恢复的内容见第九章）。所以，你可能失去的数据，仅限于没有提交过的，对 Git 来说它们就像从未存在过一样。

远程仓库的使用

查看

$ git remote
$ git remote -v
$ git remote add [shortname] [url] # 添加远程仓库
$ git remote add pb git://github.com/paulboone/ticgit.git
git remote show [remote-name]
抓取
$ git fetch [remote-name] # 从远程仓库抓取数据
$ git fetch pb
如果是克隆了一个仓库，此命令会自动将远程仓库归于 origin 名下。所以，git fetch origin 会抓取从你上次克隆以来别人上传到此远程仓库中的所有更新（或是上次 fetch 以来别人提交的更新）。有一点很重要，需要记住，fetch 命令只是将远端的数据拉到本地仓库，并不自动合并到当前工作分支，只有当你确实准备好了，才能手工合并。

如果设置了某个分支用于跟踪某个远端仓库的分支（参见下节及第三章的内容），可以使用 git pull 命令自动抓取数据下来，然后将远端分支自动合并到本地仓库中当前分支。在日常工作中我们经常这么用，既快且好。实际上，默认情况下 git clone 命令本质上就是自动创建了本地的 master 分支用于跟踪远程仓库中的 master 分支（假设远程仓库确实有 master 分支）。所以一般我们运行 git pull，目的都是要从原始克隆的远端仓库中抓取数据后，合并到工作目录中的当前分支。

推送
git push [remote-name] [branch-name]

删除和重命名
$ git remote rename pb paul
$ git remote rm paul

标签操作

Git 使用的标签有两种类型：轻量级的（lightweight）和含附注的（annotated）。轻量级标签就像是个不会变化的分支，实际上它就是个指向特定提交对象的引用。而含附注标签，实际上是存储在仓库中的一个独立对象，它有自身的校验和信息，包含着标签的名字，电子邮件地址和日期，以及标签说明，标签本身也允许使用 GNU Privacy Guard (GPG) 来签署或验证。一般我们都建议使用含附注型的标签，以便保留相关信息；当然，如果只是临时性加注标签，或者不需要旁注额外信息，用轻量级标签也没问题。



$ git tag # 在控制台打印出当前仓库的所有标签
$ git tag -l ‘v0.1.*’ # 搜索符合模式的标签


h3. 新建标签
 


$ git标签分为两种类型：轻量标签和附注标签。轻量标签是指向提交对象的引用，附注标签则是仓库中的一个独立对象。

$ git tag [tagname] # 创建轻量标签
$ git tag -a [tagname] -m "init0.1.2" # 创建附注标签,a即annotated的缩写
$ git checkout [tagname] # 切换到标签
$ git show [tagname] # 查看标签信息
$ git tag -d [tagname] # 删除标签
$ git tag -a [tagname] 9fbc3d0 # 补打标签
$ git push origin [tagname] # 标签发布,将[tagname]标签提交到git服务器
$ git push origin --tags # 标签发布,将本地所有标签一次性提交到git服务器
$ git tag -s v1.5 -m 'my signed 1.5 tag' #如果你有自己的私钥，还可以用 GPG 来签署标签，只需要把之前的 -a 改为 -s （译注： 取 signed 的首字母）
小技巧


$ git config color.ui true # 为git控制台添加颜色
$ git config --global alias.last 'log -1 HEAD' # 别名

### 查看、添加、提交、删除、找回，重置修改文件

	git help <command>  # 显示command的help
	git show            # 显示某次提交的内容
	git show $id
	
	git add <file>      # 将工作文件修改提交到本地暂存区
	git add .           # 将所有修改过的工作文件提交暂存区
	
	git rm <file>       # 从版本库中删除文件
	git rm <file> --cached  # 从版本库中删除文件，但不删除文件
	
	git reset <file>    # 从暂存区恢复到工作文件
	git reset -- .      # 从暂存区恢复到工作文件
	git reset --hard    # 恢复最近一次提交过的状态，即放弃上次提交后的所有本次修改
	
	git ci <file>
	git ci .
	git ci -a           # 将git add, git rm和git ci等操作都合并在一起做
	git ci -am "some comments"
	git ci --amend      # 修改最后一次提交记录
	
	git revert <$id>    # 恢复某次提交的状态，恢复动作本身也创建了一次提交对象
	git revert HEAD     # 恢复最后一次提交的状态
 

### 查看文件diff

 git diff <file>     # 比较当前文件和暂存区文件差异
 git diff
 git diff <$id1> <$id2>   # 比较两次提交之间的差异
 git diff <branch1>..<branch2> # 在两个分支之间比较 
 git diff --staged   # 比较暂存区和版本库差异
 git diff --cached   # 比较暂存区和版本库差异
 git diff --stat     # 仅仅比较统计信息
 

### 查看提交记录
 git log
 git log <file>      # 查看该文件每次提交记录
 git log -p <file>   # 查看每次详细修改内容的diff
 git log -p -2       # 查看最近两次详细修改内容的diff
 git log --stat      #查看提交统计信息
 

Mac上可以使用`tig`代替diff和log，brew install tig


### Git 本地分支管理


查看、切换、创建和删除分支




 git br -r           # 查看远程分支
 git br <new_branch> # 创建新的分支
 git br -v           # 查看各个分支最后提交信息
 git br --merged     # 查看已经被合并到当前分支的分支
 git br --no-merged  # 查看尚未被合并到当前分支的分支
 
 git co <branch>     # 切换到某个分支
 git co -b <new_branch> # 创建新的分支，并且切换过去
 git co -b <new_branch> <branch>  # 基于branch创建新的new_branch
 
 git co $id          # 把某次历史提交记录checkout出来，但无分支信息，切换到其他分支会自动删除
 git co $id -b <new_branch>  # 把某次历史提交记录checkout出来，创建成一个分支
 
 git br -d <branch>  # 删除某个分支
 git br -D <branch>  # 强制删除某个分支 (未被合并的分支被删除的时候需要强制)
 

 分支合并和rebase




 git merge <branch>               # 将branch分支合并到当前分支
 git merge origin/master --no-ff  # 不要Fast-Foward合并，这样可以生成merge提交
 
 git rebase master <branch>       # 将master rebase到branch，相当于：
 git co <branch> && git rebase master && git co master && git merge <branch>
 

 Git补丁管理(方便在多台机器上开发同步时用)




 git diff > ../sync.patch         # 生成补丁
 git apply ../sync.patch          # 打补丁
 git apply --check ../sync.patch  #测试补丁能否成功
 

 Git暂存管理




 git stash                        # 暂存
 git stash list                   # 列所有stash
 git stash apply                  # 恢复暂存的内容
 git stash drop                   # 删除暂存区
 

Git远程分支管理




 git pull                         # 抓取远程仓库所有分支更新并合并到本地
 git pull --no-ff                 # 抓取远程仓库所有分支更新并合并到本地，不要快进合并
 git fetch origin                 # 抓取远程仓库更新
 git merge origin/master          # 将远程主分支合并到本地当前分支
 git co --track origin/branch     # 跟踪某个远程分支创建相应的本地分支
 git co -b <local_branch> origin/<remote_branch>  # 基于远程分支创建本地分支，功能同上
 
 git push                         # push所有分支
 git push origin master           # 将本地主分支推到远程主分支
 git push -u origin master        # 将本地主分支推到远程(如无远程主分支则创建，用于初始化远程仓库)
 git push origin <local_branch>   # 创建远程分支， origin是远程仓库名
 git push origin <local_branch>:<remote_branch>  # 创建远程分支
 git push origin :<remote_branch>  #先删除本地分支(git br -d <branch>)，然后再push删除远程分支
 

Git远程仓库管理


GitHub




 git remote -v                    # 查看远程服务器地址和仓库名称
 git remote show origin           # 查看远程服务器仓库状态
 git remote add origin git@ github:robbin/robbin_site.git         # 添加远程仓库地址
 git remote set-url origin git@ github.com:robbin/robbin_site.git # 设置远程仓库地址(用于修改远程仓库地址)
 git remote rm <repository>       # 删除远程仓库
 

创建远程仓库




 git clone --bare robbin_site robbin_site.git  # 用带版本的项目创建纯版本仓库
 scp -r my_project.git git@ git.csdn.net:~      # 将纯仓库上传到服务器上
 
 mkdir robbin_site.git && cd robbin_site.git && git --bare init # 在服务器创建纯仓库
 git remote add origin git@ github.com:robbin/robbin_site.git    # 设置远程仓库地址
 git push -u origin master                                      # 客户端首次提交
 git push -u origin develop  # 首次将本地develop分支提交到远程develop分支，并且track
 
 git remote set-head origin master   # 设置远程仓库的HEAD指向master分支
 

也可以命令设置跟踪远程库和本地库




 git branch --set-upstream master origin/master
 git branch --set-upstream develop origin/develop





git reset --hard HEAD^
回退上一个版本
head^^就是上上一个版本，
head~100就是一百个版本前。

git log --pretty=oneline
查看提交信息