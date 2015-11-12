---
layout: post
title: javascript入门
category: linux
---

最近使用javascript的频率比较多，是不是会和PHP弄混。于是记录一些简单的信息信息，避免每次都去查阅手册。注意，这并不是一个完整的入门教程，例如我不会在本文包含最基础的加减乘除此类的信息，主要是一些容易与其他语言混淆的内容。

## 基本知识

### 语法

	<script> 和 </script> 之间的代码行包含了 JavaScript
	<script type="text/javascript" src="jq.js ></script>
	
	分号用于分隔 JavaScript 语句
	JavaScript 对大小写是敏感的。
	javascript是动态类型
	
	单行注释以 // 开头。
	多行注释以 /* 开始，以 */ 结尾。

### 变量声明

	var name="Gates",
	age=56,
	job="CEO";
	
	未使用值来声明的变量，其值实际上是 undefined
	
	变量类型有：数据类型，字符串、数字、布尔、数组、对象、Null、Undefined。

### 数据类型

JavaScript 中的所有事物都是对象(拥有属性和方法)

	var carname=new String;
	var x=      new Number;
	var y=      new Boolean;
	var cars=   new Array("hello","world","2");
	var person= new Object;

	var myString = "hello";
	var i1 = 3.2;
	var flag = true;
	var arr=["Hello","world"];	// 静态定义（相同数据类型的集合）
	var arr2= new Array("hello","world","2"); // 
	var arr3= new Array();
	
	数组的显示是[]包裹。
	
#### 对象Object

	对象的显示是{}包裹。

	// 这个people将会以全局方式村子啊。如果加一个var，那就是局部的。
	people = new Object();
	people.name = 'bill';
	people.age = 13;

	或者这么定义:
	
	people = {name:"bill",age:30};
	
	或者使用函数来定义一个类对象。
	
	function people(name,age){
		this._name = name;
		this._age = age;
	}
	son = new people('bill',23);
	
#### null 
	
	可以通过赋值为null的方式清除变量
	cars=null;
	person=null;

### 语句

if else
for
while
switch case
try catch

for (变量x in 对象array)
{
    在此执行代码
    array[x];
}

### 函数

通常，我们需要在某个事件发生时执行代码，比如当用户点击按钮时。
如果我们把 JavaScript 代码放入函数中，就可以在事件发生时调用该函数。

	function demo(a,b){}
	
	// 两种调用方法
	1. demo();
	2. <button onclick="demo()"></button>

### 运算

如果把数字与字符串相加，结果将成为字符串。

有一个比较特别的运算符 === ，给定 x=5

	===	全等（值和类型）	x===5 为 true；x==="5" 为 false

### 事件
	
HTML DOM 允许您通过使用 JavaScript 来向 HTML 元素分配事件

事件句柄　(Event Handlers)

	onabort	图像的加载被中断。
	onblur	元素失去焦点。
	onchange	域的内容被改变。
	onclick	当用户点击某个对象时调用的事件句柄。
	ondblclick	当用户双击某个对象时调用的事件句柄。
	onerror	在加载文档或图像时发生错误。
	onfocus	元素获得焦点。
	onkeydown	某个键盘按键被按下。
	onkeypress	某个键盘按键被按下并松开。
	onkeyup	某个键盘按键被松开。
	onload	一张页面或一幅图像完成加载。
	onmousedown	鼠标按钮被按下。
	onmousemove	鼠标被移动。
	onmouseout	鼠标从某元素移开。
	onmouseover	鼠标移到某元素之上。
	onmouseup	鼠标按键被松开。
	onreset	重置按钮被点击。
	onresize	窗口或框架被重新调整大小。
	onselect	文本被选中。
	onsubmit	确认按钮被点击。
	onunload	用户退出页面。

用法示例

	<p><a href="http://developer.mozilla.org/" onmouseover="oTooltip.append(event,'Example text 1');" onmousemove="oTooltip.follow(event);" onmouseout="oTooltip.remove();">Move your mouse here&hellip;</a></p>

	
鼠标 / 键盘属性

	altKey	返回当事件被触发时，"ALT" 是否被按下。
	button	返回当事件被触发时，哪个鼠标按钮被点击。
	clientX	返回当事件被触发时，鼠标指针的水平坐标。
	clientY	返回当事件被触发时，鼠标指针的垂直坐标。
	ctrlKey	返回当事件被触发时，"CTRL" 键是否被按下。
	metaKey	返回当事件被触发时，"meta" 键是否被按下。
	relatedTarget	返回与事件的目标节点相关的节点。
	screenX	返回当某个事件被触发时，鼠标指针的水平坐标。
	screenY	返回当某个事件被触发时，鼠标指针的垂直坐标。
	shiftKey	返回当事件被触发时，"SHIFT" 键是否被按下。


## 对象

### DOM对象
网页加载时候，就会产生dom对象
用dom操作HTML对象。

#### DOM对象获取元素

首先需要获得对象，document.getElementByXXX
在这里顺带说一下jQuery，则是：

$("button").text(xxx);
$("#id").text(xxx);
$(".class").text();
 
<script>
var str = $( "p:first" ).text();
$( "p:last" ).html( str );
</script>
	
	// 子节点
	ChildNode.remove() 
	ChildNode.before() 
	ChildNode.after() 
	ChildNode.replaceWith() 
	
	// 父节点
	if (node.parentNode) {
	  // remove a node from the tree, unless 
	  // it's not in the tree already
	  node.parentNode.removeChild(node);
	}
	
	var p = document.createElement("p");
	document.body.appendChild(p);

#### 改变HTML
改变HTML内容 `innerHTML`
改变属性 直接`.属性值=""`即可
#### 改变CSS
`.style.background=""`

#### 设置事件监听器
	target.onclick();	//dom0级，不好用
	
	target.addEventListener(type, listener[, useCapture]);
	EventTarget.removeEventListener();
	第一个参数是事件的类型 (如 "click" 或 "mousedown").
	第二个参数是事件触发后调用的函数。
	第三个参数是个布尔值用于描述事件是冒泡还是捕获。该参数是可选的。
	
	添加句柄的好处：添加多个；更好的颗粒度；对所有dom对象有效。
	
	对应与jQuery，一般我们使用on函数。

#### 事件对象

	触发dom事件都会产生事件对象。
	包括
	event.type，
	event.target，
	event.stopPropagation();阻止冒泡对象
	event.preventDefault();阻止对象默认行为
	
	移动设备端：
	touchstart: 当手指触摸屏幕时触发，即使是一个手指放在屏幕上也会触发。
	touchmove:当手指在屏幕上滑动时连续地触发，这个事件发生期间，我们可以使用preventDefault()事件可以阻止滚动。
	touchend: 当手指从屏幕上移开时触发。
	touchcancel: 当系统停止跟踪触摸时触发

	
#### 事件流
	事件冒泡 -由具体向上冒泡
	事件捕获 -由上往下到具体

### 内置对象
#### String字符串对象

	String.prototype.indexOf()  查询字符处是否存在
	String.prototype.match() 内容匹配
	
	String.prototype.replace()
	
	String.prototype.toString()
	String.prototype.toUpperCase()
	String.prototype.trim()  删除前后空格
	String.prototype.split()


#### Date日期对象

	var today = new Date();
	var birthday = new Date('December 17, 1995 03:24:00');
	var birthday = new Date('1995-12-17T03:24:00');
	var birthday = new Date(1995, 11, 17);
	var birthday = new Date(1995, 11, 17, 3, 24, 0);
	
	var unixTimestamp = Date.now(); // in milliseconds
	
	还有各种get函数。
	

#### Array数组对象

`concat`

	var new_array = old_array.concat(value1[, value2[, ...[, valueN]]])
	
`sort`，`push`末尾追加元素 `reverse`翻转

#### Math对象

	Math.PI
	Math.round(x) 四舍五入
	Math.random() 随机数
	Math.pow(x, y)次方
	Math.max([x[, y[, …]]]) 最大值

### 浏览器对象

#### window对象

	window.document.write("高度"+window.innerHeight+"宽度"+window.innerWidth);
	window.open();
	window.close();
	
	outerHeight
	outerWidth
	Window.scrollX     
	Window.scrollY
			
    function btnClick(){
        window.open('/','windowname','height = 200, width = 100, top = 200');
    }
#### 计时器
setInterval()
setTimeOut();

	var intervalID = window.setInterval(myCallback, 500);
	
	function myCallback() {
	  // Your code here
	  
	}
	
     var win;
    function mywin(){
        win = window.setTimeout(function(){
            alert("hello");
        },3000);
    }

#### History对象

	function myback(){
       window.history.back();
    }
    window.go(-1);
	history.back();     // equivalent to clicking back button
	history.go(-1);     // equivalent to history.back();


#### Location对象

	 console.log(window.location);
	
	 Location { href="http://localhost:63342/i...m=Home&c=Index&a=charge",  origin="http://localhost:63342",  protocol="http:",  更多...}
		""

#### Screen对象

console.log(window.screen);
			
	availHeight
	availLeft
	availTop
	availWidth
	colorDepth
	height
	left
	pixelDepth
	top
	width
		
#### 一些宽度数据

	screen：屏幕。这一类取到的是关于屏幕的宽度和距离，与浏览器无关，应该是获取window对象的属性。
	client：使用区、客户区。指的是客户区，当然是指浏览器区域。
	offset：偏移。指的是目标甲相对目标乙的距离。
	scroll：卷轴、卷动。指的是包含滚动条的的属性。
	inner：内部。指的是内部部分，不含滚动条。
	avail：可用的。可用区域，不含滚动条，易与inner混淆。
	
	var offsetWidth =element.offsetWidth;
 