---
layout: post
title: "iOS笔记 (3)"
date: 2012-03-08 00:34
comments: true
tags: iOS
---

# iOS笔记番外篇

## 开始之前的扯淡
反正在等IPad3的发布，就顺手写篇blog。本来觉得这个时候写IOS笔记3还有点过早。因为好像没有太多的干货能拿出来。可是在看了老头用xcode、git来教怎么管理代码的时候，我彻底崩溃了。VS新版没见过，目前Xcode是体验最好的IDE，没有之一。Emacs系列突然觉得没有必要更新的冲动。

虽然我们居住在神奇的天朝，但是我们还是要感谢这个时代。因为科技和人类文明的进步，使得我们可以看到一流名校的课程。就算那个号称天朝最公平的考试失败以后，只要你想学，还是可以学到这个世界上一流的课程。

多媒体的表现力要远远丰富于文字。清楚直观的5分钟视频，可能换成文字描述写一个小时都写不好。所以如果这篇看不懂的话直接看视频吧。（没找到链接）

	Xcode and Source Code Management (October 7, 2011) - HD

>人生是一场冒险。对我来说：活下去，然后活的爽一些。就够了。

<!--more-->


## 补遗笔记二
上次忘记写一个很有特色的东西。方法之前的+、-符号。这个其实很简单。在笔记一里面就有：

	在ObjC中一个类中的方法有两种类型：实例方法，类方法。实例方法前用(-)号表明,类方法用(+)
	
可是我这个人对专有名词都他喵的没感觉。比较喜欢通俗易懂的解释。

	+的方法，调用的时候，是[类名字 方法]
	-的方法，调用的时候，是[类实例 方法]
	
``` objc
NSString *str = [[NSString alloc] init];//这个 alloc就是+号定义的
[str floatValue]; //这个floatValue就是-号定义的
```

## Git
其实，我觉得Git完全有必要单独出来写一篇。因为它太重要太强大了。用了Git以后再也没有想过用其他的版本管理。原因是用的太爽，如果有一天出现了比Git用起来还爽的。我也肯定会用。可是现在only git。

对于Git的中文心得，教程等等。我都自认怎么写都写的不可能有阳志平大哥写的好。所以这里只是告诉你Git是IOS开发必须的技能。你可以通过下面链接来学习

[git - 简易指南](http://rogerdudler.github.com/git-guide/index.zh.html)

[Pro Git](http://progit.org/book/zh/)

[Git与Github入门资料](http://www.yangzhiping.com/tech/git.html)

[如何高效利用GitHub](http://www.yangzhiping.com/tech/github.html)

[GotGitHub](http://www.worldhello.net/gotgithub/)

资料很多，但是如果你不使用的话。看再多的游泳的书，你也学不会游泳。

## Xcode

#### 编辑魔法

首先说一个编辑的小魔法，当你想修改源码中相同一个地方，而不想点xcode的Refactor的话。你可以完全通过键盘来实现

	Command + c #copy你想替换成的文本
	#选择你要替换的文本 Command + e
	Command + v #替换了第一个地方
	Command + g #到了下一个替换的地方
	#重复 Command + v和 Command + g
	#完成替换
	
其实这个小技巧你可以再Mac的任意编辑文本处使用。

#### 一些界面说明

![xcode1](http://farm8.staticflickr.com/7040/6816041908_bc88f08c2b.jpg)

看见右边有的文件后面是M有的是A。至今我没有看见哪里说明过这个是什么。其实很简单，通过Git管理你的项目以后，xcode自然能追踪你的文件情况。M是modified的缩写。A自然是add的缩写。（其实在没有用Git的时候xcode也能追逐，估计是跟关闭之前的保存的那一次对比吧）


![xcode2](http://farm8.staticflickr.com/7192/6816060622_aaf083d9bf.jpg)

这几个按钮很有用，具体看视频里面老头的操作。

Editor第一个是单独一个文本编辑，第二个是两个文本不同文本编辑，第三个是同一个文本不同时期对比编辑（这个用Git，然后很牛的）

View第一个显示右边的导航，就是文件结构那个。第二个是调试台。第三个是可以拖控件的

Organizer这个功能比较多。暂时就说点过去了可以在里面管理Git的分支各种。


#### 在IOS项目下建立Git管理

 老头视频里面是自己建立的，刚刚我看我的所有项目都有git了，暂时不知道是我设置了什么还是默认的。直播开始了。先看直播。

