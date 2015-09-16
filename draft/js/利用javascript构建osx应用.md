# 利用JavaScript构建OSX应用
BenCHouBenCHou 48 1月22日 发布
推荐 3 推荐
收藏 5 收藏，161 浏览
转：[译]利用js构建osx应用
英文原文地址

OSX Yosemite引入了js来创建Automation,这使得javascript可以访问nativeOSX类库,我已经深入研究这块并且编写了一些examples,今天这篇文章会讲解一些基础东西并且一步步的来创建一个小的example app.

WWDC 2014上面有一个JavaScript for Automation主题,专门解释用javascript来代替applescript创建自动化应用程序,这是个非常激动的事情,使用applescript来构建自动化任务已经存在很长时间了,它的一些语法一直都不是很受欢迎.

在这个主题上,主持人讲解了object-c bridge,这个是非常酷的东西,可以往javascript中导入任何底层的方法,例如,如用你想用标准的OS X控件来创建gui应用的话,你将要引入cocoa:

ObjC.import('Cocoa');
Foundation框架就跟它名字一样,提供一些基础模块给osx app,它包含很多类以及接口,比如NSArray,NSURL,NSUserNotification等等,也许你对这些类不熟悉,但是根据字面意思大概就能知道它们的功能,这些类比较常用,所以可以直接在app中使用而不需要单独导入它.它是默认就会加载的

我可以这么跟你说,只要是用objective-c或者swift创建的app，用javascript都可以做出来.

build and example app

注意:你的操作系统要在yosemite开发版7+,下面的例子才可以正常运行
了解它的最好方法就是实践,下面我们将要实现一个简单的app(从你电脑上选取一个图片并显示它),效果如图

clipboard.png

制定一个app.js,包括一个窗口,一个文本标签,一个输入框和一个按钮,对应的class名称就是NSWindow,NSTextField,NSTextField和NSButton.

单击选择图片文件按钮,将会显示一个NSOpenPanel,它将会显示一个文件选择窗口,我们还需要配置这个窗口的文件过滤属性,只能选择.jpg,.png或者.gif

当选择一个图片之后,将会在窗口中显示出来,并且自适应图片大小,窗口设置了一个最小的宽高以免图片被裁剪

创建一个project

打开Apple Script Editor应用程序,定位到Applications > Utilities,Script Editor并不是最好用的编辑器,但是现在有必要用它,它提供了很多的特性用来创建JS OSX APP,并且可以用来编译和运行你的js osx app,它也可以添加些扩展文件像我们app需要的info.plist文件.我猜也许还有别的编辑器可以做同样的事情,不过目前我还没有找到.

创建一个新的文档,定位到File->New或者使用命令cmd + n,首先要做的就是保存它为Application,定位到File->Save或者使用命令cmd + s,确认保存之前,有两个选项需要注意下,看下图:

clipboard.png

文件格式选择Application,选中Stay open after run handler

如果没有选中Stay open after run handler,打开应用之后会一闪而过,然后自动关闭,这些网上都没有什么教程,只是自己几个小时摸索出来的.

现在应该是做些有意义的时候了
添加下面两行代码到你的编辑器中,然后运行它,定位到Script->Run Application或者opt + cmd + r.

Objc.import('Cocoa');
$.NSLog('hello feenan!');
这时候运行应用什么都没有发生,唯一看到改变的就是全局菜单栏以及dock,因为应用名称以及file,edit并排在菜单栏上,应用图片显示在dock上，这些都代表着应用已经在运行.

那么hello feenan!跑哪去了呢?$符号是什么,是jquery?,我们先把应用程序退出了,定位到File->Quit或者cmd + q,然后我们来找找NSLog输出的内容.

打开Console app,定位到Applications > Utilities > Console,任何一个应用程序都可以记录日志到console app中,它跟chrome,firefox,safari中的控制台没多大区别,主要的区别在于你可以使用它来调试应用程序代替website

在console app中有很多日志信息,你可以在右上角的输入框中输入applet来过滤日志,输入applet到过滤框中之后,回到Script Editor中，再次运行应用程序,使用opt + cmd + r命令,控制台信息如下图

clipboard.png

你是不是看到在控制台中显示hello feenan!了,如果没有的话,退出应用程序再次运行看看,有时候我们忘记退出应用程序,代码并没有再次运行.

$符号是什么呢?

$让你能够访问Objective-C bridge.任何时候你需要访问Objective-C其中某个类或者常量,你都可能使用$.foo或者ObjC.foo,后面还有讲到关于使用$的其它一些方法.

Console app和NSLog是必不可少的工具,你将会不停的使用它们来调试你的应用程序，想要了解更多的信息，可以点击NSLog example

创建一个窗口

让我们创建一个可以显示并且有交互的窗口，代码看起来像下面这样

ObjC.import("Cocoa");

var styleMask = $.NSTitledWindowMask | $.NSClosableWindowMask | $.NSMiniaturizableWindowMask;
var windowHeight = 85;
var windowWidth = 600;
var ctrlsHeight = 80;
var minWidth = 400;
var minHeight = 340;
var window = $.NSWindow.alloc.initWithContentRectStyleMaskBackingDefer(
  $.NSMakeRect(0, 0, windowWidth, windowHeight),
  styleMask,
  $.NSBackingStoreBuffered,
  false
);

window.center;
window.title = "Choose and Display Image";
window.makeKeyAndOrderFront(window);
当这些都在合适的位置之后,我们运行它通过opt + cmd + r,然后我们可以说,只需要这么点代码就可以启动一个app并且打开一个窗口,而且我们可以移动,最小化和关闭它.

clipboard.png

如果你跟我一样没有用Objective-C或者Cocoa来创建一个app的话，这些可以看起来有点难以理解,对我来说,这些方法名称的长度有点难以接受,虽然我喜欢描述性的方法名称,但是像Cocoa这样的还是太极端了.

看上面的代码其实就是javascript,就跟你编写网站代码一样.

第一行中的styleMask是干么的呢?它提供了一些对窗口属性设置的功能,有标题,关闭按钮,最小化按钮,这些选项都是常量,通过|符号来添加多个,|符号是C里的or操作符,不用去理解它的原理,只要知道它是用来并列多个选项的就足够了.

这里有很多种样式信息,想了解详情的可以点击the docs,NSResizableWindowMask将是接下来要使用的一个样式,可以添加到上面的代码中看看效果

这里还有一些有趣的语法需要你记住,$.NSWindow.alloc调用NSWindow里的alloc方法,但是并没有在后面插入(),这种调用方式就跟js里获取属性一样,但是js里调用方法不是这样的,原来,在OSX JS中,假如方法没有参数的时候,是不能在后面加入()的,否则它会出现运行时错误.所以以后当你发现有些事件并没有像你期望中发生时就要检查下console里的输出内容了.

下一个要关注的事情是这个超长的方法:

initWithContentRectStyleMaskBackingDefer

让我们来看看NSWindow的doc上讲解这个方法的,你会发现有一点不同:

initWithContentRect:styleMask:backing:defer:

上面的描述相当于在Objective-c中创建下面的代码

NSWindow* window [[NSWindow alloc]
  initWithContentRect: NSMakeRect(0, 0, windowWidth, windowHeight)
  styleMask: styleMask,
  backing: NSBackingStoreBuffered
  defer: NO];
注意下上面方法签名中的:,当你想把一个Objective-c中的方法转换成js的方法时,首先需要把:除掉然后把跟在后面的第一个字母大写.当看到方括号[]里有两项是,代表调用一个类或者对象的方法,NSWindow alloc表示调用NSWindow的alloc方法,换成js的话,需要在两者之前添加一个.,像NSWindow.alloc这样

我认为剩下的代码足够用来描述创建一个窗口并显示它,我跳过了很多关于这方面的细节说明,这个需要很多时间用来阅读相关文档,不过你可以这些.当你做到显示出一个窗口来已经不错了,让我们做更多的事情吧

添加控件

窗口里还需要一个标签,一个输入框,一个按钮,我使用NSTextField和NSButton来创建它,输入下面的代码然后运行你的app

ObjC.import("Cocoa");

var styleMask = $.NSTitledWindowMask | $.NSClosableWindowMask | $.NSMiniaturizableWindowMask;
var windowHeight = 85;
var windowWidth = 600;
var ctrlsHeight = 80;
var minWidth = 400;
var minHeight = 340;
var window = $.NSWindow.alloc.initWithContentRectStyleMaskBackingDefer(
  $.NSMakeRect(0, 0, windowWidth, windowHeight),
  styleMask,
  $.NSBackingStoreBuffered,
  false
);

var textFieldLabel = $.NSTextField.alloc.initWithFrame($.NSMakeRect(25, (windowHeight - 40), 200, 24));
textFieldLabel.stringValue = "Image: (jpg, png, or gif)";
textFieldLabel.drawsBackground = false;
textFieldLabel.editable = false;
textFieldLabel.bezeled = false;
textFieldLabel.selectable = true;

var textField = $.NSTextField.alloc.initWithFrame($.NSMakeRect(25, (windowHeight - 60), 205, 24));
textField.editable = false;

var btn = $.NSButton.alloc.initWithFrame($.NSMakeRect(230, (windowHeight - 62), 150, 25));
btn.title = "Choose an Image...";
btn.bezelStyle = $.NSRoundedBezelStyle;
btn.buttonType = $.NSMomentaryLightButton;

window.contentView.addSubview(textFieldLabel);
window.contentView.addSubview(textField);
window.contentView.addSubview(btn);

window.center;
window.title = "Choose and Display Image";
window.makeKeyAndOrderFront(window);
如果应用跑起来没问题的话,将会看到下面的效果,你可以在输入框中输入内容,按钮点击没有任何效果,不过我们后面会加些东西上去



看到上面的代码是不是对添加有些疑惑,到底怎么实现的呢?textFieldLabel和textField非常相似,它们都是NSTextField的实例,在这里都是通过相似的方法实现的,当你看到initWithFrame和NSMakeRect时,它们是创建ui元素的好方法,NSMakeRect就跟它的名字一样,用来创建有一定大小的方形用给定的位置信息(x, y, width, height).这就相当于创建了Objective-c中的结构体信息,在js中我们引用它为一个对象或者hash,或者一个dict,拥有自己的键值对.

创建完输入框这后,给它赋一些属性,Cocoa并没有单独创建标签的方法,所以这里我们利用NSTextField,禁用它的编辑属性并且设置的背景样式来模拟标签效果.

假如是正常的输入框的话,其实只需要设置一行代码就可以了

对于按钮我们使用NSButton类,跟输入框一样,创建它也需要先画一个矩形,不过这里有两个属性需要强调下:bezelStyle和buttonType.它们的值就是常量,主要用来控制按钮的渲染以及有什么样式信息,想了解更多的信息,可以点击docs,我也有一些关于不同样式的按钮例子,example app

最后的事情就是通过addSubview把这些控件添加到我们的window中去,刚开始我调用window.addSubview(theView),但是并没添加起来,最后发现只能添加其它用NSView创建的实例,我不知道为什么会这样,但是想要添加NSWindow创建的实例的话,只能调用window.contentView.addSubview(theView).官方文档上是这样描述的,NSView对象是窗口里最高的访问层次.

给按钮添加代码

当点击按钮的时候,我想显示一个面板出来,上面列出本地的文件信息,做这些之前,让我们先添加一个日志信息热热身.

在javascript中添加事件一般是给对象添加监听,但是在Objective-C中没有这样的概念,在它这里叫消息通讯,你得发送一个包含方法名的消息给目标对象,目标对象收到这个包含方法名的消息之后才能决定做什么,也许我说的不是很准确,但是大概就是这个意思

首先我们要做的就是添加一个target和一个action,target是发送给action的一个对象,也许现在还没什么意义,但是后面我会增加更多的代码,先增加下面的一部分代码,用来更新按钮属性的:

...
btn.target = appDelegate;
btn.action = "btnClickHandler";
...
appDelegate和btnClickHandler还没存在,所以要先创建它们,在下面的代码有提供,然后代码中还增加了注释用来告诉你把这些新的代码添加到何处

ObjC.import("Cocoa");

// New stuff
ObjC.registerSubclass({
  name: "AppDelegate",
  methods: {
    "btnClickHandler": {
      types: ["void", ["id"]],
      implementation: function (sender) {
        $.NSLog("Clicked!");
      }
    }
  }
});

var appDelegate = $.AppDelegate.alloc.init;
// end of new stuff

// Below here is in place already
var textFieldLabel = $.NSTextField.alloc.initWithFrame($.NSMakeRect(25, (windowHeight - 40), 200, 24));
textFieldLabel.stringValue = "Image: (jpg, png, or gif)";
...
运行app,然后在console中查看是否有显示Clicked!,如果显示了,说明代码没问题,否则检查下代码跟文中是否有区别,然后仔细看下console中的错误信息.

Subclassing(子类)

ObjC.registerSubclass是干什么的呢?Subclassing是创建一个子类的方法,它可以继承一个父类.也许这个名称叫的不专业,还请多多包含.registerSubclass需要一个参数,它是一个对象,成员可以包含name,superclass,protocols,properties,methods,我不敢保证这里列出了所有的成员,不过你可以看release notes.

一切看起来挺不错的,但是上面的代码做了什么呢?因为并没有写superclass属性,所以默认继承NSObject,它是所有Objective-c里的基类, 设置name属性方便后面我们用$或者Objc来引用它.

$.AppDelegate.alloc.init会创建一个AppDelegate类实例,需要再次注意的是,alloc和init后面并没有插入(),因为我们并没有传任何参数.

Subclass methods(子类方法)

可以通过一个字符串内容来创建一个方法,比如上面的btnClickHandler,然后给它提供一个对象参数,成员包括types和implementation,官方文档上并没有说明types数组应该包括什么,但是经过我不断尝试感觉它的参数说明应该是这样的:

["return type", ["arg 1 type", "arg 2 type",...]]
btnClickHandler没返回任务东西所以设置return type为void,它需要一个参数,就是发送的对象,所以这里设置参数名为id,它可以表示任何对象.

想查看整个类型的列表信息,可以点击release notes

implementation是一个普通的函数,在这里面可以写javascript,同时可以访问$符号以及外面定义的变量.

使用protocols的问题

在子类中可以实现Cocoa protocols,但是我发现在你脚本中使用protocols数组,应用程序会停止而且没有任何错误,我写了一些example and explanation来说明这些问题,有兴趣的可以看一看.

Choosing and displaying images(选择并显示图片)

我们准备打开一个面板,选择一个图片,然后显示它,更新btnClickHandler的implementation函数,代码如下

...
implementation: function (sender) {
  var panel = $.NSOpenPanel.openPanel;
  panel.title = "Choose an Image";

  var allowedTypes = ["jpg", "png", "gif"];
  // NOTE: We bridge the JS array to an NSArray here.
  panel.allowedFileTypes = $(allowedTypes);

  if (panel.runModal == $.NSOKButton) {
    // NOTE: panel.URLs is an NSArray not a JS array
    var imagePath = panel.URLs.objectAtIndex(0).path;
    textField.stringValue = imagePath;

    var img = $.NSImage.alloc.initByReferencingFile(imagePath);
    var imgView = $.NSImageView.alloc.initWithFrame(
    $.NSMakeRect(0, windowHeight, img.size.width, img.size.height));

    window.setFrameDisplay(
      $.NSMakeRect(
        0, 0,
        (img.size.width > minWidth) ? img.size.width : minWidth,
        ((img.size.height > minHeight) ? img.size.height : minHeight) + ctrlsHeight
      ),
      true
    );

    imgView.setImage(img);
    window.contentView.addSubview(imgView);
    window.center;
  }
}
首先我们创建NSOpenPanel一个实例,如果你从来没有打开过文件或者保存操作,那么默认显示的面板是活动文件列表.

我只想打开图片文件,所以需要过滤文件列表,设置allowedFileTypes属性,它是一个NSArray类型,我们创建一个js数组allowedTypes来给它赋值,但是我们需要转换成NSArray类型,通过$(allowedTypes),这是bridge桥的另外一种用法,可以通过这个方法在js和Objective-c之间进行类型转换,想转换Objective-c类型为js中对应的类型的话,使用$(ObjCThing).js方法.

打开面板通过panel.runModal方法,这个代码会马上执行,你可以点击取消和确定,然后我们可以通过返回值来判断你点击是哪个,当点击确定返回的是$.NSOKButton.

另一个需要注意的是panel.URLs,通常我们访问js中的数组元素是通过[],因为URLS是NSArray类型,所以不能通过[]来访问,这里提供了objectAtIndex方法来访问,它跟[]的效果是一样的.

只要我们获取到图片的URL,那么我们就可以创建一个NSImage实例,这里有一个专门的方法

initByReferencingFile
跟创建其它ui元素一样, 这里我们创建一个NSImageView实例来显示图片

我们想窗口的大小能够自适应图片的大小,但是不能低于最小宽高,为了设置窗口的大小,这里调用setFrameDisplay方法

我们通过图像视图来包装图片,然后把它添加到window视图中,因为窗口大小改变了,所以需要重新计算居中显示.

Tidbits(花絮)

迄今为止,我们已经在Script Editor中创建了一个app并用命令opt + cmd + d运行它,也可以像其它app一样,双击它的应用图标来运行.

clipboard.png

你也可以修改app图标的路径,替换/Contents/Resources/applet.icns就行,想访问应用的资源文件,可以右键app选择Show Package Contents就行.

WHY I’M EXCITED(为什么我这么激动?)

我对它的潜能感到非常激动,下面是我已经想到的一点观点.当Yosemite正式发布之后,很多人可以坐下来开发原生app了,使用最普通的编程语言,而且不用下载或者安装别的东西,假如你想的话连xcode也可以不用安装,完全降低了进入app开发的门槛,这简直太疯狂了.

我知道有很多大型应用程序通过脚本是办不到的,我也没有说脚本是创建app的唯一方式,但是我们可以让一些人创建一些小的app为他自己或者别人.当一个团队整天在命令行下工作的不舒服时,我们可以为它们创建一个gui程序;当需要快速,可视化的创建或者修改配置文件时,也可以为它创建一个小的app.

当然也有其它编程语言可以办到,像python和ruby也可以访问到低层api,然后创建app.只是利用javascript来创建app显的更与众不同,这简直有点颠覆我们的思想.这感觉就像一些网站敲响了桌面上的门,apple让这个门解锁了,我完全被它吸引了.