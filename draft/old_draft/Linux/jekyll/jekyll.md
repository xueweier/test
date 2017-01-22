# jeykll

###### website:
http://jekyllrb.com/docs/variables/  
http://jekyllrb.com/docs/permalinks/
http://blog.csdn.net/on_1y/article/details/19259435

官网的文档讲的非常的详细到位。在这里就点一下要点好了。

# 查看官方文档

### Configuration

Jekyll allows you to concoct your sites in any way you can dream up, and it’s thanks to the powerful and flexible configuration options that this is possible. These options can either be specified in a _config.yml file placed in your site’s root directory, or can be specified as flags for the jekyll executable in the terminal.

文章采用 Markdown 格式写成，可以指定 Markdown 的解析器 redcarpet。 另外，安全模式需要关闭，以便 Jekyll 解析时会运行插件。 pygments 可以使得Jekyll解析文章中源代码时加入特殊标记，例如指定代码类型， 这可以被很多 javascript 代码高度库使用。 excerpt_separator 指定了一个摘要分割符号，这样 Jekyll 可以在解析文章时， 将文章的提要提取出来。 paginate 指定了一页有几篇文章，页数太多时，我们可以将文章列表分页，我们在 后文还会提到。

### [Front Matter](http://jekyllrb.com/docs/frontmatter/)

###### Predefined Global VariablesPermalink

There are a number of predefined global variables that you can set in the front matter of a page or post.

VARIABLE|DESCRIPTION
-|-
layout|If set, this specifies the layout file to use. Use the layout file name without the file extension. Layout files must be placed in the _layouts directory.
permalink|If you need your processed blog post URLs to be something other than the default /year/month/day/title.html then you can set this variable and it will be used as the final URL.
published|Set to false if you don’t want a specific post to show up when the site is generated.
category/categories|Instead of placing posts inside of folders, you can specify one or more categories that the post belongs to. When the site is generated the post will act as though it had been set with these categories normally. Categories (plural key) can be specified as a YAML list or a space-separated string.
tags|Similar to categories, one or multiple tags can be added to a post. Also like categories, tags can be specified as a YAML list or a space- separated string.

###### Custom Variables

Any variables in the front matter that are not predefined are mixed into the data that is sent to the Liquid templating engine during the conversion. For instance, if you set a title, you can use that in your layout to set the page title:

	<!DOCTYPE HTML>
	<html>
	  <head>
	    <title>{{ page.title }}</title>
	  </head>
	  <body>
	    ...
	    
	    
### [Writing posts](http://jekyllrb.com/docs/posts/)

###### Content FormatsPermalink

All blog post files must begin with YAML Front Matter. After that, it’s simply a matter of deciding which format you prefer. Jekyll supports Markdown out of the box, and has myriad extensions for other formats as well, including the popular Textile format. These formats each have their own way of marking up different types of content within a post, so you should familiarize yourself with these formats and decide which one best suits your needs.


###### Including images and resourcesPermalink

Chances are, at some point, you’ll want to include images, downloads, or other digital assets along with your text content. While the syntax for linking to these resources differs between Markdown and Textile, the problem of working out where to store these files in your site is something everyone will face.

Because of Jekyll’s flexibility, there are many solutions to how to do this. One common solution is to create a folder in the root of the project directory called something like assets or downloads, into which any images, downloads or other resources are placed. Then, from within any post, they can be linked to using the site’s root as the path for the asset to include. Again, this will depend on the way your site’s (sub)domain and path are configured, but here some examples (in Markdown) of how you could do this using the site.url variable in a post.

Including an image asset in a post:

… which is shown in the screenshot below:
![My helpful screenshot]({{ site.url }}/assets/screenshot.jpg)

Linking to a PDF for readers to download:

… you can [get the PDF]({{ site.url }}/assets/mydoc.pdf) directly.

	ProTip™: Link using just the site root URL
	
	You can skip the {{ site.url }} variable if you know your site will only ever be displayed at the root URL of your domain. In this case you can reference assets direct		ly with just /path/file.jpg.


###### Post excerptsPermalink

Each post automatically takes the first block of text, from the beginning of the content to the first occurrence of excerpt_separator, and sets it as the post.excerpt. Take the above example of an index of posts. Perhaps you want to include a little hint about the post’s content by adding the first paragraph of each of your posts:

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      {{ post.excerpt }}
    </li>
  {% endfor %}
</ul>

Because Jekyll grabs the first paragraph you will not need to wrap the excerpt in p tags, which is already done for you. These tags can be removed with the following if you’d prefer:

{{ post.excerpt | remove: '<p>' | remove: '</p>' }}

If you don’t like the automatically-generated post excerpt, it can be overridden by adding excerpt to your post’s YAML Front Matter. Completely disable it by setting your excerpt_separator to "".

Also, as with any output generated by Liquid tags, you can pass the | strip_html flag to remove any html tags in the output. This is particularly helpful if you wish to output a post excerpt as a meta="description" tag within the post head, or anywhere else having html tags along with the content is not desirable.

Additionally you are able to specify per-post excerpt_separator value if it is required just only the the selected post. Just specify the excerpt_separator with the same way as excerpt in the post’s YAML head:

---
excerpt_separator: <!--more-->
---

Excerpt
<!--more-->
Out-of-excerpt

###### Highlighting code snippetsPermalink

Jekyll also has built-in support for syntax highlighting of code snippets using either Pygments or Rouge, and including a code snippet in any post is easy. Just use the dedicated Liquid tag as follows:

{% highlight ruby %}
def show
  @widget = Widget(params[:id])
  respond_to do |format|
    format.html # show.html.erb
    format.json { render json: @widget }
  end
end
{% endhighlight %}


