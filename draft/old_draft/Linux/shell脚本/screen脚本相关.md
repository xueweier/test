### screen脚本相关


脚本PythonBash应用服务器Django
这里不会介绍如何使用Screen，只是记录我从脚本创建并操纵Screen会话(session)的一些尝试。 如果想看Screen的一些入门介绍，可以看这里，还有一个quick reference，很有用。

当要同时维护或开发多个项目时，我的习惯是每个项目一个screen会话，每个会话中打开多个窗口。切换项目时，先detach当前session，然后attach另一个项目的session，保持只打开一个控制台窗口，将不必要的窗口或应用程序关闭或隐藏是我的习惯。但总是要来回切换session，有时就显得比较麻烦，第一次切换需要先创建session，每个session基本上都要先启动一个django web服务器，这要好几个步骤，先cd相应目录，然后用virtualenv切换到python虚拟环境，然后再输入"python manage.py runserver"启动服务器，视情况最后还要启动一个vim以及一个ipython终端。整个过程挺麻烦，容易出错，作为一个很懒的程序员，就想通过bash脚本来自动完成这些操作。

写screen脚本并不那么简单，直接将交互控制台下的命令复制到脚本中并不起作用：

Python代码  收藏代码

    #!/bin/bash  
    cd ~/projects/light     # 切换到项目根目录  
    screen -R light         # 开启一个新的screen session，命名为light  
    workon light  
    python manage.py runserver  



脚本运行到第3行会开启一个screen会话，但这条命令并没有完成，只有等待screen会话退出或detach它才结束，然后能才能执行后面的命令。这样，后续的命令并不是在screen会话中执行，而是直接在实际terminal中执行，这当然不是我们想要的。一个解决方法是创建一个detach的session，然后通过screen -X向session发送命令。

Python代码  收藏代码

    #!/bin/bash  
    cd ~/projects/light         # 切换到项目根目录  
    screen -dmS light           # 创建一个detached session  
    screen -S light -p bash -X title server     # 将window标题从默认的bash改成server  
    screen -S light -p server -X stuff $'workon light\n'    # 执行workon light命令  
    screen -S light -p server -X stuff $'python manager.py runserver\n' # 启动服务器  
    screen -r light             # attch会话  



第3行选项-dmS创建一个detached的会话，第4行将默认的window标题从bash改成server，-S选项指定session名称，-p选项选择指定window名称，-X选择执行screen命令，这里执行是修改标题命令。第5行执行screen的stuff命令，相当于你在交互式screen中输入命令"workon light"，接着第6行启动服务器。最后一行attach会话。

我们还可以做得更智能，我们可以检测screen会话是否已经存在，如果已经存在则直接attach该会话。

Python代码  收藏代码

    screen_light() {  
        if [[ $STY == *light* ]]; then      # 如果已经在会话中，不做事情  
            return  
        #elif [ -n "$STY" ]  
        #    screen -S light -X detach  
        fi  
        if screen -ls | grep 'light' > /dev/null 2>&1; then  
            screen -r light  
            return  
        fi  
        cd ~/projects/light         # 切换到项目根目录  
        screen -dmS light           # 创建一个detached session  
        screen -S light -p bash -X title server     # 将window标题从默认的bash改成server  
        screen -S light -p server -X stuff $'workon light\n'    # 执行workon light命令  
        screen -S light -p server -X stuff $'python manager.py runserver\n' # 启动服务器  
        screen -r light             # attch会话  
    }  
    screen_light  


第2行判断是否已经处于相应session中，是则直接返回。第6行判断是否已经存在相应的session，是则只需要重新attach此会话，否则创建新的会话。注意第3-4行被注释的部分，表示当处于其它session中时，先detach该会话，然后再attach需要的session，这似乎很符合逻辑，但不幸的是，这不起作用，因为即使你detach了该会话，剩下的命令仍然是在该会话中执行，最终的效果就是在一个screen会话中attach了另一个会话，即嵌套会话。总的说来，虽然可以向脚本向screen session发送命令，这已经可以做不少事情了，但要离完全用脚本操控screen还有很远，其主要限制有：


    不能detach当前session，然后attach另一个session
    不能获取某个session的所有window
    不能获取某个session某个window正在执行的进程 


如果能够做突破上述限制，我相信能够用script做更多事情。


最后说下screen的自动补全，我一直以为screen不带自动补全的，就想写一个自动全会话的脚本，后来发现它其实是有的，只不过补全的名称是如pid.name的形式，pid是个数字，这意味着必须输入数字才能补全，一点也不人性化，我对它的补全脚本做了点修改，使得能够补全名称。改动/etc/bash_complete.d/screen，修改_screen_sessions函数：

Python代码  收藏代码

    _screen_sessions()  
    {  
       local pattern  
      
       if [ -n "$1" ]; then  
           pattern=".*$1.*"  
       else  
       pattern=".*"  
       fi  
      
       COMPREPLY=(  
           $( command screen -ls | sed -ne  
    's|^['$'\t'']\+[0-9]\+\.\('"$cur"'[^'$'\t'']\+\)'"$pattern"'$|\1|p' )  
           $( command screen -ls | sed -ne  
    's|^['$'\t'']\+\('"$cur"'[0-9]\+\.[^'$'\t'']\+\)'"$pattern"'$|\1|p' )  
       )  
    }  