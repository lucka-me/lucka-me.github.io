---
title: Hexo 博客改造笔记
date: 2017-04-12 22:44:58
updated: 2017-04-12 22:44:58
tags: [Tech]
---
最近开始不满足于 [NexT](http://theme-next.iissnan.com "NexT 使用文档") 提供的功能，想修改定制一下，于是小心翼翼地查资料，小心翼翼地修改……然后总结一下放在这里。  
以后也会进行一些改造（大概），毕竟有很多想做的……这篇文章也会跟着更新的。

<!--more-->

## 修改文章排序
目前文章按创建日期（即 Front-matter 中的 `date`）排序，即使加入了`updated`排列也不会随之改变。因为有一些文章（比如 [Ingress 某系列任务说明](/2017/04/03/yuyuko-mission/)和本文）预计会长期更新，所以希望能按更新时间排序。

### 要修改的内容
* `/node_modules/hexo-generator-index/index.js`  
	
	```js
	order_by: '-date'
	```
	修改为

	```js
	order_by: '-updated'
	```

这样的话就**只**按 Front-matter 中的`updated`排序了，因此所有文章的 Front-matter 中都必须要有`updated`，已发布的文章可以手动修改或者找方法批量将 `data`换成`updated`，对于将来的文章，只要做如下修改：

* `/scaffolds/post.md`  
	在`date`后加入一行
	
	```
	updated: {{ date }}
	```

### Note
所以似乎可以按任何 Front-matter 中的参数进行排序，比如加一个`Top`参数，设置文章等级，置顶文章……有办法按多个参数排序吗？

### 参考内容

* [杂记](https://www.lilonghe.net/archives/website-changelog.html#主页改为按修改时期排序文章 "网站更新日志 | 杂记") 主页改为按修改时期排序文章  
	虽然用这位的方法整个排序都混乱了……但通过它摸索到了解决办法。

## 圆形头像
把左边 Sidebar 里的头像做成圆形的。

### 要修改的内容
* `\themes\next\source\css\_common\components\sidebar\sidebar-author.styl`  
	在
	
	```css
	.site-author-image {}
	```
	的尾部加入

	```css
	border-radius: 128px;
	-webkit-border-radius: 128px;
	-moz-border-radius: 128px;
	```

### Note
其中的值我也不知道应该设置为多少……资料中写的是`80`但我改成`128`好像也没什么关系233

### 参考内容
* [Ehlxr's Blog](http://ehlxr.me/2016/08/30/使用Hexo基于GitHub-Pages搭建个人博客（三）/#11-1-头像圆形修改 "使用Hexo基于GitHub Pages搭建个人博客（三） | Ehlxr's Blog") 头像圆形修改