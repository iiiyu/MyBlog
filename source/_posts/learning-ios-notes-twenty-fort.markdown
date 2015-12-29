title: iOS笔记(24) 关于使用xCode的Tab来提高开发效率
date: 2013-06-03 16:32:08
tags: iOS
---


# 前言

通过Google分析来看blog的访问统计。发现评价阅读时间也就一分钟不到。但是之前都习惯性写长篇大论。这次换一种方式来写blog。尽量写短一些的小一些的题目。使得更新数量上去。

这次我来说说怎么设置Tab来提高在xCode的工作效率。



# 我是如何使用Tab来提高效率的

## xCode的Tab是什么
诺，就是这一个东西。 

![](http://ww4.sinaimg.cn/large/686e6613jw1e5b26nmyqij20oq00v0sp.jpg)

使用过各种浏览器的你一定不会陌生。对在xCode里面我们也可以开出多个页面。而且每一个页面的状态是单独保存的。

<!--more-->

## 如何提高效率

因为在实际的代码编写过程中，我们可能需要来回的查找和阅读代码。会很自然的在多个文件中跳转编辑。这时候单个编辑页面明显拖累了速度。所以我们需要多个页面来回切换就会很爽。

如上图所示。我习惯性长开着这几个Tab。

## UI

![](http://ww3.sinaimg.cn/large/686e6613jw1e5b1quy5pij207409wt99.jpg)

如图所示，我们可以在圈起来的地方设置关键词过滤显示的文件
这样我的名为UI的Tab就只会显示storyboard。这样改UI点击起来会很方便

## Data
同理可得这个表情用来显示data model的。

![](http://ww2.sinaimg.cn/large/686e6613jw1e5b1uqa7yhj207w094mxd.jpg)

## VC

显示ViewController的

![](http://ww2.sinaimg.cn/large/686e6613jw1e5b1v4lkuyj207x0933z6.jpg)

## Debug

Debug这个Tab有些特殊。并不是我手动创建的。而且我配置了编译行为出来的。
这样每次Run的时候都会跳到这个名为Debug的Tab里面。这样做的原因是，我改了一个地方的代码。运行以后可能在其他地方挂掉了（或者在其他地方打了断点）。然后跟着进去看了看。然后想回到之前改代码的地方就会很麻烦。

![](http://ww4.sinaimg.cn/large/686e6613jw1e5b1ws3qu9j20ku0f8mzu.jpg)

这样设置了以后，就没有上述烦恼了。

# 顺便说一句

希望这些对你有所帮助。

顺便说一句：Tab直接的切换可以使用快捷键 Command + Shift + ([, ]) 其实这个快捷键适用于绝大部分有Tab的App。 都可以完成切换功能

再顺便说一句： xCode本身内存消耗很大，开Tab。感觉很是消耗内存。如果内存吃紧的话。应该去升级内存了。不然开多个Tab只会降低工作效率并不会提高。



