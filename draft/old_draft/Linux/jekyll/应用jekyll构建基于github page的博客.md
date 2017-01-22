---
layout: post
title: 应用jekyll构建基于github page的博客
category: other
---
基于github page的博客好处:

bq. 
简单至极，且完全免费
可以支持rss订阅，评论功能
依赖于git，文章原生支持版本控制(对比)，更有利于知识库类文章
可以使用vim编写文章，写的时候像在写代码，更符合程序员的习惯

本博客此次除了启用全新个人域名saberma.me，将做以下调整

bq. 
美化网站风格，更加的清新:)
页面布局调整为html5、css3架构
文章可按分类显示
代码增加高亮显示
增加rss feed输出

h2. 挑个新的博客模板

要求是基于html5、css3的免费模板，google之，发现个好地方: "http://freehtml5templates.com":http://freehtml5templates.com
一页一页的挑吧，总会找到自己喜欢的，可以根据右下角的Tag Cloud进行筛选，现在看到本站的新样子就是找好久才看中的模板

h2. 绑定独立域名

在godaddy中注册 "saberma.me":http://saberma.em 域名，.me专用于博客类型，但比.com贵一些，且没有优惠

# 注册完域名后，在域名管理中增加A record并指向207.97.227.245
# 在你的github项目下增加CNAME文件，内容为你的域名，如 "http://github.com/saberma/saberma.github.com/blob/master/CNAME":http://github.com/saberma/saberma.github.com/blob/master/CNAME

具体参考 "http://pages.github.com":http://pages.github.com 中 @Custom Domains@ 部分的内容

h2. pygments代码高亮

h3. 安装pygments

{% highlight bash %}
# On Ubuntu 安装
sudo apt-get install python-pygments
{% endhighlight %}

"完整安装说明":http://wiki.github.com/mojombo/jekyll/install

h3. 生成高亮显示的css文件

选择喜欢的样式，记下名称
"http://pygments.org/demo/6622":http://pygments.org/demo/6622

我选择的是fruity style，作为pygmentize命令style的参数值

{% highlight bash %}
# 生成相应的css
pygmentize -S fruity -f html > stylesheets/syntax.css
{% endhighlight %}

"参考pygments Command line usage":http://pygments.org/docs/quickstart/

h3. 如何使用

语法高亮的代码段

<pre>
{{ "{% highlight ruby" }} %}
def foo
  puts 'foo'
end
{{ "{% endhighlight" }} %}
</pre>

highlight后面第一个参数为language，如php，也可以是ruby控制台irb，更多lanuages可以查询 "http://pygments.org/docs/lexers/":http://pygments.org/docs/lexers/
第一个参数为必填，不填会导致_site目录生成不了相应的html文件，第二个参数显示行号

"参考jekyll说明":http://wiki.github.com/mojombo/jekyll/liquid-extensions

h2. Markdown标记与Liquid逻辑处理

h3. 两个知识点

* 为避免直接编写html代码，编写文章时，内容需要加入标记信息，即Markdown
* 博客中都是需要经过处理的，比如逻辑判断处理、循环处理，jekyll应用liquid模板语言进行这些处理

h3. Markdown

标记语言有很多种，如textile
这些标记语言会被标记引擎转换，输出成相应的目标格式（大部分情况是输出成html）
引擎也有很多种，不同的编程语言有不同的实现，ruby常用的引擎有RedCloth

h3. Liquid

简单来说，凡是看到{{ "{{" }}}}或者{{ "{%" }} %}包含的内容都是会被Liquid引擎处理的

比如*将日期格式化*的liquid语句

<pre>
{{ "{{ post.date | date_to_string" }} }}
</pre>

除了标记的Liquid语法外，jekyll还扩展出了几个便利的方法，其中有上面介绍的highlight方法
"jekyll liquid扩展":http://wiki.github.com/mojombo/jekyll/liquid-extensions
"liquid参考资料":http://github.com/tobi/liquid/wiki/Liquid-for-Designers

h2. 整合评论

由于github page最终生成的都是静态html页面，所以是没有评论功能呢
但我们利用disqus实现在线评论功能，先到 "http://disqus.com":http://disqus.com 注册帐号(免费)
注册成功后，为简单起见，只要把 @_includes/post.html@ 中的saberma替换为你的注册帐号就行了(disqus_url输入你实际的域名)

h2. 整合rss订阅

因为jekyll可以生成blogs列表，所以我们可以编写atom.xml，由jekyll生成最终xml结果
"这是我的atom.xml文件":/atom.xml

将生成的xml地址提交至 "feedsky.com":http://www.feedsky.com ，由feedsky进行管理和美化

"Setting Up an Atom Feed at GitHub Pages":http://elemel.se/2009/01/25/setting-up-an-atom-feed-at-github-pages.html

h2. 你也想弄一个github page博客?

最快的做法是 "fork我的博客":http://github.com/saberma/saberma.github.com ，git clone到你的电脑
然后修改成你的，具体需要调整的地方是:

# 删除_posts中的文章
# 按上面介绍的[整合评论]修改 @_includes/post.htm@ 文件
# 按上面介绍的[整合rss]修改 @atom.xml@ 文件
# 修改CNAME的内容为你的独立域名
# 运行jekyll，看看效果
# 上传!ok，访问你的blog地址看看

h2. 参考资源

"publishing-a-blog-with-github-pages-and-jekyll":http://blog.envylabs.com/2009/08/publishing-a-blog-with-github-pages-and-jekyll/
"jekyll wiki":http://github.com/mojombo/jekyll/wiki
"http://pages.github.com":http://pages.github.com 






使用 GitHub, Jekyll 打造自己的独立博客

GitHub 是一个代码托管网站，现在很多开源项目都放在GitHub上。 利用GitHub，可以让全球各地的程序员们一起协作开发。GitHub 提供了一种功能，叫  GitHub Pages , 利用这个功能，我 们可以为项目建立网站，当然，这也意味着我们可以通过 GitHub Pages 建立自己的网站。

Jekyll 是一个简单的，针对博客设计的静态网站生成器。使用 GitHub 和 Jekyll，我们可以打造自己的独立博客，你可以自由地定制网站的风格，并且这 一切都是免费的。

这是我在GitHub上自己建立的 博客  及 源代码  （两个分支），在下文的讲解中，你可以随时查看博客的源代码，以便有直观的认识。

网站截图：


GitHub Pages 的  主页  提供了一个简单的入门指引，阅读并 操作一下，会有一个直观简单的认识。

阮一峰的文章《 搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门 》是使用 GitHub 和 Jekyll 搭建独立博客非常好的入门文章， 强烈建议先阅读并操作一遍 。

在学习完阮一峰同学的文章后，你就已经有能力搭建自己的独立博客了，但是这个博客 只有最基本的功能，并且也不好看。这时候，你面临几个选择:

完全自己定制博客
找一份框架，修改后使用
从GitHub上fork别人的博客代码，在其中添加自己的文章
如果选择 2, 那么  jekyll-bootstrap 是一个选择。 如果选择 3, 那么自己Google一下  github.io 博客  能找到不少博客,去fork,然后修改一下就好。 如果选择 1, 那么可以好好看看后文的内容。

机制一 简单地说，你在 GitHub 上有一个账号，名为 username (任意)， 有一个项目，名为  username.github.io (固定格式，username与账号名一致)， 项目分支名为  master (固定)，这个分支有着类似下面的 目录结构:
.
├── index.html
├── _config.yml
├── assets
│   ├── blog-images
│   ├── css
│   ├── fonts
│   ├── images
│   └── javascripts
├── _includes
├── _layouts
├── _plugins
├── _posts
└── _site
这样，当你访问  http://username.github.io/ 时，GitHub 会使用 Jekyll 解析 用户  username 名下的 username.github.io 项目中，分支为 master  的源代码，为你构建一个静态网站，并将生成的  index.html  展示给你。

关于这个目录更多的内容，我们还不需要关心，如果你好奇心比较重，可以先看 后文 源代码 一节。

看完上面的解释，你可能会有一些疑问，因为按照上面的说法，一个用户只能有一个 网站，那我有很多项目，每个项目都需要一个项目网站，该怎么办呢？另外，在阮一峰 同学的文章中，特别提到，分支名应该为  gh-pages ，这又是怎么回事呢？

原来，GitHub认为，一个GitHub账号对应一个用户或者一个组织，GitHub会 给这个用户分配一个域名： username.github.io ，当用户访问这个域名时， GitHub会去解析 username 用户下， username.github.io 项目的 master 分支， 这与我们之前的描述一致。 
另外，GitHub还为每个项目提供了域名，例如，你有一个项目名为 blog ， GitHub为这个项目提供的域名为 username.github.io/blog ， 当你访问这个域名时，GitHub会去解析 username 用户下， blog 项目的 gh-pages  分支。

所以，要搭建自己的博客，你可以选择建立名为  username.github.io 的项目， 在 master 分支下存放网站源代码，也可以选择建立名为  blog  的项目，在  gh-pages 分支下存放网站源代码。

GitHub 的 Help 文档中的  User, Organization and Project Pages 对此有 详细的描述。

机制二
Jekyll 提供了插件功能，在网站源代码目录下，有一个名为  _plugins 的目录， 你可以将一些插件放进去，这样，Jekyll在解析网站源代码时，就会运行你的插件， 这样插件是 Ruby 写成的。可以为Jekyll添加功能，例如，Jekyll默认是不提供分类 页面的，你可以写一个插件，根据文章内容生成分类页面。如果没有插件，你只能每 次写文章，添加分类时，为每个分类手动写 HTML 页面。

在本地运行 Jekyll 时，这些插件会自动被调用，但是GitHub在解析网站源代码时， 出于安全考虑，会开启安全模式，禁用这些插件。我们既想用这些插件，又想用 GitHub，怎么办呢怎么办呢？

GitHub还为我们提供了更一种解析网站的方式，那就是直接上传最终的静态网页， 这样，我们可以在本地使用 Jeklly 把网站解析出来，然后再上传到 GitHub上， 这就使得我们既使用了插件，又使用了 GitHub。在上文的目录结构中，有一个 名为  _site  的目录，这个就是Jeklly在本地解析时最终生成的静态网站，我们 把其中的内容上传到 GitHub 的项目中就可以了。例如，我在GitHub上的网站， 既解析后的  _site  目录，大概是这样的:

.

├── index.html
├── 2013
├── 2014
├── assets
├── categories
├── page2
├── page3
├── page4
├── 工具
├── 思想
├── 技术
└── 源代码阅读
其中的  categories ， 2013 ,  2014  目录就是分类插件和归档插件帮助我生成的， 我只要把这个目录下的内容上传到 GitHub 相应的项目分支中就可以了。这样，你 访问  username.github.io 时，GitHub就不解析了，直接把 index.html 返回给你了。

关于 git 和 jekyll 的安装与基本使用，这里就不多说了。

工作流一
如果你不使用插件，那么只需要维护一个分支就好:

- username/username.github.io 的 master 分支
- username/blog 的 gh-pages 分支
其中  username  是你的 GitHub 帐号。

你需要在本地维护一份网站源代码，添加新文章后，使用 jekyll 在本地测试一下， 没有问题后，commit 到 GitHub 上的相应分支中就可以了。

工作流二
如果你需要使用插件，那么需要维护两个分支，一个是网站的源代码分支，另一个 是 Jeklly 解析源代码后生成的静态网站。

例如，我的源代码分支名为  master ，静态网站分支名为  gh-pages 。平时写博客时， 首先在 master 分支下，添加新文章，然后本地使用 jekyll build 将添加文章后的网站 解析一次，这时  _site  目录下就有新网站的静态代码了。然后把这个目录下的所有内容 复制到  gh-pages  分支下。这个过程，可以写一个 Makefile，每次添加文章后 make 一下， 就自动将文章发布到 GitHub 上。

Makefile 内容如下：

deploy:
    git checkout master
    git add -A
    git commit -m "deploy blog"
    cp -r _site/ /tmp/
    git checkout gh-pages
    rm -r ./*
    cp -r /tmp/_site/* ./
    git add -A
    git commit -m "deploy blog"
    git push origin gh-pages
    git checkout master
    echo "deploy succeed"
下面的内容涉及源代码，如果需要进一步学习，或者有问题，可以在  Jeklly 官网 上找到更详细的解释，或者在评论中留言。

再来看一下这个目录结构：

.
├── _config.yml
├── index.html
├── assets
│   ├── blog-images
│   ├── css
│   ├── fonts
│   ├── images
│   └── javascripts
├── _includes
├── _layouts
├── _plugins
├── _posts
└── _site
_config.yml
这是针对 Jekyll 的 配置文件 ， 决定了 Jekyll 如何解析网站的源代码,下面是一个示例：

baseurl: /StrayBirds
markdown: redcarpet
safe: false
pygments: true
excerpt_separator: "\n\n\n"
paginate: 5
我的网站建立在  StrayBirds  项目中，所以  baseurl  设置成  StrayBirds ， 我的文章采用 Markdown 格式写成，可以指定 Markdown 的解析器  redcarpet 。 另外，安全模式需要关闭，以便 Jekyll 解析时会运行插件。  pygments  可以使得Jekyll解析文章中源代码时加入特殊标记，例如指定代码类型， 这可以被很多 javascript 代码高度库使用。  excerpt_separator  指定了一个摘要分割符号，这样 Jekyll 可以在解析文章时， 将文章的提要提取出来。 paginate 指定了一页有几篇文章，页数太多时，我们可以将文章列表分页，我们在 后文还会提到。

_layouts
这个目录存放着一些网页模板文件，为网站所有网页提供一个基本模板，这样 每个网页只需要关心自己的内容就好，其它的都由模板决定。例如，这个目录下的 default.html 文件：

{% raw %}
<!DOCTYPE html>
<html>
  <head>
    <meta charset='utf-8'>
    <title>{{ page.title }}</title>
  </head>
  <body>
    <header>
    </header>

    <aside>
    </aside>

    <article>
{{ content }}
    </article>

    <footer>
    </footer>
  </body>
</html>
{% endraw %}
可以看出，这个文件就是所有页面共有的东西，每个页面的具体内容会被填充在  {{ content }}  中，注意这个 content 两边的标记，这是一种叫  liquid  的标记语言。 另外，还有那个  {{ page.title }}  ，其中  page  表示引用  default.html 的 那个页面，这个页面的  title  值会在  page  相应页面中被设置，例如 下面的  index.html  文件，开头部分就设置了  title 值。

index.html
这是网站的首页，访问  http://username.github.io  时，会指向  http://username.github.io/index.html ，我们看一下基本内容：

---
layout: default
title: 首页
---

{% raw %}
<ul class="post-list">
    {% for post in site.posts %}
        <a href="{{site.baseurl}}{{post.url}}"> {{ post.title }}  </a> <br>
        {{ post.date | date: "%F" }} <br>
        {{ post.category }} <br>
        {{ post.excerpt }} 
    {% endfor %}
{% endraw %}
</ul>
注意，文件开头的描述，我们称之为  front-matter ， 是对当前文件的一种描述，这里 设置的变量可以在解析时被引用，例如这里的  layout 就会告诉 Jekyll, 生成  index.html  文件时，去  _layouts  目录下找  default.html  文件，然后把当前文件解析后，添加到  default.html  的  content  部分，组成最终的  index.html  文件。还有 title  设置好的 值，会在  default.html  中通过  page.title  被引用。

文件的主体部分遍历了站点的所有文章，并将他们显示出来，这些语法都是  liquid  语法， 其中的变量，例如  site , 由 Jekyll 设置我们只需要引用就可以了。而  post  中的变量， 如  post.title ,  post.category  是由  post  文件中的 front-matter 决定，后面马上就会看到。

_posts
这个目录存放我们的所有博客文章，他们的名字有统一的格式：

YEAR-MONTH-DAY-title.MARKUP
例如，2014-02-15-github-jeklly.md，这个文件名会被解析，前面的  index.html  中，  post.date  的值就由这里文件名中的日期而来。下面，我们看看一篇文章的内容示例：

---
layout: default
title: 使用 Markdown
category: 工具
comments: true
---

# 为什么使用 Markdown

* 看上去不错  
* 既然看上去不错，为什么不试试呢  


# 如何使用 Markdown
可以看出，文章的 front-matter 部分设置了多项值，以后可以通过类似  post.title ,  post.category  的方式引用这些些，另外， layout 部分的值和之前解释的一样， 文件的内容会被填充到  _layouts/default.html  文件的  content  变量中。

另外，文章中  为什么不试试呢 之后的有三个不可见的  \n ，它决定了这三个  \n  之前的内容会被放在  post.excerpt  变量中，供其它文件使用。

_includes
这个文件中，存放着一些模块文件，例如  categories.ext ，其它文件可以通过

{% raw %}
{% include categories.ext %}
{% endraw %}
来引用这个文件的内容，方便代码模块化和重用。我的博客  主页 上的 分类，归档，这些模块的代码都是通过这种方式引用的。

_plugins
这个文件中存放一些Ruby插件, 例如  gen_categories.rb ，这些文件会在 Jekyll 解析网站源代码时被执行。下一节讲述的就是插件。

_site
Jekyll 解析整个网站源代码后，会将最终的静态网站源代码放在这里

插件使用 Ruby 写成，放在 _plugins 目录下，有些 Jekyll 没有的功能，又不能 手动添加，因为页面的内容会随着文章日期类别的不同而不同，例如分类功能和归档功能， 这时，就需要使用插件自动生成一些页面和目录。

分类 我的分类插件使用的是  jekyll-category-archive-plugin , 它会根据网站文章的分类信息，为每个类别生成一个页面。
使用方法是，把  plugins/category archive_plugin.rb 放在  plugins 目录下， 把 _layouts/category archive.html 放在  layouts 目录下， 这样，这个插件会在Jekyll解析网站时，生成相应categories目录，目录下是各个分类， 每个分类下都有一个  index.html  文件，这个文件是根据模板文件 category archive.html 生成的，例如：

_site/categories/
├── 工具
│   └── index.html
├── 思想
│   └── index.html
├── 技术
│   └── index.html
└── 源代码阅读
    └── index.html
然后，你就可以通过  http://username.github.io/blog/categories/工具/  访问  工具 类下的  index.html  文件。

归档 我的归档插件使用的是  jekyll-monthly-archive-plugin ,它会根据网站 _posts目录下的文章日期，为每个月生成一个页面。
使用方法同上。注意，这个插件在 jekyll-1.4.2 中可能会出错，在 jekyll-1.2.0 中没有错误。

分页
当文章很多时，就需要使用分页功能，在 Jekyll 官网上提供了一种  实现 ，把相应代码放在 主页上，然后在  _config.yml  中设置  paginate  值就行了。

评论
评论功能需要使用外挂，我使用的是  DISQUS , 注册 之后，将评论区的一段代码放在你需要使用评论功能的页面上, 然后，通过在页面的 front-matter 部分使用

comments: true
启用评论。

评论区截图：


基本的内容就介绍到这里，任何问题，欢迎留言。





另外，如果这篇文章对你有用的话，在GitHub上帮我点个 star 吧，即是对我的肯定，也可以帮助更多的人。
另外，注意如果你要 fork 我的模板，注意里面有些链接是与我的 GitHub 名 minixalpha 相关的，在使用前最好批量地将这个字符串替换为你的账号名。

一个极简风格的博客
从上面的工作流程可以看出，虽然每次我在本地添加文件后，都只要 make 一下就能发布文章，但我还是觉得麻烦，希望能直接通过浏览器在 GitHub 的网站上添加文章，所以，我又建立了一个非常简洁的博客，没有分类，没有评论，就是一个主页，上面有所有文章链接，添加文章时候，只要在 _post 目录下添加一个 markdown 文件就可以了。
这个博客项目为： StrayBirds , 是通过 GitHub 的  Automatic page generator生成。完全通过浏览器操作就能建成，不用 git啊，make啊。
博客首页：飞鸟集。



要使用这个项目，你唯一需要做的是：
1. 注册 GitHub，例如你的用户名为 minixbeta
2. 到 StrayBirds 点右上角的 fork
3. 到你 fork 后的项目中，将 _config.yml 中的 username 修改成你的用户名 minixbeta
4. 得到你自己的博客 http://minixbeta.github.io/StrayBirds/
需要注意的是，第一次使用 GitHub Pages 时，可能会有较长时间的缓冲时间，过15min左右，才能正常访问博客，请耐心等待。
如果你想把项目的名字改了，例如，将 StrayBirds 修改为 blog
那么，你需要做的是:
1. 在项目的 Setting 中将 Repository name 从 StrayBirds 修改为 blog
2. 将 _config.yml 中的 baseurl 修改为 /blog
3. 通过 http://minixbeta.github.io/blog/ 来访问你的新博客。
如果你不太明白，可以看这个 StrayBirds  项目在 GitHub 上的 READEME，里面有如何fork项目，修改项目名，添加文章的 GIF 演示。
无论遇到任何问题，欢迎给我留言，或者在　GitHub 上提交Issues. 
