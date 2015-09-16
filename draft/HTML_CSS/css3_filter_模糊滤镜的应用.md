
# CSS3 filter 模糊滤镜的应用
Posted on November 22, 2012	
http://mao.li/css3-blur-filter-pratice/

在segmentfault上回答过的一个问题，如何将网页CSS背景图高斯模糊且全屏显示
当时没有深入了解，只觉得滤镜应该只是应用于图片上的。而且各大网站的demo也清一色的图片加滤镜效果。

直到不久前进入nodejitsu的介绍页面，点击登录按钮之后出现弹窗，看到mask下面的模糊效果。
![image](http://mao.li/wp-content/uploads/2012/11/Web-Operations-Nodejitsu-Inc..png)
这不科学呀，心理活动如下:
这肯定是canvas实现的=>不对，难道现在不借助chrome extension接口就可以截取可见区域的图像数据=>那就是预先处理好模糊后的图片在弹窗时出现=>不对，国外工程师没这么蛋疼=>莫非是CSS3效果，而且是我不了解的=>打开源码，mask层没什么特殊的css3属性呀=>倒是body上有个
1	body {
2	-webkit-backface-visibility: hidden;
3	}

=>而且比正常状态下多了modal-active属性，可惜该类在body上没有style，再往后面查，最后发现了magic code！

1	.modal-active .row {
2	-webkit-filter: blur(3px);
3	-moz-filter: blur(3px);
4	-o-filter: blur(3px);
5	-ms-filter: blur(3px);
6	filter: blur(3px);
7	}

这里不讲 CSS3 滤镜的基本资料，可以参考：

    http://www.qianduan.net/what-is-webkit-filter.html
    https://dvcs.w3.org/hg/FXTF/raw-file/tip/filters/index.html

废话太多了，说明一些问题：
为什么不加在body上

你已经想到了吧，直接加到body上就会出现下面的样子。效果很逆天，可惜连表单也成天书了。当然也不能加到遮罩上，因为内容区域不在遮罩里。
![image](http://mao.li/wp-content/uploads/2012/11/Web-Operations-Nodejitsu-Inc.1.png)
应该怎么处理

为每个大的分割区域加个共同的类名，如.row,最后在body上统一加上blur样式。实际按照一般情况可能就是header,wrap和footer了。
当然也可以#header,#wrap,#footer {/* code */}
性能问题

看这篇文章 CSS filter effects get GPU-accelerated in Chrome

    The feature first landed in WebKit last year and has been making its way into Web browsers. The feature was supported out of the box in Chrome 19, which was released last month, but it’s about to get a whole lot better. In recent Chromium builds, the filter effects are now offloaded to the GPU. This support for hardware-accelerated rendering will boost the performance of filter effects, making it practical to use in a wider range of scenarios.

    Google highlighted the GPU support in an entry posted this week in the official Chromium blog. According to Google software engineer Stephen White, the performance of CSS filter effects in Chrome has been elevated to the point where it can now be used efficiently with animations or applied to an HTML5 video element. 

英文水平不高就不翻译了，主要是说启用硬件加速后即使对视频(HTML5 video)使用滤镜也能流畅观看了。实际上比较疑惑的问题是css3属性如box-shadow,border-radius等等使用多了会不会有性能的问题？留待研究。
-webkit-backface-visibility 的作用

可以看这里：

    http://www.never-online.net/blog/article.asp?id=323
    http://stackoverflow.com/questions/3461441/prevent-flicker-on-webkit-transition-of-webkit-transform

    http://stackoverflow.com/questions/2946748/iphone-webkit-css-animations-cause-flicker

解释一下，在用position:absulote+zindex!=0时用transform会偶尔出现页面会闪的现象，解决方法有三：

    -webkit-backface-visibility: hidden;
    -webkit-perspective: 1000;
    该方法对于sprite图无效。

    body {-webkit-transform:translate3d(0,0,0);}

    .no-flick{-webkit-transform:translate3d(0,0,0);}

比较多使用的解决方法是 -webkit-backface-visibility: hidden;

关于-webkit-backface-visibility有兴趣查看官方文档和上面的链接。
例子

刚好今天看到一个叫一搜的网站，看下前后的对比，效果还是不错的。

结论

滤镜效果不只能用于图片上，普通的元素包括DOM和视频(HTML5 video)也能使用。目前只有Chrome 18.0.976.0 (currently canary), Webkit nightly支持该属性。作为渐进增强还是可以使用的，和webkit speech一样，代码只有几句，何乐而不为？
其他

nodejitsu网站值得学习的还有很多，遮罩的fadeIn和弹窗的bouncedown效果都是使用css3的animation来实现的，当然也没有看到jQuery这个大块头。看他们网站的css可以找到不少值得学习的地方。
