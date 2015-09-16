---
layout: post
title: uebersicht - 一款漂亮的桌面Widgets
category: mac
---

Übersicht 是Mac下一款可以用来自定义桌面插件的工具。我们可以根据自己的喜好定制不同的Widgets。和Windows下的Rainmeter类似。把玩了一下感觉棒棒的。

效果还真挺炫的。目前在想着可以给自己的服务器加一些状态接口，通过uebesicht方便简单又干脆就可以拿到服务器的实时数据，实在是居家旅行必备。



![uebersicht](http://7vigrt.com1.z0.glb.clouddn.com/desktopScreenShot.png)

详细的介绍在[少数派](http://sspai.com/28020)上有介绍，下面是转载文。

## 定制你的 Mac 桌面，简单华丽的桌面自定义工具：Übersicht

<div class="clearfix typo content">
                            <p><a href="http://tracesof.net/uebersicht/" target="_blank">Übersicht</a>&nbsp;是一款可以用来自定义桌面插件的工具，基于当下流行的编程语言和系统级命令运行，再以美观通俗的 <a href="http://zh.wikipedia.org/zh/%E5%9B%BE%E5%BD%A2%E7%94%A8%E6%88%B7%E7%95%8C%E9%9D%A2" target="_blank">GUI</a>&nbsp;展现给用户，既做到了可读性，也秉承了这类工具的相对实用性。我们可以根据自己的喜好定制不同的 Widgets，例如在桌面放一个好看的时钟插件，一款精致的天气插件，或是一些系统数据的 Dashboard，等等。</p>
<p>由于 Übersicht 官方对 Widgets 的定义是一个完整的代码库，而不是简单整合的命令，所以它的插件整体质量相当之高。这虽然让插件的数量无法与同类应用抗衡（因为制作的成本较高），但用户完全可以自由修改参数，自定义插件的样式，以达到自己满意的效果。总而言之，Übersicht 是一款非常讨喜的桌面插件创造工具。</p>
<p style="text-align: center;"><a class="imagelightbox" href="http://cdn.sspai.com/attachment/thumbnail/2015/01/19/61691b7dfad35013aefc4055ecb73b52_mw_800_wm_1_wmp_3.jpg"><img class="lazy" alt="" src="http://cdn.sspai.com/attachment/thumbnail/2015/01/19/61691b7dfad35013aefc4055ecb73b52_mw_800_wm_1_wmp_3.jpg?q90" data-src="http://cdn.sspai.com/attachment/thumbnail/2015/01/19/61691b7dfad35013aefc4055ecb73b52_mw_800_wm_1_wmp_3.jpg?q90" style="opacity: 1;"></a></p>
<p>在相关的同类应用中，听说得比较多的当属 <a href="http://projects.tynsoe.org/en/geektool/" target="_blank">GeekTool</a>&nbsp;和人称「通知中心 GeekTool」的 <a href="../27662" target="_blank">Today Scripts</a>&nbsp;无疑，它们和 Übersicht 的运行原理基本相同，但后者的不同在于，它还支持 <a href="http://zh.wikipedia.org/zh/HTML5" target="_blank">HTML5</a>&nbsp;和特殊的 <a href="https://github.com/felixhageloh/uebersicht#readme" target="_blank">CoffeeScript</a>&nbsp;语法，所以 它的优势就很明显了：</p>
<ul>
<li><span style="line-height: 1.6;">相对轻松的编写和自定义过程。</span></li>
<li><span style="line-height: 1.6;">多元化的显示风格。</span></li>
<li><span style="line-height: 1.6;">主动适配不同屏幕尺寸的能力。</span></li>
</ul>
<h2>安装插件</h2>
<p>Übersicht Widgets 的安装方法很简单，先在官网&nbsp;<a href="http://tracesof.net/uebersicht/" target="_blank">下载安装 Übersicht</a>&nbsp;，然后前往&nbsp;<a href="http://tracesof.net/uebersicht-widgets/" target="_blank">官方 Widget 商店</a>&nbsp;寻找自己想要的插件，点击 Banner 封面图可以读取相关开发者的介绍，或直接通过右下方 download 进行下载。接着，将解压缩后得到的 .coffee 文件置入 Übersicht Widgets 的文件夹即可<span style="color: #808080;">（偏好设置中可以进行自定义）</span>。稍等片刻，桌面插件就会被主程序自动读取，安装完成。</p>
<p style="text-align: center;"><a class="imagelightbox" href="http://cdn.sspai.com/attachment/thumbnail/2015/01/19/36227647ea22131f99089d62076636ff_mw_800_wm_1_wmp_3.jpg"><img class="lazy" alt="" src="http://cdn.sspai.com/attachment/thumbnail/2015/01/19/36227647ea22131f99089d62076636ff_mw_800_wm_1_wmp_3.jpg?q90" data-src="http://cdn.sspai.com/attachment/thumbnail/2015/01/19/36227647ea22131f99089d62076636ff_mw_800_wm_1_wmp_3.jpg?q90" style="opacity: 1;"></a></p>
<p>不过，有些插件的安装方法比较复杂，比如 <a href="https://github.com/felixhageloh/uebersicht-widgets/tree/master/pretty-weather" target="_blank">Pretty Weather</a>&nbsp;这款。除了上述步骤，它还要求用户获取「天气数据」所需的个人 API Key，读者可以 <a href="https://developer.forecast.io/" target="_blank">点击</a>&nbsp;前往注册。接着，需要将 API Key 覆盖至源代码中的相关地址，方可正常显示天气数据。还有最后一步，就是获取你当前的位置信息，有条件的读者可以通过 <a href="https://www.google.com/maps" target="_blank">Google Maps</a>&nbsp;获取。</p>
<p style="text-align: center;"><a class="imagelightbox" href="http://cdn.sspai.com/attachment/thumbnail/2015/01/19/177cd29bb748af32cfbfb61869ffa6d3_mw_800_wm_1_wmp_3.jpg"><img class="lazy" alt="" src="http://cdn.sspai.com/attachment/thumbnail/2015/01/19/177cd29bb748af32cfbfb61869ffa6d3_mw_800_wm_1_wmp_3.jpg?q90" data-src="http://cdn.sspai.com/attachment/thumbnail/2015/01/19/177cd29bb748af32cfbfb61869ffa6d3_mw_800_wm_1_wmp_3.jpg?q90" style="opacity: 1;"></a></p>
<h2>自定义插件</h2>
<p>由于 Übersicht 的发布处于 OS X 10.9 与 OS X 10.10 的跨越阶段，所以部分插件可能存在兼容性方面的问题。这是个尴尬的局面，好在开发商对此特意加入了「Inspect Element」调试工具，以便使用者清楚地了解当前运行状态中可能存在的问题。可是...</p>
<p style="text-align: center;"><a class="imagelightbox" href="http://cdn.sspai.com/attachment/thumbnail/2015/01/19/91c82cbb7f45b89ffb409a4ad5a98eba_mw_800_wm_1_wmp_3.jpg"><img class="lazy" alt="" src="http://cdn.sspai.com/attachment/thumbnail/2015/01/19/91c82cbb7f45b89ffb409a4ad5a98eba_mw_800_wm_1_wmp_3.jpg?q90" data-src="http://cdn.sspai.com/attachment/thumbnail/2015/01/19/91c82cbb7f45b89ffb409a4ad5a98eba_mw_800_wm_1_wmp_3.jpg?q90" style="opacity: 1;"></a></p>
<p>掌握浅层系统知识的人都知道，像这类调整当前状态的「调试」工具，大多都是针对「内存数据」的，即修改的结果只能临时起效，而一旦发生缓存被清除或插件重启的行为，刚才的方案将全部失效，取而代之，插件会重新从源代码中读取数据，生成最原始的效果。我想说的是，当用户需要调整插件位置<span style="color: #808080;">（或外观）</span>这些基本参数时，若想保证状态的持久性，就一定要从源代码中下手。这里笔者推荐一款强大的代码编辑应用 <a href="http://www.sublimetext.com/" target="_blank">Sublime Text</a>&nbsp;系列（免费版即可）。</p>
<p style="text-align: center;"><a class="imagelightbox" href="http://cdn.sspai.com/attachment/thumbnail/2015/01/19/ea15b8cb6041d001ff284c42fc014d9a_mw_800_wm_1_wmp_3.jpg"><img class="lazy" alt="" src="http://cdn.sspai.com/attachment/thumbnail/2015/01/19/ea15b8cb6041d001ff284c42fc014d9a_mw_800_wm_1_wmp_3.jpg?q90" data-src="http://cdn.sspai.com/attachment/thumbnail/2015/01/19/ea15b8cb6041d001ff284c42fc014d9a_mw_800_wm_1_wmp_3.jpg?q90" style="opacity: 1;"></a></p>
<p>以插件 <a href="https://github.com/soberstadt/simple-clock-widget" target="_blank">Simple-Clock</a>&nbsp;为例。第一步，先将 .coffee 文件以 Sublime Text 方式打开，完成后就能看到插件的源代码<span style="color: #808080;">（见上图）</span>。第二步，在第 48 行找到 style 关键词，也就是「外观」和「位置」的参数地址。第三步，修改数据并使用 CMD-S 进行保存以刷新插件状态，完成修改步骤。</p>
<ul>
<li><span style="line-height: 1.6;">fontSize：文字大小</span></li>
<li><span style="line-height: 1.6;">width：插件占用的单位宽度<span style="color: #808080;">（百分比）</span></span></li>
<li><span style="line-height: 1.6;">transform：自比例调整能力<span style="color: #808080;">（自动）</span></span></li>
<li><span style="line-height: 1.6;">bottom：距离屏幕下方边缘的单位长度<span style="color: #808080;">（百分比）</span></span></li>
<li><span style="line-height: 1.6;">right：距离屏幕右侧边缘的单位长度<span style="color: #808080;">（百分比）</span></span></li>
</ul>
<p>还可以通过修改 background color 参数调整插件的背景色、修改 font-family 参数调整显示字体或修改 text-align 参数调整文字相对背景框的显示位置：center 置中、left 置左、right 置右等。插件<span style="color: #808080;">（位置及外观）</span>参数的单位可能是百分比，也可能是像素<span style="color: #808080;">（px）</span>，但笔者个人不建议没有相关基础的读者修改其默认单位，因为通常情况下会使 Widgets 出现「报错」现象，那就得不偿失了。</p>
<p style="text-align: center;"><a class="imagelightbox" href="http://cdn.sspai.com/attachment/thumbnail/2015/01/19/12150ec71b8fded2d9f968a7d62cb08b_mw_800_wm_1_wmp_3.jpg"><img class="lazy" alt="" src="http://cdn.sspai.com/attachment/thumbnail/2015/01/19/12150ec71b8fded2d9f968a7d62cb08b_mw_800_wm_1_wmp_3.jpg?q90" data-src="http://cdn.sspai.com/attachment/thumbnail/2015/01/19/12150ec71b8fded2d9f968a7d62cb08b_mw_800_wm_1_wmp_3.jpg?q90" style="opacity: 1;"></a></p>
<hr>
<p>其实最先 Übersicht 只是由独立开发者 <a href="https://github.com/felixhageloh" target="_blank">@Felix</a>&nbsp;托管在 Github 上的项目，因获得大量用户好评<span style="color: #808080;">（其中包括 <a href="http://brettterpstra.com/contact/" target="_blank">Brett</a>）</span>从而得以正身。所以，为保证用户能及时获取最新资讯和问题反馈，建议大家前往&nbsp;<a href="https://github.com/felixhageloh/uebersicht/issues?page=1" target="_blank">官方 Github 页面</a>&nbsp;或 <a href="http://tracesof.net/" target="_blank">官方网站</a>&nbsp;了解详情。</p>
                        </div>
