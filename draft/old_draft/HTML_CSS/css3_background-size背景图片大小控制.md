# css3 background-size背景图片大小控制

今天想要自己做一个网页玩，在给body加一个背景图片遇到了一个问题，因为传统的方式都是做一个小图通过平铺repeat-x或者repeat-y还实现的，但是当我想用一个大图力作背景的时候就会遇到一个很尴尬的问题，图片不能自动适应窗口的大小，不加no-repeat又被重复平铺达不到我想要的效果。

现在上面的问题可以用css的background-size来解决这个问题，background-size需要两个值，它的类型可以是像素（px）、百分比（%）或是auto，还可以是cover和contain。第一个值为背景图的width，另外一个值用于指定背景图上的height，如果只设定1个值，则第2个默认为auto，但当值为cover和contain时除外。

cover：保持图像的宽高比例，将图片缩放到正好完全覆盖定义的背景区域，其中有一边和背景相同

contain：保持图像的宽高比例，将图片缩放到宽或者高正好适应定义背景的区域，但背景仍在定义的区域内被包含。

支持浏览器：IE(9)、firefox、Chrome、Opera、Safari。
![image](http://img.bimg.126.net/photo/2m2zzrzNXx-m_JoJOd8z-A==/4843621399254005423.jpg)
css3 background-size背景图片大小控制

这样背景图就可以自适应你的窗口啦