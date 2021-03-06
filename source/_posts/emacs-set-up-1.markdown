---
layout: post
title: "调教Emacs(一)——存活"
date: 2012-02-26 22:59
comments: true
tags: Emacs
---


# 序

  一直以来，各界对编辑器的争论由来依旧。最后都会沦为Emacs vs Vim。这里就不在过多的讨论了。
  
  不管是Emacs还是Vim都是需要一个长期的调教过程。才会让你用的很爽。如果你妄想今天安上他们，明天就变得像魔术师一样的按键。那你洗洗睡吧亲。明天去泡妞别做宅男了。它们会耗费你大量的时间，用那些时间说不定你都可以找一个妹子了。
  
  在我学习使用Emacs的时候，入门资料相当少。有一些写的好的，却又各种Lisp配置。可是要了亲的命了。就想找一个先教我用的爽的，那些各种配置，各种文件能不能先放放。就只是先让我用的爽爽。
  
  我提供了一些基本配置。几乎没有改里面的内容，但是可以用。可能用的不是很爽，但是基本的功能都有了。先用用心里有个谱。然后再去研究高深的使用。
  
  我不会Lisp，所以这些配置最多算我这个工具仔拼凑起来的。
  
  所以，看这系列Blog的亲们。
  
  * 只要安装Emacs 23.3-23.4 (其他版本不知)
  * 会git clone
  * 知道~/目录
  * 不需要会Lisp
  
  就可以开始了。
  
  <!--more-->
  
  

# 安装

首先确认你已经安装了Emacs。

然后备份你之前的配置文件

``` 
mv ~/.emacs ~/emacs.conf.bak
mv ~/.emacs.d ~/emacs.d.bak
```
然后clone我的配置

```
git clone git://github.com/iiiyu/.emacs.d.git
```


然后你点开你的Emacs，如果他是这样的颜色并且下面没有提示报错的话。恭喜你。我们开始存活在Emacs中吧。

![Emacs](http://farm8.staticflickr.com/7058/6934750127_d78b255104.jpg)

# 存活

* 启动Emacs 什么都别干
* 阅读Emacs Tutorial（它就再你启动Emacs的那个图片下面第一个，而且如果你的环境是中文的话，它应该是中文的）


### 科普键位
* 大写字母C == Ctrl
* 大写字母M == command（Mac）or alt（PC）
* C-c == 按住Ctrl不放 然后按c
* M-x == 按住command不放 然后按x
* C-x C-f == 按住Ctrl不放 然后按x（此时可以两个键位都放开）再按Ctrl不放 然后按f **小技巧：你可以按住Ctrl一直不放 分别按x键 f键**
* C-M-x == 按住Ctrl和command不放， 然后按x
* RET == 回车
* **高级键位：一般情况下，用Emacs或者Vim的人都会把右边的Ctrl键和Caps Lock键交换位置，这可以大大提高使用Emacs的按键舒适度。强烈建议交换。Mac os在系统设置里面有。Gnome在键盘设置里面也有。Windows据说可以改注册表和软件修改具体要自己google哈。我用的是KBC proker 40%的键盘，硬件就支持了交换。**

### 幸存在Emacs

   当你安装好一个编辑器后，你一定会想在其中输入点什么东西，然后看看这个编辑器是什么样子。但Emacs不是这样的，请按照下面的命令操作：

```
C-x C-f (会在Emacs的底部让你输入你想编辑的文件路径，输入完成以后，Emacs打开一个buffer，然后你就可以再里面尽情的输入文字了。)
C-d 删除一个字符
C-x C-s 保存文件
C-x C-c 退出Emacs
```

```
推荐移动光标方法
C-p 上一行 
C-n 下一行
C-f 下一个字符
C-b 上一个字符
(强例推荐使用其移动光标，但不必需) →你也可以使用光标键 (←↓↑→).
```



你能在Emacs幸存下来只需要上述的那8个命令，你就可以编辑文本了，你一定要把这些命令练成一种下意识的状态。恭喜你，你已经在Emacs里面存活了。