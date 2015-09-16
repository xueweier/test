# 如何画Flot柱形图

柱形图常被用来比较各组数据的相互关系, 直条的高度越高, 所代表的值越大. 除了柱形图之外, 还有条形图, 柱形图和条形图的做法一样, 差别只在一个属性值"horizontal", 如果设为true, 就会以条形图呈现, 另外x轴与y轴的数据需要互相对调位置.
如何画Flot柱形图
![image](http://img.bimg.126.net/photo/8Bjt62Qm3pQazjGFN0UMyQ==/5124533426011252403.jpg)
柱形图做法 一开始的做法和折线图一样, 先插入定位点到页面中, 并指定其id以及长度和宽度


接下来就是准备数据, 柱形图的数据和折线图的数据不太一样, 因为柱形图通常是用来比较各组数据的关系.
如何画Flot柱形图
![image](http://img.bimg.126.net/photo/76khFKNvqapalw2glpOenw==/4536532199662681236.jpg)
然后在在页面加载完毕后执行此函数。

$(document).ready(function() { initBarCharts(); });

柱状图就做好了其中可以指定到options里的xaxis.ticks里去设置自己想要的刻度，如果想要自定义y轴就设置yaxis中的属性data为数据源color可以调整自己想要的颜色