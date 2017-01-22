12个你未必知道的CSS小知识
BenCHouBenCHou 48 5天前 发布
推荐 3 推荐
收藏 11 收藏，539 浏览
虽然CSS并不是一种很复杂的技术，但就算你是一个使用CSS多年的高手，仍然会有很多CSS用法/属性/属性值你从来没使用过，甚至从来没听说过。

1.CSS的color属性并非只能用于文本显示

对于CSS的color属性，相信所有Web开发人员都使用过。如果你并不是一个特别有经验的程序员，我相信你未必知道color属性除了能用在文本显示，还可以用作其它地方。

你可以先看一下下面的演示：

HTML代码

<img alt="Example alt text" width="200" height="200">
<ul>
  <li>Example list item</li>
</ul>
<ol>
  <li>Example list item</li>
</ol>
<hr>
CSS代码

body {
  color: yellow;
  background: #444;
  font-size: 20px;
  font-family: Arial, sans-serif;
  text-align: center;
}

ul {
  border: solid 2px;
  text-align: left;
}

ol {
  text-align: left;
}

hr {
  border-color: inherit;
}
http://jsfiddle.net/c0501uL5/3/embedded/result/点击预览

请注意，上面的代码里只使用了一个color属性，就是在body元素上，设置成了yellow。但是，你也看到了，所有这个页面上的东西都变成了黄色，包括：

无法显示的图片的alt文字
list元素的边框
无序list元素前面的小点
有序list元素前面的数字
还有hr元素
有趣的是，这个hr元素，缺省情况下它并不从body上继承color的属性，但我使用border-color: inherit强制让它继承。这是个很诡异的特征。

在CSS规范里是这样说的：

这个属性声明了元素文本内容的前景色(foreground color)。除此之外，它的值还被用于其它地方间接的引用….比如，其它可以接受颜色值的属性。
我无法想象出还有什么地方的属性能用“前景色”来描述，如果你知道，请在评论里告诉我。

2.CSS里的visibility属性有个collapse属性值：collapse

对于CSS里的visibility属性，相信你用过不下几百次。大多时候，你会把它的值设置成visible(这是所有页面元素的缺省值)，或者是hidden。后者相当于display: none，但仍然占用页面空间。

其实visibility可以有第三种值，就是collapse。当一个元素的visibility属性被设置成collapse值后，对于一般的元素，它的表现跟hidden是一样的。但例外的是，如果这个元素是table相关的元素，例如table行，table group，table列，table column group，它的表现却跟display: none一样，也就是说，它们占用的空间也会释放。

但遗憾的是，各种浏览器对collapse值的处理方式不一样。看一下下面的演示：

HTML代码

<table cellspacing="0" class="table">
  <tr>
    <th>Fruits</th>
    <th>Vegetables</th>
    <th>Rocks</th>
  </tr>
  <tr>
    <td>Apple</td>
    <td>Celery</td>
    <td>Granite</td>
  </tr>
  <tr>
    <td>Orange</td>
    <td>Cabbage</td>
    <td>Flint</td>
  </tr>
</table>
<p><button>collapse行1</button></p>
<p><button>hide行1</button></p>
<p><button>重置</button></p>
CSS代码

body {
  text-align: center;
  padding-top: 20px;
  font-family: Arial, sans-serif;
}

table {
  border-collapse: separate;
  border-spacing: 5px;
  border: solid 1px black;
  width: 500px;
  margin: 0 auto;
}

th, td {
  text-align: center;
  border: solid 1px black;
  padding: 10px;
}

.vc {
  visibility: collapse;
}

.vh {
  visibility: hidden;
}

button {
  margin-top: 5px;
}
Javascript代码

var btns = document.getElementsByTagName('button'),
    rows = document.getElementsByTagName('tr');

btns[0].addEventListener('click', function () {
  rows[1].className = 'vc';
}, false);

btns[1].addEventListener('click', function () {
  rows[1].className = 'vh';
}, false);

btns[2].addEventListener('click', function () {
  rows[1].className = '';
}, false);

演示

http://jsfiddle.net/c0501uL5/4/embedded/result/点击预览

CSS-Tricks的Almanac建议说不要使用这个值，因为浏览器的不统一。

据我的观察：

在谷歌浏览器里，使用collapse值和使用hidden值没有什么区别。 (See this bug report and comments)
在火狐浏览器、Opera和IE11里，使用collapse值的效果就如它的字面意思：table的行会消失，它的下面一行会补充它的位置。
说实话，估计这个值很少人会使用它，但你要知道确实可以使用这样的一个值，如果以前不知道它，那么，现在，在有些罕见的地方，你也许就会变得聪明一点了。

3.CSS的background简写方式里新增了新的属性值

在CSS2.1里，background属性的简写方式包含五种属性值 – background-color, background-image, background-repeat, background-attachment, and background-position。从CSS3开始，又增加了3个新的属性值，加起来一共8个。下面是按顺序分别代表的意思：

background: [background-color] [background-image] [background-repeat]
            [background-attachment] [background-position] / [ background-size]
            [background-origin] [background-clip];
注意里面的反斜杠，它更font和border-radius里简写方式使用的反斜杠的用法相似。反斜杠可以在支持这种写法的浏览器里在position后面接着写background-size。

除此之外，你开可以增加另外两个描述它的属性值： background-origin 和 background-clip.

它的语法用起来像下面这个样子：

.example {
  background: aquamarine url(img.png)
              no-repeat
              scroll
              center center / 50%
              content-box content-box;
}
你可以用下面的演示检测你的浏览器是否支持这种写法：

http://jsfiddle.net/c0501uL5/5/embedded/result/点击预览

关于浏览器的支持情况，大概所有的现代浏览器都支持这些新属性值，但对于一些非常老旧的浏览器(IE6/7/8)，最好在代码里提供一些万一不支持的补救机制。

4.CSS的clip属性只在绝对定位的元素上才会生效

之前说到了background-clip，你可能会想到clip属性。它的用法是下面这个样子：

.example {
    clip: rect(110px, 160px, 170px, 60px);
}
它的作用是按指定的尺寸、规定的大小裁剪元素。很多简单，但唯一你需要注意的事情是，clip只会在绝对定位的元素上生效。所有，你必须这样做：

.example {
    position: absolute;
    clip: rect(110px, 160px, 170px, 60px);
}
在下面的演示中，你可以看到当元素在绝对定位/相对定位的切换中表现出来的效果：

http://jsfiddle.net/c0501uL5/6/embedded/result/点击预览

但是，你也可以将元素的position设置成position: fixed，因为，根据css官方规范，fixed的元素也属于‘absolutely positioned’元素。

5.元素竖向的百分比设定是相对于容器的宽度，而不是高度

这是一个很让人困惑的CSS特征，我之前也谈到过它。我们大家都知道，当按百分比设定一个元素的宽度时，它是相对于父容器的宽度计算的，但是，对于一些表示竖向距离的属性，例如padding-top,padding-bottom,margin-top,margin-bottom等，当按百分比设定它们时，依据的也是父容器的宽度，而不是高度。

下面是一个实例演示，你可以调整容器的宽度，但你会发现，黄块块的padding-bottom的距离也会随之宽度而变大或变小。

HTML代码

<div class="wrapper" id="w">
  <div class="box" id="b"></div>
</div>



<input type="range" min="120" max="400" value="400" class="range" id="r">
<output>宽度是: <span id="op">400px</span></output>
<output>黄块块的Padding bottom是:<br><span id="op2">10%</span></output>
CSS代码

body {
  font-family: Arial, sans-serif;
  padding-top: 30px;
  text-align: center;
}

.wrapper {
  width: 400px;
  margin: 0 auto;
  border: solid 1px black;
}

.box {
  width: 100px;
  height: 100px;
  background: gold;
  margin-left: auto;
  margin-right: auto;
  padding-top: 10%;
  padding-bottom: 10%;
  margin-bottom: 5%;
}

.range {
  display: block;
  margin: 20px auto;
}

output {
  text-align: center;
  display: block;
  font-weight: bold;
  padding-bottom: 20px;
}

output span {
  font-weight: normal;
}

实例演示

http://jsfiddle.net/c0501uL5/7/embedded/result/点击预览

上面的代码中，我们对内部子元素声明了3个竖向的距离，都是百分比形式。当移动滑块时，我们的js代码只需修改了容器的宽度。但是，这个这三个属性高度都跟随着变化，可以看出，它们的百分比计算是基于容器的宽度，而不是高度的。

6.border属性比你想象的要复杂

我们很多人都用过这样的写法：

.example {
  border: solid 1px black;
}
这里的border属性的用法实际上是一种简写的形式，它分别设置了border-style, border-width, 和border-color — 用一句代码表示它们三个。

但不要忘记，border-style, border-width, 和border-color也都有自己的简写形式。所以，border-width可以写成这样：

.example {
  border-width: 2px 5px 1px 0;
}
这样，四个边的宽度被一次设定。border-color 和 border-style 属性也可以这样做。下面的这个实例演示就是用的这种写法：

HTML代码

<div class="box">
CSS代码

body {
  font-family: Arial, sans-serif;
}

.box {
  width: 150px;
  height: 150px;
  margin: 20px auto;
  border-color: peachpuff chartreuse thistle firebrick;
  border-style: solid dashed dotted double;
  border-width: 10px 3px 1px 8px;
}

p {
  text-align: center;
}
演示

http://jsfiddle.net/c0501uL5/8/embedded/result/点击预览

其实，这些每个属性还可以继续细化，被拆分成border-left-style, border-top-width, border-bottom-color….

但是，你无法用border的简写分别对四个边设置不同的值，只能分开来设置。所以，border是一个简写里还有简写的属性。

7.text-decoration属性变成了属性简写

我相信有些小知识会让你大吃一惊。

跟着最新的CSS规范，text-decoration现在的写法是这样的：

a {
  text-decoration: overline aqua wavy;
}
text-decoration属性现在需要用三种属性值来表示了：text-decoration-line, text-decoration-color, and text-decoration-style.

但不幸的是，目前只有火狐浏览器实现了对这些新属性的支持。

你可以用火狐浏览器试一试下面的演示：

HTML代码

<a href="#" id="a">IT'S LIKE WATER, PEOPLE
(You should see a wavy line on top. Currently works only in Firefox)

CSS代码

body {
  padding: 30px;
  text-align: center;
  font-family: Arial, sans-serif;
}

a, a:visited {
  color: blue;
  background: aqua;
  -moz-text-decoration-color: aqua;
  -moz-text-decoration-line: overline;
  -moz-text-decoration-style: wavy;
  text-decoration-color: aqua;
  text-decoration-line: overline;
  text-decoration-style: wavy;
}
演示

http://jsfiddle.net/c0501uL5/9/embedded/result/点击预览

在这演示中，我们没有使用简写形式，而是分开描述每个属性。这是可以更好的保证浏览器的向后兼容，使那些目前不支持这种写法的浏览器也不受影响。

8.border-width属性可以使用预定义常量值

也许对与你来说这并不是一个什么新鲜信息。除了可以使用标准宽度值(例如5px或1em)外，border-width属性可以接受预定义的常量值：medium, thin, 和 thick

事实上，如果你不给border-width属性赋值，那它的缺省值是“medium”。下面的演示就是用了预定义常量值：

HTML代码

<div class="example">
CSS代码

body {
  font-family: Arial, sans-serif;
  text-align: center;
}

.example {
  width: 100px;
  height: 100px;
  margin: 20px auto;
  background: aqua;
  border: solid thick red;
}
演示

http://jsfiddle.net/c0501uL5/10/embedded/result/点击预览

在浏览器使用这些预定义常量值时，CSS规范里并没有规定都应该是什么宽度，但从我的观察看，它们的值分别是 1px, 3px, 和 5px.

9.为什么没有人使用border-image

之前我曾经写过一篇关于CSS的border-image属性的文章。现在几乎所有的现代浏览器都支持这个属性——除了IE10及以下IE版本。

看起来这是一个非常漂亮的CSS功能，它可以让你用图片修饰元素的边框。下面是一个实例演示，你可以拖拽调整里面的方块的大小，看看有什么边框图案的变化。

HTML代码

<div class="bi">
<p><上面的方块使用了图片描边。在这个方块的右下角，有一个可以调整这个方块大小的小三角，点住它，拖动它调整方块大小，看看有什么效果。.</strong></p>

<p>Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Vestibulum tortor quam, feugiat vitae, ultricies eget, tempor sit amet, ante. Donec eu libero sit amet quam egestas semper. Aenean ultricies mi vitae est. Mauris placerat eleifend leo.</p>
</div>


CSS代码

body {
  font-family: Arial, sans-serif;
  text-align: center;
}

.bi {
    border: 45px solid transparent;
    -webkit-border-image: url(http://www.webhek.com/wordpress/wp-content/uploads/2014/07/bg-pawprints-all.jpg) 45 round;
    -moz-border-image: url(http://www.webhek.com/wordpress/wp-content/uploads/2014/07/bg-pawprints-all.jpg) 45 round;
    border-image: url(http://www.webhek.com/wordpress/wp-content/uploads/2014/07/bg-pawprints-all.jpg) 45 round;
    font-family: Arial, Helvetica, sans-serif;
    color: #222;
    width: 500px;
    margin: 30px auto 30px auto;
    overflow: hidden;
    resize: both;
}

演示

http://jsfiddle.net/c0501uL5/11/embedded/result/点击预览

但不幸的是，这么好的一个功能，却没有看到多少人使用它，也许是我的眼界太窄。如果你在哪看到过有人使用border-image属性，或你在真正项目中使用了它，请留言告诉我。

10.你知道table里的empty-cells属性吗？

css里的empty-cells属性是所有浏览器都支持的，甚至包括IE8，它的用法是下面这个样子：

table {
  empty-cells: hide;
}
估计你从语义上已经猜出它的作用了。它是为HTML table服务的。它会告诉浏览器，当一个table单元格里没有东西时，就隐藏它。下面的演示里，你可以点击里面按钮，它会切换empty-cells的属性值，看看table的显示有什么变化。

HTML代码

<table cellspacing="0" id="table">
  <tr>
    <th>Fruits</th>
    <th>Vegetables</th>
    <th>Rocks</th>
  </tr>
  <tr>
    <td></td>
    <td>Celery</td>
    <td>Granite</td>
  </tr>
  <tr>
    <td>Orange</td>
    <td></td>
    <td>Flint</td>
  </tr>
</table>


<button id="b" data-ec="hide">切换EMPTY-CELLS</button>

CSS代码

body {
  text-align: center;
  padding-top: 20px;
  font-family: Arial, sans-serif;
}

table {
  border: solid 1px black;
  border-collapse: separate;
  border-spacing: 5px;
  width: 500px;
  margin: 0 auto;
  empty-cells: hide;
  background: lightblue;
}

th, td {
  text-align: center;
  border: solid 1px black;
  padding: 10px;
}

button {
  margin-top: 20px;
}
js代码

var b = document.getElementById('b'),
    t = document.getElementById('table');

b.onclick = function () {
  if (this.getAttribute('data-ec') === 'hide') {
    t.style.emptyCells = 'show';
    this.setAttribute('data-ec', 'show');
  } else {
    t.style.emptyCells = 'hide';
    this.setAttribute('data-ec', 'hide');
  }
};
演示

http://jsfiddle.net/c0501uL5/12/embedded/result/点击预览

在上面的演示中，我为能让单元格的边框显示出来，在单元格的边框间添加了空隙。有时候这个属性不会有任何视觉效果，因为你必须让你里面有可见的东西。

11.font-style的oblique属性值

对与css的font-style属性，我估计大家每次见到的都是使用“normal”或 “italic”两个属性值。但事实上，你还可以让它赋值为“oblique”。请看下面的演示：

HTML代码

<h1>Oblique Text</h1>
<h1>Italic Text</h1>
CSS代码

h1 {
  font-weight: normal;
  font-family: Georgia, serif;
  font-style: oblique;
  text-align: center;
  font-size: 50px;
}

h1:nth-child(2) {
  font-style: italic;
}

p {
  font-family: Arial, sans-serif;
  text-align: center;
}
演示

http://jsfiddle.net/c0501uL5/13/embedded/result/点击预览

这是什么意思？为什么“oblique”和斜体”italic”的效果是一样的？

CSS规范中是这样描述“oblique”的：

“…让一种字体标识为斜体(oblique)，如果没有这种格式，就使用italic字体。”
这里描述所用的“oblique”和“italic”都是倾斜的意思。“oblique”在维基百科里的解释就是一种排版术语，就是一种倾斜的文字，但不是斜体。

因为“oblique”对于font-style来说是一种合法的属性值，它可和italic进行互换，除非真有一种字体只提供了oblique体而没有提供斜体。

但我似乎从来没有听说过哪种字形提供过oblique字体，也许我错了。研究发现，一种字库好像不能同时提供斜体和oblique两种字体，因为oblique基本上是一种模仿的斜体，而不是真正的斜体。

所以，如果我没有猜错的话，如果一种字库里没有提供斜体字，那当使用CSS的font-style: italic时，浏览器实际上是按font-style: oblique显示的。

12.word-wrap和overflow-wrap是等效的

word-wrap并不是一个很常用的CSS属性，但在特定的环境中确实非常有用的。我们经常使用的一个例子是让页面中显示一个长url时换行，而不是撑破页面，下面是一个例子。

HTML代码

<p class="p" id="p">supercalifragilisticexpialidocious</p>
<button id="b" data-ww="break-word">TOGGLE word-wrap</button>
CSS代码

body {
  font-family: Arial, sans-serif;
  text-align: center;
}

.p {
  font-size: 24px;
  text-align: center;
  width: 200px;
  margin: 20px auto;
  border: solid 1px black;
  min-height: 60px;
  word-wrap: break-word;
}

button {
  display: block;
  margin: 0 auto;
}
JS代码

var p = document.getElementById('p'),
    b = document.getElementById('b');

b.onclick = function () {
  if (this.getAttribute('data-ww') === 'break-word') {
    p.style.wordWrap = 'normal';
    this.setAttribute('data-ww', 'normal');
  } else {
    p.style.wordWrap = 'break-word';
    this.setAttribute('data-ww', 'break-word');
  }
};
演示

http://jsfiddle.net/c0501uL5/14/embedded/result/点击预览

因为这个属性最初是由微软发明的，所以，所有的浏览器都支持这个属性。

尽管有所有的浏览器都支持，但W3C决定要用overflow-wrap替换word-wrap，我想可能是他们认为word-wrap用词不当。overflow-wrap跟word-wrap具有相同的属性值，但现在，word-wrap被当作overflow-wrap的备选写法。

虽然已经有不少的浏览器支持overflow-wrap这种写法，但看起来没必要使用overflow-wrap来让老的浏览器不支持。所有的浏览器都会继续支持word-wrap这种写法。

这其中有多少是以前不知道的？

不知道你从这篇博客里学到了多少知识？我希望它对你有些用处。非常有经验的Web程序员可能会知道其中的大部分，但未必全部。而如果你是新手，想必收益颇丰。

转自 WEB 骇客-12个你未必知道的CSS小知识