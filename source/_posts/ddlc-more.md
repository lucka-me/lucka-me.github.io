---
title: 翻译 | "Doki Doki Literature Club" Has More To It Than We Think.
date: 2017-11-06 10:33:00
updated: 2017-11-06 10:33:00
tags: [Game, Translate]

---

上周六玩了一下午 Doki Doki Literature Club! <sup>[Official](http://ddlc.moe "Doki Doki Literature Club!") · [Steam](http://store.steampowered.com/app/698780/Doki_Doki_Literature_Club/ "Steam 上的 Doki Doki Literature Club!")</sup>（*心跳心跳文学社！*），震撼之余也意识到游戏里还有很多等待玩家发掘的隐藏内容，其中最明显的就是`characters`文件夹下的四个文件，显然它们并不是所谓的「角色数据文件」而是完全由隐藏信息构成的。今天看到 reddit 上的这篇文章，感觉这个谜差不多(?)解出来了，正好有点时间，试着把原文翻译一下吧。

原文：["Doki Doki Literature Club" Has More To It Than We Think.](https://www.reddit.com/r/ARG/comments/71z2pt/doki_doki_literature_club_has_more_to_it_than_we/ "reddit")

*[翻译已获得原作者授权](https://twitter.com/Mithost/status/927513101813145600 "Twitter")。*

<!--more-->

请注意：

* 翻译得一般般。
* ~~翻译内容并不完整，但至少把我认为重要的部分先翻译出来，剩下的……咕咕咕~~……我也是没想到一个中午就肝完了……
* 稍微修改了一下格式。
* 其中<font color="gray">*（灰色斜体的部分）*</font>是我自己加的内容。
* 其中<font color="orange">*（橙色斜体的部分）*</font>是我不确定应该怎么翻译的原文。

EDIT: 所有文件都解决了，请到文章结尾处浏览！  
EDIT 2: 这游戏里的文件远比本帖子里讨论的多，<font color="orange">*(THERE ARE MORE FILES THAN WHAT ARE DESCRIBED IN THIS THREAD.)*</font> 它们涉及情节，而且并不是这个「另类现实游戏」（*ARG*）的重点，为了防止剧透我不会讨论这些东西。我也强烈建议把所有和剧情有关的东西都打上「剧透」标签，好让别人能自己享受这个游戏。  
**EDIT 3: 有一个叫 [JustMonika.com](https://justmonika.com/ "Just Monika") 的网站，显然这是别人自己做的一个 ARG。[开发者已经确认这是个爱好者网站。](https://twitter.com/Mithost/status/922577149676138496 "Twitter")这儿当然可以随便讨论这个网站啦，不过请注意这个网站和 DDLC 本身没有任何关系，它也没有暗示新的游戏内容、结局或别的什么东西 <font color="orange">*(lore)*</font>。**

---
经历了两年的精心开发之后，Dan Salvato 发布了一部视觉小说，所有人都对此表示一脸懵逼：万万没想到，一位靠着在 Smash 中出色的建模表现而闻名，还参与了 FrankerFaceZ 的开发者居然带着团队做了一部视觉小说。不过值得庆幸的是这部视觉小说的确非常棒，但我不禁会想，游戏的内容可能远不止我所看到的东西。

下面的内容或多或少地会涉及到 Doki Doki Literature Club<font color="gray">*（简称 DDLC）*</font>的真实内容，不过这毕竟不是一篇剧情分析之类的帖子，我会尽可能地压缩涉及剧透的内容。我再次建议你先独立地玩一遍游戏，毕竟人家是免费开玩的嘛。

话说就到这里，那我们开始心跳心跳大~~冒险~~解密吧！

---


## 文件里的秘密

和很多「粉切黑」的视觉小说一样，DDLC 需要玩家在游玩的时候对游戏文件多留个心眼。当触发某些事件的时候，游戏会创建、修改或者删除某些文件，某些时候玩家的思维也必须突破这个游戏界面。最有意思的文件在 `characters ` 文件夹里，里面有四个以 `角色名.chr` 命名的文件，分别对应四位女主角。

![分别代表四位角色的文件](https://i.gyazo.com/8fb1c0de1a4bcfc9c421d8e258ae32e0.png)

没有什么软件可以打开这种 `chr` 格式的文件，但如果用文本编辑器（比如我用的 Notepad++）打开，会发现里面藏着一些秘密。

---

### Sayori.chr
这是我最先取得了有价值的进展的文件。如果把它的后缀改成`OGG`，就会发现它其实是一段尖叫的音频。把这个音频播放出来吓吓人好像没什么意思啊，然后我想到一些老游戏会把图片藏在音频文件的频谱里，于是我用 Sonic Visualizer 打开这个文件，然后显示出它的频谱 <font color="orange">*(spectograph)*</font>……事实证实了我的猜测：游戏开发者埋的「雷」，远比我们想象的要多。

这是频谱的图像，第一张是未经处理的原始版本，第二张是处理之后的。

![原始版本](https://i.gyazo.com/f9ab2cd8d32d21661bec3283cf11dbe3.png)

![处理后](https://i.gyazo.com/97eb3646415f74df4f73cc7013e2987c.png)

也许有人还不知道<font color="gray">*（大家应该都看出来了吧）*</font>，这是一个标准的 QR 码，可以用一些手机 App 之类的扫描它来打开一个网址。这张图变形太大了所以可能扫不出来，我把它 PS 了20多分钟之后终于扫出来这个网页：

[http://www.projectlibitina.com/](http://www.projectlibitina.com/ "%%%%%%%%%%%")

这个网页上都是一些数据，似乎是关于两个虚构的「测试对象」（*test subjects*）的，但貌似没有别的隐藏信息了。后面我们研究其它大多数文件都会得出这个结论。<font color="orange">*(As seen in other files later in the post, most files will reach this sort of conclusion.)*</font>

---

### Monika.chr

文件开头的字符说明这个文件其实是个 PNG 图像。把后缀名改掉之后可以看到有一圈环形的火焰，中间围着一个由黑白色块组成的正方形。

![图片](https://i.gyazo.com/c146fd53abe24670bababc1d4e3b12af.png)

我寻思了半天这正方形到底是干啥的，然后一个老哥提出这说不定是个二进制码，被转成了这组黑白色块。我小心翼翼地把正方形裁下来，用线上工具把它转成了二进制码，然后我把这段二进制码丢进各种解码器看看能解出个啥。 ~~转成 ASCII 码的话它就变成了无法辨认的 Unicode 乱码，不过看上去的确像某种文件类型。我改了各种文件名，试了很久也没成功，所以我还是先把解码出的 Unicode 文本放在[这儿](https://pastebin.com/yWmAMq72)了，如果你有兴趣不妨看看啊。~~

把二进制码转成文数字（*alphanumeric*）后，出现了一组[令我面熟的字符串](https://pastebin.com/3J21BAqy)，然后我来了一发 Base64，duāng 地就把文本解出来了！

[这是解码的最终结果](https://pastebin.com/MZt9deig)

---

### Natsuki.chr
用文本编辑器打开这个文件，可以看到它其实也是个 PNG 图像（开头有个 `PNG`，就像 `monika.chr` 一样）。不像 `monika.chr`，这个图片貌似没有编码。

[图片](https://i.gyazo.com/92be63a12aadac797ef226d7becafe4d.jpg)  
<font color="gray">***（图片可能引起不适，请点击链接打开。）***</font>

值得注意的是，我用 Photoshop 打不开这个文件，我费了好些功夫把这图片倒腾进 Photoshop 里 <font color="orange">*(I did as much as I could in paint then copy/pasted the pixel array into photoshop for an easier time.)*</font>

倒置，旋转180度，然后偏移居中 <font color="orange">*(Inverting the image, flipping it 180 degrees, then offsetting it to center the image)*</font>（在此感谢 [/u/warchamp7](https://www.reddit.com/u/warchamp7)！）之后就出现了[这个](https://i.gyazo.com/fd4167dbe7891a46949b7bf0f03056df.png)<font color="gray">***（图片可能引起不适，请点击链接打开。）***</font>，一张女性的脸，非常扭曲。如果你把它作为纹理放进一个球形3D模型的话会不那么扭曲了，可以清楚地看出那是张脸。

---

### Yuri.chr

最后一个文件是一个单纯的[文数字字符串](https://pastebin.com/Jy57VE77)。把它放进随便哪个哈希类型检测工具就可以发现它是经过 Base64 编码的（在此感谢 [/u/warchamp7](https://www.reddit.com/u/warchamp7)） ，解码之后得到了[这篇短文](https://pastebin.com/YpZVHx0i)。

这篇短文是从[一篇2015年的恐怖故事](https://thoughtcatalog.com/anonymous/2015/06/i-found-a-box-containing-the-story-of-a-19-year-old-girl-who-killed-a-random-person-for-no-reason/)里来的，是关于一位19岁的少女成长为杀人犯的故事。这篇文章可以在 [/r/nosleep](https://www.reddit.com/r/nosleep) 的[这个帖子里](https://www.reddit.com/r/nosleep/comments/3azru6/i_found_a_small_box_hidden_in_a_mall_that/)找到。我有点在意的是这篇文章的发布时间是……[两年前](https://twitter.com/TeamSalvato/status/911274272990969858)……

EDIT:  [确认了，Dan Salvato 承认他两年前写了这篇文章。](https://twitter.com/dansalvato/status/911627120132055040)

---

所以说……这些是我目前知道的全部东西了。游戏里的其它文件貌似都没有埋什么「雷」，基本上都只是普通的程序文件而已。肝了这么久感觉身体被掏空，不过我还是应该把这些写下来！如果我找到了新的线索我会更新这篇帖子的。

**EDIT: 所有文件都被解出来啦！如果你不想看上面的那些破玩意，下面有个归纳精简版：**

在游戏文件的 `characters` 文件夹里有四个神秘文件分别对应游戏里的四位女主角。这些文件后缀名都是 `.chr`，但打不开。然鹅在经过一些恰当的修改之后，会解出一些肥肠有意思的信息。

* `Sayori.chr` 以 OGG 音频文件格式打开并获得频谱图形，可以解出一张 [QR 码](https://gyazo.com/f9ab2cd8d32d21661bec3283cf11dbe3)，指向 [http://www.projectlibitina.com/](http://www.projectlibitina.com/)。
* `Monika.chr` 以 [PNG 图形文件格式打开](https://i.gyazo.com/c146fd53abe24670bababc1d4e3b12af.png)，把中间的正方形黑白色块转换为二进制码，再把二进制码转换成文本，最后通过 Base64 解码可得到[一篇短文](https://pastebin.com/MZt9deig)。
* `Natsuki.chr` 以 PNG图形文件格式打开并在图片编辑器进行一系列操作之后可以得到[一张非常扭曲的疑似3D球模型纹理的女性的脸](https://i.gyazo.com/fd4167dbe7891a46949b7bf0f03056df.png)。
* `Yuri.chr` 以文本文件格式打开并用 Base64 解码可得到 [一篇短文（恐怖故事）](https://thoughtcatalog.com/anonymous/2015/06/i-found-a-box-containing-the-story-of-a-19-year-old-girl-who-killed-a-random-person-for-no-reason/)，[开发者承认这是他两年前写的](https://twitter.com/dansalvato/status/911627120132055040)，大概是这个游戏刚开始开发的时候。

以上就是全部了。

如果你想看看我~~从入门到入院~~的心路历程，去[我的 Twitter 时间线](https://twitter.com/Mithost) 上看比较好。如果你发现有任何值得分享的线索，请大声说粗来！如果真的还有意料之外的东西我会贼开心的啊！

[/u/Aran_SSB](https://www.reddit.com/u/Aran_SSB) 也编写了一份蛮全面的 Google 文档，不仅涵盖了这里讨论的东西，还有一些游戏里你可能会感到好奇的东西<font color="orange">*(in-game quirks)*</font>，[打开看看呗～](https://docs.google.com/document/d/1c3gS8Te3gbWgQ5sdGxvcfURkUAhkDXQxASyAPlirmlo/edit)

我们也开了个非官方的 [Discord 服务器](https://discord.gg/te7A39X)来讨论这些七七八八。在加入服务器前我们**强烈**建议你先独立通关至少一遍，不然怕不是要被剧透一脸。

---

*EDIT:* 游戏现在更新了个 1.10版，这次更新在某些场景增加了一些互动，强烈建议看一看啊。不过这次更新 把 `scripts.rpy` 加了混淆，所以不能查看游戏对话和过场信息了（不过数据也许被转移到哪里了）。在触发了某些事件之后一个叫 `enjoytheweekend!` 的新文件会出现，没有后缀名，感觉是新的线索。用文本编辑器打开，再 Base64 解码之后貌似还是不行。

被误导了……在 Twitter 用户 @lilmonix3 的帮助下，我们发现这段字符串是用维吉尼亚密码加密的密文，用密钥 `libitinia` 解码后的明文如下：

> What is a man without knowing the rich aroma of the future; the hot, complex balance of the present; and the bittersweet aftertaste of the past?

译文：

> 如果有人，  
> 不知未来浓郁强烈的芬芳，  
> 不知现在炙热复杂的平衡，  
> 不知过去酸甜苦辣的回味，  
> 他会是谁？

至此，我们几乎可以确定这些是这款 ARG 里所有需要我们解码的东西。如果游戏再度更新，这篇文章也会跟进。不过就目前来看应该算是完成了。
