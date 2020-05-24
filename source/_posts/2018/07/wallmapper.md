---
title: Wallmapper
tags:
  - Tech
  - Programming
date: 2018-07-23 20:18:31
updated: 2018-07-23 20:18:31
---

{% blockquote %}
[![](/images/2018/7/Wallmapper.png)](https://lucka.moe/Wallmapper/ "Demo")

用了近两年的壁纸们终于可以换了。
我先叉会腰。
然后<ruby><rb>写</rb><rt>水</rt></ruby>一篇开发心得。
{% endblockquote %}

<!-- more -->
## 初衷
做 Wallmapper 的初衷很简单：我觉得我需要换一套壁纸。
在我看来，壁纸是「自己不会注意但对别人可以稍稍彰显个性」的存在。我从2016年底开始用大豆（[@sssbeean](https://twitter.com/sssbeean "大豆(@sssbeean) | Twitter")）的几幅作品作为电脑、手机和手表的壁纸，画风非常独特，深得我心。
虽然还没有审美疲劳，但最近越来越频繁地被问到各种关于壁纸的问题：「这三个小姐姐到底是谁呀」、「还是这个人啊」、「这是宝可梦里的角色吗」……每次都感觉有点谜之尴尬。再说用别人的画作壁纸显然不够个性啊，真正的「个性」当然是在亲自创造或参与创造的过程中凝聚到成果中的。
我想自己创造壁纸，并且一直在尝试：之前练习板绘的目标之一是画出一张够资格作为壁纸的画，做 [DeMap](https://github.com/lucka-me/DeMap "DeMap | GitHub") 的初衷是希望生成壁纸。加之不久前的一些事让我希望（暂时）离开某些我过去深爱着的东西和回忆，而这些壁纸里刻画着这些回忆。
当然，最重要的是「**亲自动手把想要的东西做出来**」真的是一件很美妙的事，乐趣无穷，还很有成就感。

产生了作为基础的初衷，接下来需要确定做一个怎样的壁纸，一些 Idea。

## IDEA
我学习[地理信息科学](https://zh.wikipedia.org/wiki/地理信息科學 "地理信息科学 | 维基百科")专业，去年学期末有一次大地测量实习，内容大概是测绘+画地图，期间有人说把成果图印在衣服上会超有个性，虽然只是说说而已（至少我觉得用 [CASS](http://www.zwcad.com/product/partnerproduct/Structural/41.html "CASS地形地籍成图软件 | 中望软件") 画出的图毫无美感可言），不过倒是让我认识到地图除了用来做 LBS 之类正儿八经的事之外还能拿来看。是的，我看过的地图大部分是功能性很强但没什么美感的地图，剩下的是既没有功能性也没有美感的地图，它们限制了我对地图的想象力。

今年上半年，看到了[少数派的文章](https://sspai.com/post/43567 "用这个壁纸 App，做一张独一无二的壁纸：Mapapers | 少数派")，介绍了 [Mapapers](https://play.google.com/store/apps/details?id=com.matteolobello.mapapers "Mapapers | Google Play")，一款用简单的颜色给地图配色并将之保存成壁纸的 App，看上去超棒，可惜是 Android 独占。
然后我又找到了 [Alvar Carto](https://alvarcarto.com/ "Alva Carto")，这家提供精美地图挂画的网站还提供了一个[手机壁纸制作器](https://alvarcarto.com/phone-background/ "Background Generator | Alva Carto")，然而输出图片分辨率被 API 限制，我无法获取 2880×1800 的壁纸。

就这样，逛了一圈，没有发现别的办法。
但此时的我已经打定主意要拿地图做壁纸。

在这种情况，在我想要某件东西却没找到合适选项的时候，我会评估一下自己的需求和能力以及过程的趣味性和价值，决定是放弃还是亲自动手做一个。

## 决定
通过 Alvar Carto 的网站，我认识到 [Mapbox](https://www.mapbox.com "Mapbox")，一家比 Google Maps 更加开放、自由的定制地图提供商，其地图数据来自 OpenStreetMap。Mapbox 提供的 [Mapbox Studio](https://www.mapbox.com/mapbox-studio/ "Mapbox Studio | Mapbox") 更可以对地图样式进行详细的编辑，从而创造出[各种好看的地图样式](https://www.mapbox.com/gallery/ "Gallery | Mapbox")。

今年三月我才开始接触 HTML 和 JavaScript，之前仅有[一点点 MediaWiki 编辑经验](https://lucka.moe/wikist/ "Wikist")。一开始「Web GIS 原理与方法」课程的一次作业要求做一个网页并用 ActiveX 写一个插件，我却卡在了当时 Visual Studio 2017 的一个 Bug 上，解决无果只得另寻他法；接着「空间位置服务原理及应用」课程也布置了一个作业，好像是要求用百度地图 API 随便做个什么东西。
于是我开始尝试 HTML、CSS 和 JavaScript，当然还是像以前学习编程语言一样写什么查什么，靠着 [Google Maps JavaScript API 指导教程](https://developers.google.com/maps/documentation/javascript/examples/ "Code Samples | Google Developer") 和[别人的算法](https://github.com/zhiyishou/Polyer/blob/master/lib/delaunay.js "zhiyishou/Polyer/delaunay.js | GitHub")做出了 [DeMap](https://github.com/lucka-me/DeMap "DeMap | GitHub")，在地图上生成随机点然后生成 [Delaunay 三角形](https://zh.wikipedia.org/wiki/德勞內三角化 "德劳内三角化 | 维基百科")并依据高程填色的网页应用。
在这个过程中我学到了不少东西，包括对 HTML 结构的了解、CSS 的配置方法、JavaScript 编程的基础，这些都是制作 Wallmapper 的基础，另外这个界面也延续到了我之后制作的几个网页应用中，包括 Wallmapper。JavaScript 感觉上比较接近 Python，入门还是挺容易的，做一个这样的小应用，入门等级就够了。

制作地图壁纸最关键的一步是将地图转换为图片，虽然 Google Maps 提供了静态地图但分辨率很低。为此我很直白地搜到了[这个问题](https://stackoverflow.com/q/16235161 "Save current Google Map as image | Stack Overflow")，其中[这个回答](https://stackoverflow.com/a/19406604 "Save current Google Map as image | Stack Overflow")提到使用第三方的 [html2canvas](http://html2canvas.hertzen.com "html2canvas") 在 Canvas 中绘制 HTML 元素。之前在完善 DeMap 的时候我认识了 Canvas，HTML 5 中的「画板」，我用它将 SVG 转成了 PNG，完成了 DeMap 最后一项功能，虽然此时我已经不再想用 Low-Poly 图片当桌面了……

有了这些微薄但目测够用的基础，我决定动手开发。

## 开发
实际开发的曲折程度超出了预期，不过在成功后也带来了更多的满足感，真好玩儿。从开始到写这篇文章的时候，Wallmapper [经历了](https://github.com/lucka-me/Wallmapper/blob/master/CHANGELOG.md "lucka-me/Wallmapper/CHANGELOG.md | Github")从 `0.1` 到 `0.3.3`，3个大版本，11个小版本，每个大版本我都换了地图 JavaScript 库……希望以后不会换了（Flag×1

### 地图样式
作为壁纸的地图，样式很重要。我看过的为数不多的地图中，小米8的一则广告视频（[小米手机微信公众号](https://s1.mi.com/pages/92ae5cfef57d9ef9a523753e45fc9b0b/index.html?needValidHost=false&client_id=180100031058&masid=20109.00000&scene=4&subscene=126#wechat_redirect) | [爱奇艺](http://www.iqiyi.com/w_19rylz0p4p.html)）背景中的黑白地图让我印象~~比对手机的印象还~~深刻。在着手写代码之前，我构思了一下地图样式，越发觉得这种极简风格好看。于是确定了样式：底色（陆地）纯黑、道路纯白、水系深灰，无植被、建筑物等地物，无标注，将之命名为「Dark」。~~啊好没特色啊要不然改叫「Absolute Dark」、「Deep Space」、「Zero」之类的如何？~~

### `0.1`
一开始我并没有使用 Mapbox，而是直接把 DeMap 整个复制过来，删掉了不需要的部分，修改了一下界面，地图还是用 Google Maps，然后在 [Google Maps Styling Wizard](https://mapstyle.withgoogle.com "Styling Wizard: Google Maps APIs") 里创建了 Dark 样式，很简单地就成功套用进 Google Maps 里。但问题也出现得很快，html2canvas 似乎并不能在 Canvas 中正确地绘制这个容器，地图控件和版权标记还留着不说（这些理论上可以通过 CSS 或者 JavaScript 修掉），关键是每次输出的图像框内右侧和下侧的部分地图都不能成功加载出来。我犹豫了一下，我不确定我是否能解决这个问题（我要做的应用似乎有些「冷门」，网上可供参考的资料不是很多），其次我也真心想尝试一些新鲜事物，我很快选择放弃 Google Maps，转而寻找一个我未曾用过的提供商。

`0.1` 版就这么凉在 `0.1.2` 了，这时的 Wallmapper 甚至没法保存图片。

### `0.2`
从 `0.2` 开始，我换用 [Leaflet](https://leafletjs.com/index.html "Leaflet")，一款开源的地图 JavaScript 库，数据来源则使用 Mapbox，然后在 Mapbox Studio 里制作了 Dark 样式。使用 Leaflet 的最主要原因是发现了 [mapbox/leaflet-image](https://github.com/mapbox/leaflet-image)，一款将 Leaflet 地图图层转换成图片的工具，这下好了，我连 Canvas 都不需要了。
至于输出图片的方法，我的构想是在用户调整地图时显示的地图大小只保持用户所设定长宽的比例而不保持其大小，输出图片时则创建一个新的地图，将其大小设置为用户设定的大小，调整范围，输出，然后删除新地图。
我依照这个思路做出了 `0.2`，一切都变得很顺利……才怪咧，不然版本号就停留在 `0.2.x` 了。我很快发现两个比较大的问题：首先输出的地图范围和设定的不一样，排查之后发现是缩放分级太粗导致 `fitBounds()` 无法将预览地图的的范围精确地设置到新地图上，我尝试设置 `zoomSnap`，但这似乎并不能解决问题，反而导致 leaflet-image 输出空白图片；其次在修改图片大小，即预览图长宽比之后，地图仍然只在原先的区域加载和显示，底部新增的部分则不显示，这个问题倒是好解决，设置容器大小后调用一下 `invalidateSize()` 就好。

虽然已经可以输出图片了，但范围不一致的问题始终无法解决，另外 Leaflet 不支持即时更改地图样式，只能新增图层并把原先的图层删除，这对后续添加更多地图样式选项非常不利，于是这个大版本在 `0.2.3` 就结束了。

### `0.3`
为了能即时更改地图样式，我从 `0.3` 开始换用 Mapbox 自家的 [Mapbox GL JS](https://www.mapbox.com/help/how-web-apps-work/#mapbox-gl-js-1 "Web applications | Mapbox")，它可以通过调用 `setStyle()` 方便地修改地图样式，顺便还（有些意外地）解决了范围不一致的问题，leaflet-image 和 html2canvas 也不需要，直接调用 `getCanvas()` 就能获取到地图的 Canvas，没有控件和版权标记。
接下来就是一些小问题啦。
- 在新地图创建时要设置 `preserveDrawingBuffer: true` 以确保地图加载完毕，否则 Canvas 里可能是空的。
- 在高 DPI 设备（比如 Retina）上，输出图像的长宽分别是容器长宽乘以 `window.devicePixelRatio`，所以在设置容器时要将长宽分别除以它。
- 使用 `toDataURL()` 生产并下载图片可能在 Chrome 上出现问题，即浏览器开始下载（图标上也出现下载标志）但并不出现在下载列表里，解决方法是用 `Blob` 下载。
- `toBlob()` 方法并不是所有浏览器都支持（比如 Edge 和 IE），如果不支持可以[参考此处](https://stackoverflow.com/a/47487073 "Canvas to blob on Edge | Stack Overflow")现场添加。

## 总结 & 期望
总的来说做完了还是蛮开心的，把 `0.2.2` 提交和上传完的时候已经将近5点了，天都要亮了，当时处在一种「啊算是做出来了但范围不一致就很不爽」的状态，差点没睡着……好在下午开辟了 `0.3`，解决了问题，也最终完成了这个应用的基础功能。

有壁纸了，不过以后应该还会继续做下去吧，而且这个 Repo 居然被~~野生~~大佬 [@jingsam](https://github.com/jingsam "jingsam(@jingsam) | GitHub") 打星了！第一次被~~野生~~大佬打星啊啊啊！超开心的！话说我在 Demo 里用了用这位做的地图样式，这么巧的吗（

如果以后继续做，我希望能添加这些东西：
- 输入中心坐标、缩放等级，满足对齐强迫症的需要。
- 用 [uiGradients](https://uigradients.com) 的颜色创建一些新配色/风格？
- 更多好看的地图样式，也希望我自己能作出一些来。

不过下半年预计事情很多而且烦……我也不知道 Wallmapper 会怎样吧。
