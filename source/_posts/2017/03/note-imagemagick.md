---
title: ImageMagick 使用小记
date: 2017-03-04 12:09:33
updated: 2017-03-04 12:09:33
tags: [Tech]

---
多亏 ImageMagick，最近终于把百科上某个咕了三个多月的页面……的一半……的80%填完了……

真的懒到令自己发指。

~~嘛，也好久没更新博客了，不能吃灰啊啊啊！~~

<!--more-->

填的坑是[神奇宝贝百科](https://wiki.52poke.com/wiki/主页 "主页")里[日月](https://wiki.52poke.com/wiki/精靈寶可夢_太陽%EF%BC%8F月亮 "精灵宝可梦 太阳／月亮")的[服装样式列表](https://wiki.52poke.com/wiki/服装样式列表（SM） "服装样式列表（SM）")，虽然看上去量很大（仅仅目前填完的女装部分维基代码就有65kB，位列百科第23）实际上在网络上找到了[别人整理的简表](http://www.pokeuniv.com/bbs/thread-7429-1-2.html "可购买服装位置及服装基本样式一览（女主）")后，用表格软件通过几个「机械化」的步骤就可以快速转化成维基格式的表格代码，这项工作在开坑后的半个月就基本成型了（当然那段时间也做了别的事……

然后我遇到了一个大问题：服装的图片。  
日月继承和发扬了 XY 的服装系统，女装共计70余种，很多服装都有2种版本通用颜色、5种太阳版限定颜色和5种月亮版限定颜色，[Serebii.net](http://serebii.net/sunmoon/customisation.shtml "Trainer Customisation") 提供了大部分通用颜色和月亮版限定颜色的服装截图，共计约300张。但这里提供的图片都是全身图，如果直接放在百科上会变得很难辨认，比如这张：
![宝石发夹](http://serebii.net/sunmoon/clothing/female/80.png "宝石发夹，售价110,000元")
要死啊，图片宽度是400px，放在百科上缩成64px的话什么都没了啊！  
所以有什么办法能裁切，批量裁切，一键裁切……啊？  
然后就开始咕咕咕，一咕就咕到2017年。

## ImageMagick
寒假咸在家的时候，百科的小伙伴截了一批发型图片让我处理一下上传到[百科](https://wiki.52poke.com/wiki/美髮沙龍 "美发沙龙")。他大概是用了截取屏幕上的固定区域的功能，所以要裁切的区域也是一样的。虽然图片很少可以用 Photoshop 一张一张裁，但既然咸在家，而且解决这个问题就能解决服装的大问题，不如查一查好了。

我的目的很简单：批量裁剪图片。

很快，[ImageMagick](http://www.imagemagick.org/script/index.php "官方网站") 再次进入了我的视线。之前我用这个工具制作了这个博客的 [Favicon](/favicon.ico)，把几张不同大小的图片整合进一张图片，只用了一行命令：

```bash
$ convert *.png favicon.ico
```

### 简介
先来介绍一下吧，**ImageMagick** 是一个基于命令行的、跨平台的位图工具套件，提供了编辑、格式转换、截图、显示图片等很多工具，可以参考[官网手册](http://www.imagemagick.org/script/command-line-tools.php "Command-line Tools")。这里我仅仅使用了`convert`一个工具。  
虽然使用命令行看上去「很落后」也不直观，但在我的需求环境下使用命令行的效率可能反倒比使用 PS 批处理来得高。

### 安装
[官网下载页](https://www.imagemagick.org/script/binary-releases.php "Install from Binary Distribution")提供了各个平台的安装方法，我使用的是 macOS，通过命令行的 [Homebrew](https://brew.sh/index_zh-cn.html "官网") 来安装：

```bash
$ brew install imagemagick
```

安装好之后别忘了试一下

```bash
$ convert
```


### 裁切命令
注：以下命令中花括号`{}`内的是参数。

```bash
$ convert {inputFile} -crop {positionX}x{positionY}+{width}+{heigth} {outputFile}
```

#### 参数说明
输入输出文件

* `inputFile`  
	输入文件名，ImageMagick 会自动判断文件格式。
* `outputFile`  
	输出文件名，ImageMagick 也会自动判断文件格式。

裁切位置

* `positionX`  
	裁切区域左上角相对于图片左上角的横向位置（像素）
* `positionY`  
	裁切区域左上角相对于图片左上角的纵向位置（像素）

裁切区域大小

* `width`  
	宽度（像素）
* `heigth`  
	宽度（像素）

比如：

```bash
$ convert 001.png -crop 152x72+96+96 crop-001.png
```

就是正好将 3DS 上屏截图中部96x96的部分裁切下来。

## 批处理
ImageMagick 的一行命令就完成了裁切，但我还想要批量裁切，它提供了输入文件名的格式化以便批处理，比如`*.png`；还提供了文件名指引（*Filename References*），可以从文本文件里获取列表，比如创建一个文本文件`list.txt`，内容是：

```
001.png
002.png
003.png
```

只要运行

```bash
convert @list.txt fin.gif
```

即可生成 GIF 文件。

然而，然而，然而，ImageMagick 似乎不支持**批处理覆盖原文件**，这个时候可以通过 Shell 脚本来实现：

```bash
for file in *.png; do
	convert $file -crop 152x72+96+96 $file
done
```

如果写进一行的话，运行的时候只要按上再回车就好了吧：

```bash
for file in *.png; do convert $file -crop 152x72+96+96 $file; done
```

就靠着这一行命令，我一晚上肝了200多张图……当然，上衣、下装、鞋子袜子什么的大小和位置需要调整，不过已经比我想象中的轻松多了。

那啥，我也不懂 Shell 脚本，似乎还可以做得复杂一些，利用循环语句从一张大图上裁取多个位置什么的，比如从一张名单图片上裁下每个人的照片之类的（只要位置、大小可以算出来）。

## 参考
* [我的ImageMagick使用心得](http://www.charry.org/docs/linux/ImageMagick/ImageMagick.html)