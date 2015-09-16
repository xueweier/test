# 使用css模拟vista毛玻璃效果
2011/6/18 15:39:57

近来Windows Vista的毛玻璃效果一直受到界内同行的争相模仿。小弟不才，也来发表下自己的拙见。

首先准备两张背景图片，一张为正常图片，另外一张表现为正常图片的模糊效果。

利用css里面对背景的定义，将正常图片设置成为body节点的背景（注意body的margin必须设置为0，不然模糊的图片和正常的图片会有位置偏移），并且设置background-attachment的属性为fixed。 
接下来在需要应用毛玻璃效果的图片上设置样式，将模糊图片设置为需求节点的背景，同样设置background-attachment的属性为fixed（需求节点的位置最好为绝对定位）。
这样模仿Windows Vista的毛玻璃效果就基本完成。 
------------------------------------- 
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd"> 
<html xmlns="
http://www.w3.org/1999/xhtml"> 
<head> 
<style type="text/css"> 
*{ 
margin:0px; 
padding:0px; 
} 
body{ 
background-image:url(./back.jpg); 
background-attachment:fixed; 
} 
div.glass{ 
background-image:url(./glass.jpg); 
background-attachment:fixed;

position:absolute; 
top:100px; 
left:200px; 
width:300px; 
height:200px; 
overflow:hidden; 
} 
</style> 
</head> 
<body> 
<div class="glass"></div> 
<div style="width:3000px;height:3000px;"></div> 
</body> 
</html>

-------------------------------------
PS：暂时不支持IE6以下版本，如果各位要使用，back.jpg为原背景，glass.jpg自己把原背景PS下成磨砂玻璃的效果图，大小跟原图一样吧。
