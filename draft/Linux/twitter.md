http://www.ibm.com/developerworks/cn/linux/l-friendfeed/


http://www.ibm.com/developerworks/cn/linux/l-friendfeed/



从 Linux 命令行更新 Twitter 和 FriendFeed

通过法宝 GNU Wget 和 cURL 让您的朋友保持最新

学习如何使用 GNU Wget 和 cURL 将状态更新发送到 Twitter 和 FriendFeed，而不必使用 Twitter 桌面应用程序。此外，还学习如何从 Linux 命令行跟踪来自 Twitter 和 FriendFeed 的 feed。

0 评论：

Marco Kotrotsos, 创始人和开发人员, Incredicorp

2008 年 11 月 13 日

    +内容

    在 IBM Bluemix 云平台上开发并部署您的下一个应用。

    现在就开始免费试用

    人们选择 Linux 这样的操作系统，是因为它在各方面的优点 — 它的总效用。它稳定、快速、便宜并且可以在所有类型的硬件上运行。它一开始就非常灵活，这主要是因为它强大的命令行接口（CLI）或 shell。

    本文关注 2 个工具 — GNU Wget 和 cURL。您将学习如何使用这 2 个工具将状态更新发送到社交网络站点，而不必使用 Twitter 桌面应用程序，以及如何从命令行跟踪来自 Twitter 和 FriendFeed 的 feed。

    您需要了解 API 方面的细节吗？本文不会深入探究关于 API 的使用的细节。Twitter 和 FriendFeed 都有那样的 API，这种 API 很容易通过一个 Representational State Transfer（REST）界面来访问。
    GNU Wget 的历史

    GNU Wget 是一个灵活的软件，用于从服务器获取数据（例如文件、mp3 和图像）。它的非交互式、健壮和递归特性使得它非常通用，它主要用于从 Web 站点 抓取内容或脱机阅读 HTML 文件。（HTML 页面中的链接将自动调整，以支持该功能）。

    例如，要获取在一个特定的 URL 发现的页面，可以使用以下命令：

    wget http://wikipedia.org/

    该命令将在那个 URL 上发现的 Wikipedia 主页下载到计算机上，且文件名为 index.html，因为那就是 GNU Wget 发现的页面。该工具没有跟踪那个页面上发现的任何链接，但是如果跟踪也很简单：

    wget –r http://wikipedia.org/

    命名工具

    GNU Wget 是由 Hrvoje Nikšić 以他开发的程序 Geturl 为基础开发的。Nikšić 将他的工具的名称改为 Wget，以便与一个名为 GetURL 的 Amiga 工具区分开来，后者具有相同的功能，但是是用 Amiga REXX 编写的。

    在这个命令中，-r 开关告诉 GNU Wget 递归地跟踪那个页面上的所有链接，所以该工具将抓取整个站点。不过，您可能不希望对 Wikipedia 这样的站点使用该开关，因为这会导致为方便本地访问而下载整个数据库，这需要很长的时间（取决于可用的带宽）。

    回页首
    cURL 的历史

    Client URL（cURL）是与 GNU Wget 不同的一种文件传输工具：它主要用于将货币汇率输入到 Internet Relay Chat（IRC）环境中。cURL 是用于执行 URL 操作和以 URL 语法传输文件的强大工具，这意味着可以通过 HTTP、HTTPS、FTP、FTPS 和大多数的其他协议传输大多数类型的文件。

    cURL 应用程序主要用于 Web scraping 和 Web 站点交互自动化，例如表单提交（使用 GET 或 POST 命令）。例如，命令：

    curl http://wikipedia.org

    将请求的结果输出到终端窗口。实际上，在这种情况下，cURL 与浏览器做同样的事情，只不过浏览器呈现的是结果，而 cURL 则是给出它所发现的东西，很多情况下这就是 HTML，但是也可以是任何其他东西。

    注意：若要看到 cURL 发出的请求，可以添加 -v 开关（详细输出），这样不仅会发出请求，而且会返回 cURL 发出的用于获取结果的任何 HTTP 请求。

    掌握了这些背景知识后，让我们将目光转移到一些更有意义的任务上。

    回页首
    使用 GNU Wget 和 cURL 添加 tweet

    Twitter 是一个社交网络和小型博客服务，有了它，您可以通过将简短的文本消息（长度不超过 140 个字符），即所谓的 tweets，发送给您的朋友等，用来回答像 “你在干什么” 之类的问题。为了帮助您更好地理解 GNU Wget 和 cURL 的强大之处，我们首先使用它们将 tweet 添加到 Twitter timeline 中。有 2 种方法可以添加 tweet：使用 Web 站点或一个客户机应用程序，例如 GtkTwitter、Spaz 或 twhirl（实际上是一个 Adobe Air 应用程序）。

    您可以用脚本来编写自己的完整的 Twitter 客户机，这样可以将诸如 twitter 当前系统使用情况或可用性（例如，用一条消息 “server@servername is currently experiencing heavy load”）的任务自动化。您还可以用脚本编写一个自动通知系统。可能性是无穷的。

    要看看这种技术是如何工作的，可以从命令行输入：

    wget-keep-session-cookies-http-user=youremail-http-password=yourpassw-post-
        data="status=hello from the linux commandline" 
            http://twitter.com:80/statuses/update.xml

            如果您对命令行接口使用得不多的话，这段代码看上去可能有点令人生畏。但是别担心：它实际上有一定的格式。我们来看看该命令的几个组成部分：

                wget 运行 GNU Wget 应用程序。
                    keep-session-cookies 保存会话 cookie，而不是将它们留在内存中，这对于需要访问其他页面的站点比较有用。
                        http-user 表示您的用户名。
                            http-password 是密码。
                                post-data 是发送到 Twitter 的数据，您将在此数据上执行动作。
                                    status= 告诉您这是状态更新。

                                    您也可以使用 cURL 完成相同的任务。为此，可输入：

                                    curl -u youremail:yourpassw -d status=”text” http://twitter.com/statuses/update.xml

                                    该命令与前面的 wget 命令做的事情基本相同，但是使用稍微不同的、更加友好的语法。在这里，这两个应用程序之间的不同之处在于默认情况下它们的行为。

                                    以上述方式使用 GNU Wget 会强制地将一个名为 update.xml 的文件下载到本地机器上。下载的这个文件可能有用，但不是必需的。相反，cURL 则将产生的输出发送到标准的输出（stdout）。
                                    查找 Twitter public timeline

                                    要访问 Twitter public timeline，首先必须先找到它。换句话说，首先必须找到将用于访问 Twitter 上的 public feed 的端点。（要获得关于 Twitter API 的信息的链接，请参阅本文后面的 参考资料） 。 最常见也是最容易使用的端点是 public timeline，可以从 http://twitter.com/statuses/public_timeline.rss 访问该端点。用于 FriendFeed public timeline 的端点位于 Google 代码库中（后面的 参考资料 小节中包含了链接）。

                                    FriendFeed API 接收简单的 GET 和 POST 请求。为简化问题，这里使用 public 端点，它可以在http://friendfeed.com/api/feed/public?format=xml 找到。后面将使用 XML。
                                    访问 Twitter public timeline

                                    现在有了 Twitter public timeline 端点，那么，怎样访问它呢？

                                    可以在浏览器中输入以下地址，但更好的做法是从命令行中使用 curl：

                                    curl http://twitter.com/statuses/public_timeline.rss

                                    现在，您可能已经从结果或者构建端点的方式中注意到，您看到的是 RSS 格式的输出。仔细阅读 API 文档可以看到，还有其他的格式。通过将文件的扩展名改为 .xml 或 .json，可以更改输出的格式。

                                    通过使用 grep 命令，可以过滤结果，只获取想要的参数：

                                    curl http://twitter.com/statuses/public_timeline.xml | grep 'text'

                                    查看输出：您需要的是 <text> 标记之间的内容。但是，如果想去掉围绕 tweet 的标记，可以使用 sed 命令。（本文不讨论关于 sed 命令的详细信息，要了解这个神奇的工具的更多信息，请参阅 参考资料）。

                                    curl http://twitter.com/statuses/public_timeline.xml | sed -ne '/<text/s<\/*text>//gp'

                                    现在，去掉进度条，因为它为 timeline 增加了不必要的信息，然后添加 -s 开关：

                                    curl -s http://twitter.com/statuses/public_timeline.xml | sed -ne '/<text/s<\/*text>//gp'

                                    查找 FriendFeed public timeline

                                    您已使用 cURL 来获得用于 Twitter 的 public timeline。现在，您想为 FriendFeed 做相同的事情。在这里，用于 public feed 的 FriendFeed API 端点是 http://friendfeed.com/api/feed/public?format=xml。但是，跟踪用于 FriendFeed 的 public feed 就像大海捞针一样，所以这里将范围缩小到您的朋友的 feed。

                                    再次查看 API 文档。这需要进行一些搜索，但您要找的是 home feed，它的位置是http://friendfeed.com/api/feed/home。当然，您必须验证这个 feed，并且您还必须签名，以便让 feed/home 知道您是谁。幸运的是，cURL 带有验证选项，从而使得这个过程非常简单：

                                    username:password

                                    但是，在 FriendFeed 中并不使用用户名和密码。相反，站点需要一个昵称和远程认证密钥。所以，必须通过 http://friendfeed.com/account/api 进入 FriendFeed 站点并获取它们。在进入该 URL 之后，就可以登录并获得昵称和远程密钥。

                                    使用昵称和远程密钥对发出以下命令：

                                    curl -u "nickname:key" http://friendfeed.com/api/feed/home

                                    其中 nickname:key 是昵称和密钥。

                                    该命令以 JavaScript Object Notation（JSON）格式返回当前的 FriendFeed。要获得 XML，必须添加 format 参数。由于这是一个 get 请求，因此可以将它添加到 URL 的末尾：

                                    curl -u "nickname:key" http://friendfeed.com/api/feed/home?format=xml

                                    很好，不是吗？

                                    回页首
                                    解析输出

                                    通过解析 Twitter feed，您知道需要首先用 sed 对它进行处理，以得到一个真正的、易读的结果。XML 确实易读，但是在查看结果后可以得出结论，您需要解析标记之间的所有东西。然而，这里有一个障碍。XML 并没有包含任何新的行或 CR 代码，它只不过是一个冗长的 XML 字符串。

                                    那么，如何解析它呢？在这里，必须选择一种不同的输出格式。可用的格式有 JSON、XML、RSS 或 Atom。对于这个例子，可以选择 RSS，因为它是最整洁的，而且包含您需要的换行。

                                    查看 RSS feed 中的结果。您需要的是标记之间的东西，所以用一个修改后的 sed 命令处理输出：

                                    curl -s -u "nickname:key" http://friendfeed.com/api/feed/home?format=rss | 
                                        sed -ne '/<ff:body/s/<\/*ff:body>//gp'

                                        您得到了想要的东西！FriendFeed 中的所有条目都可以看到。

                                        回页首
                                        知识总结

                                        从命令行手工运行这些命令来跟踪 feed 并不是恰当的做法。

                                        别忘了，在站点上按 F5 键就可以完成这件的事情。所以，为了尽可能接近命令行，可以使用 shell 脚本将它编写成脚本。当然，也可以使用 Python、Perl 或平台上可用的任何脚本语言，但是从命令运行给这个例子能得到需要的结果。

                                        通过创建一个命名为 lintweet 的脚本，可以将 Twitter 流编写成脚本。当然，您可以随意选择任何名称。清单 1 显示了该脚本。
                                        清单 1. Lintweet.sh

                                        !/bin/bash
                                        while :
                                        do
                                        curl -s http://twitter.com/statuses/public_timeline.xml | sed -ne '/<text/s<\/*text>//gp'
                                        sleep 10
                                        done
                                        exit
                                        Next, make this script executable. Then, run it using the command:
                                        ./lintweet

                                        每过 10 秒钟，窗口被最新的 tweet 更新。对于 Twitter，由于服务条款（TOS）没有限制 public feed 的频率，所以可以通过将 sleep 设置为 1，每过一秒钟便更新一次该设置。不过，您应该减轻服务器的压力，所以还是将它设置为 10。（如果坚持将 sleep 设置为 1，实际上并没有多少可以跟踪的东西，因为结果将是一系列快速流过的更新）。

                                        回页首
                                        结束语

                                        现在，您知道如何使用大多数 Linux 发行版上可用的 2 个工具 — cURL 和 GNU Wget — 从 Linux 命令行获取 tweet。您也可以从 Twitter 和 FriendFeed 手动地跟踪 feed，或者使用一个简单的 shell 脚本跟踪 feed。

                                        您可以扩展 shell 脚本，根据一些关键词进行过滤，使之只显示包含某些单词或短语的状态更新。或者可以将脚本保存到一个文件中，以便获取归档的 Twitter 和 FriendFeed 更新。如果运行 Mac OS X，甚至可以将该脚本钩接到 Growl 等通知系统上（参见 参考资料）。实现这个目标的方法是多种多样的，需要您自己实践。
                                        参考资料
                                        学习

                                            您可以参阅本文在 developerWorks 全球站点上的 英文原文。
                                                在 cURL 站点 和 GNU Wget 站点 上学习更多知识。
                                                    获得更多关于 Twitter 和 FriendFeed 的信息。
                                                        阅读关于 sed 命令的教程和信息。
                                                            “用 Ghosd 和 Perl 创建丰富多彩的屏幕显示内容”（developerWorks，2007 年 2 月）：学习如何设置通知系统。
                                                                在 developerWorks Linux 专区 中可以找到为 Linux 开发人员准备的更多参考资料（包括为 Linux 新手准备的 new to Linux），还可以查阅 最受欢迎的文章和教程。
                                                                    在 developerWorks 上查阅所有 Linux 技巧 和 Linux 教程。
                                                                        随时关注 developerWorks 技术活动和网络广播。 

                                                                        获得产品和技术

                                                                            下载 Twitter API。
                                                                                下载 FriendFeed API。
                                                                                    用可直接从 developerWorks 下载的 IBM 试用软件 构建您的下一个 Linux 开发项目。 

                                                                                    讨论

                                                                                        通过博客、论坛、podcast 和空间加入 developerWorks 社区。 

                                                                                        条评论

                                                                                        请 登录 或 注册 后发表评论。

                                                                                        添加评论:

                                                                                        注意：评论中不支持 HTML 语法


                                                                                        有新评论时提醒我剩余 1000 字符

                                                                                        快来添加第一条评论

                                                                                            IBM PureSystems

                                                                                                IBM PureSystems 系列解决方案是一个专家集成系统
                                                                                                    developerWorks 学习路线图

                                                                                                        通过学习路线图系统掌握软件开发技能
                                                                                                            软件下载资源中心

                                                                                                                软件下载、试用版及云计算


