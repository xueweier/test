---
layout: post
title: Linux 命令之convert
category: tech
tags: linux linux-command
---
![](https://cdn.kelu.org/blog/tags/linux.jpg)



Linux 的 convert 命令可以用来转换图像的格式，支持JPG, BMP, PCX, GIF, PNG, TIFF, XPM和XWD等类型:

```
convert  xxx.jpg  xxx.png   将jpeg转成png文件 
convert  xxx.gif   xxx.bmp  将gif转换成bmp图像 
convert  xxx.tiff    xxx.pcx   将tiff转换成pcx图像 
```

# 简单应用

或者改变图像的大小: 

```
convert -resize 1024x768  xxx.jpg   xxx1.jpg 
convert -sample 50%x50%  xxx.jpg  xxx1.jpg
```

旋转图像， 将图像顺时针旋转270度

```
convert -rotate 270 sky.jpg sky-final.jpg
```

在图像里面添加文字： 

在图像的10,80 位置采用60磅的全黑Helvetica字体写上 Hello, World! 

```
convert -fill black -pointsize 60 -font helvetica -draw 'text 10,80 "Hello, World!" '  hello.jpg  helloworld.jpg 
```



# 安装

```
apt-get install imagemagick
convert -version 
```

# 应用

### 批量转换

```
----------- 从 PNG 转换到 JPG -----------
$ ls -1 *.png | xargs -n 1 bash -c 'convert "$0" "${0%.png}.jpg"'
----------- 从 JPG 转换到 PNG -----------
$ ls -1 *.jpg | xargs -n 1 bash -c 'convert "$0" "${0%.jpg}.png"'
```

### 水印

```
convert 1.png -fill white -pointsize 13 -draw “text 10,15 ‘lifesinger 2006＇” 2.png 
```

可以用-font指定字体，需要安装Ghostscript支持: http://www.cs.wisc.edu/~ghost/ 

还可以用composite命令在所有图片上加上水印，<http://www.imagemagick.org/script/composite.php> 

# 参考资料

* [通过命令行处理图形-使用 ImageMagick 进行翻转、缩放大小、旋转以及更多操作](https://www.ibm.com/developerworks/cn/linux/l-graf/index.html)
* [composite命令](http://www.imagemagick.org/script/composite.php)