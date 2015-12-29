---
layout: post
title: "iOS笔记 (9)"
date: 2012-08-12 14:56
comments: true
tags: iOS
---

# xCode4 配色主题

一直以来，我对写代码的各种颜色就乱折腾不不管是Vim还是Emacs花在上面的时间都蛮多的。也不知道是哪里来的精神。
这次换xCode了。也折腾过很多。今天小文问了一下。恩，觉得这个东西可以拿出来写一篇blog，来填补我还差3篇的坑。

<!--more-->

## Path

``` bash
cd ~/Library/Developer/Xcode/UserData/
```

看看里面有没有一个叫FontAndColorThemes的文件夹，没有就新建立一个

``` bash
mkdir ~/Library/Developer/Xcode/UserData/FontAndColorThemes
```

然后你下载的那些Themes就可以丢里面。重启xCode就能看见了。

## Themes

### solarized
我见过史上最全最多的配色方案[solarized](http://ethanschoonover.com/solarized)

它居然支持我用过的所用编辑器和IDE:Vim,Emacs, IntelliJ IDEA, NetBeans, SeeStyle theme for Coda & SubEthaEdit, TextMate, TextWrangler & BBEdit ,Visual Studio, Xcode

而且不仅仅是编辑器，连总端配色都用。一条龙服务啊 Xresources, iTerm2, OS X Terminal.app, Putty.

完整版的地址在这里

	 https://github.com/altercation/solarized
	 
xCode4

	https://github.com/brianmichel/solarized/tree/master/apple-xcode4-solarized
	
把Solarized - Dark.dvtcolortheme和Solarized - Light.dvtcolortheme这两个文件丢到刚刚新建的~/Library/Developer/Xcode/UserData/FontAndColorThemes目录下重启xCode就可以用了。
效果如下:

Solarized - Dark
![Solarized - Dark](http://farm8.staticflickr.com/7249/7764088206_fa5ee8d24c.jpg)

Solarized - Light
![Solarized - Light](http://farm9.staticflickr.com/8296/7764088580_7f10c462e4.jpg)


### zenburn
zenburn的评价也很高，可是我不喜欢那灰蒙蒙的感觉
我是在[这里](http://allara.blogspot.com/2011/02/zenburn-theme-for-xcode-4.html)下载的

zenburn
![zenburn](http://farm9.staticflickr.com/8436/7764088966_2ac50f6e34.jpg)

### ObjectiveSheep
看上去也还可以 但是缺少色彩变化
我是在[这里](http://objectivesheep.com/blog/xcode4_color_theme)找到

ObjectiveSheep
![ObjectiveSheep](http://farm9.staticflickr.com/8424/7764089338_776811c569.jpg)

### DarkCity
这个本来是boss在用，今天小文也用这个。看着确实色彩丰富了一些。试试看。
找到的还是[Robin Lu](http://www.robinlu.com/hacking)制作的。他的blog很不错，顺带推荐一下。

我是在[这里](http://www.robinlu.com/blog/archives/320)下载到的

DarkCity
![DarkCity](http://farm9.staticflickr.com/8431/7764089724_93d11895c2.jpg)

### tomorrow-theme

summer推荐，也是各种编辑器方案都有

	https://github.com/chriskempson/tomorrow-theme

找到xCode4的两个文件 就ok了




### 小结
选个喜欢的进去根据自己的需求改吧。等等。怎么就完了。为什么在xCode里面不可以改变刚刚拷贝上去的主题

## 修改主题
因为刚刚考进去的文件权限的问题，并不能修改主题怎么办

``` bash
chmod 666 ~/Library/Developer/Xcode/UserData/FontAndColorThemes/*
```

这下，你可以爽快的修改各种主题了。






