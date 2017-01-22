看样子，你还没有自己的知识体系，谈一下我的建议。

学基础，这里的基础是指Cocoa的构建基础，比如响应链的构成，控件的继承关系(特别是协议的实现)，视图的层级结构，layer的层级结构等；你所要应用的知识是属于哪个框架，这个你要弄清楚；再往深一点，你可以了解设计模式在Cocoa中的应用，Runtime，Runloop等。

你要学习的ios开发知识，官方文档远远足矣(苹果的官方文档相当不错的)，很多人都不读，碰到问题就google，stackoverflow，解决的都是小的知识点，难以提高；学技术就像读书一样吧，你可以从开发语言和系统框架入手，然后选择某个点多花时间学习，就是所谓的精读和泛读结合；阅读优秀的第三方库也是很重要的，难以掌握，说明积累的不够。
		          Reply    2
WildCat   138 天前
@ylkk925 
感谢。

不想读官方文档的原因在于不知道以什么顺序和速度去读，举个例子：
https://developer.apple.com/library/ios/navigation/#section=Frameworks&topic=UIKit
UIKit的文档，到底从哪里开始呢？
		隐藏       感谢回复者   Reply    3
chisj   138 天前   ♥ 2
请参看巧哥的博客： http://blog.devtang.com/blog/2014/07/27/ios-levelup-tips/
iOS开发如何提高。
其实我觉得任何技术都应该差不多，那就是潜心研究，多花时间多踩坑，有一定的积累量才会带来质的进步。CocoaTouch也一样的，甚至还更方便，因为现在资料很多，很全面。
		          Reply    4
hoogle   138 天前   ♥ 1
一般这样的效果自己实现。。 就是几个button，和根据scrollView的offsetX变化位置的view。。
		          Reply    5
railgun   138 天前   ♥ 3
@WildCat 先看两个指南：
View Programming Guide For iOS
View Controller Programming Guide For iOS
看苹果的文档不要从参考(Reference)开始看，从指南(Guide)开始看。
一般你看到一个具体类的时候，如果有相关的指南都会有链接直接跳过去的
		          Reply    6
ylkk925   138 天前   ♥ 2
@WildCat

5楼的方法很好，读Guide；碰到比较重要，但文档说明不够的知识点，基本上可以找到相应的书籍和博客来补充。
