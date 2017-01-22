# 如何将网页CSS背景图高斯模糊且全屏显示
AmelieAmelie 1.1k 2012年09月29日 提问

0 关注

    6 收藏，10.6k 浏览

1

以Path为代表的，展示了这种背景图模糊并全屏显示的方法，而且会根据屏幕分辨率放大缩小。
这种效果应该怎么实现呢？

    css3
    css

    链接
    评论
    更多

3 个回答
3
采纳
willerce 1.7k 2012年09月29日 回答 · 2013年04月13日 更新

高斯模糊是PS、FW图片处理工具搞的。

全屏显示的方法
1：使用CSS

.bg {
    background-image:url(scale.jpg);
    -moz-background-size: 100% 100%; /*  Firefox 3.6 */
    -o-background-size: 100% 100%;/* Opera 9.5 */
    -webkit-background-size: 100% 100%;/* Safari 3.0 */
    background-size: 100% 100%;/*  Firefox 4.0 and other CSS3-compliant browsers */
    -moz-border-image: url(scale.jpg) 0; /* Firefox 3.5 */
    filter:progid:DXImageTransform.Microsoft.AlphaImageLoader(src='scale.jpg', sizingMethod='scale');/* for < ie9 */
}

2：使用 IMG 标签

<img class="stock" style="position: absolute; top: 0px; left: 0px; height: 100%; width: 100%;" src="default.jpg">

补充方法

3：使用 CSS3 的背景 Cover

.bg {
    background: #000 url(scale.jpg) no-repeat center center;
    -webkit-background-size: cover;
    -moz-background-size: cover;
    -o-background-size: cover;
    background-size: cover;
}

