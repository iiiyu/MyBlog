---
layout: post
title: "iOS笔记 (2)"
date: 2012-03-05 00:25
comments: true
tags: iOS
---

此笔记是观看斯坦福的IOS开发课程和自己思考以后的产物，如果有所偏差，还望指导更正。

## iOS的MVC初级代码说明

上次说到MVC模式。那些都是理论。真正离写代码终究还是很大差距。特别是Xcode4 IOS5以后，变化很大，中文资料几乎全部过时。经过反复看了斯坦福老头的课程。终于有所顿悟。记录下来。

斯坦福老头的课程前面12课都没有说道model。全部是view和controller。我也只是看到12课而已。所以现在只讨论IOS5开发中的view和controller。

IOS5中，我们可以看到一个叫storyboard的文件。这个文件就是view。或者说是view的载体。UIKit上所有的控件都可以往上面拖。这里我简单的把storyboard以及带UI前缀的类都统一看成view。view就是再ios设备上你能看见的整个屏幕。

<!--more-->

在最简单的例子里面，我们可以看到一对叫XXXViewController.h XXXViewController.m的文件。他们就是一个Controller类。

现在回忆一下view和controller的关系

>controller一直叫唤狗腿子view，叫view干啥view就干啥。某天，view出事情了，这时候view想在出事情的时候通知一下controller主子。controller主子也不是没有人性，在controller设置一个target。这时候要是出现了什么事情view这个狗腿子要发一个action给主子controller就好了。有时候view狗腿子要和主子controller步调保持一致。这个时候要怎么做呢。在controller里面设置view狗腿的delegate（代表）这个delegate就通过一个protocol来设置 

在老头的视频教学里面我们可以看到。比如一个UILabel拖到storyboard上面的时候，这个UILabel仅仅是一个view，它和controller还没有产生任何关系。此时我们的主子controller要知道他有了一个狗腿UILabel这个view要怎么办呢。我们就按着Ctrl点击UILabel拖入XXXViewController.h文件里面，形成一个outlet。


``` objc
@property (weak, nonatomic) IBOutlet UILabel *display;
```

这里我理解为狗腿子跟主子报告了一声，说hi 我在这里。

这时候狗腿子说，hi 主子。我出了个事情要跟你汇报。就是视频里面按住Ctrl然后点UI控件会出来一个框里面有很多事件。然后点点击事件不放向
XXXViewController.m文件里面拖，就会形成一个Action。

```
- (IBAction)digitPressed:(UIButton *)sender 
{
	...
}
```

这就是狗腿出什么样的事情通知主子的地方。


## 一些小细节上的感悟

#### ARC和strong、weak

新的IOS5引入了ARC（Automatic Referencing Counting）机制。当然，你可以在创建项目的时候不选择，或者再项目创建以后关闭它来手动释放内存。

可惜我直接是从IOS5开始学习，而且不喜欢旧的东西。所以坚决使用ARC和storyboard。而且再内存管理上，我觉得编译器比人靠谱多了。当然我也是初学IOS。理解上可能有错误，欢迎指导。

窃以为使用ARC，一开始接触到的是：

	strong
	weak
	
在我的记录中老头是这样定义的：
> Strong and weak are descriptions or attributes parentheses.
> Strong keep it in the heap. Keep it in heap until i stop pointing to it.
> Weak keep this as long as someone else point to it strongly.

值得注意的是
>Strong and weak. This is not garbage collection.
>This is refernce counting done automatically for you.

大概就是strong声明的东西是放到内存堆里面,它在我停止指向它之前都一直会在内存堆里面，引用记数也不会为0（猜测）。weak呢，则是只要没有指向它的东西时候，引用记数就为0了。然后呢，他们都只管理引用记数这个一亩三分田。至于垃圾回收这种工作，是不该他们管理的。

#### 声明初始化类

一句话:

alloc后面永远跟着init

```
[[CalculatorBrain alloc] init];
[[NSMutableArray alloc] init]
...
```

#### 面向对象的封装

小时候还在用过几天java写web的时候，很纳闷为什么每一个属性都要写get、set方法。直到现在才慢慢明白这是封装。封装的好处。

在IOS5编程里面。通常是用@property来声明属性，然后使用@synthesize来默认使用get、set方法。当然，再objective-c里面可以重写get、set方法。而且我觉得@property和@synthesize应该像alloc和init一样配套使用。

```
@property (nonatomic, strong) NSMutableArray *operandStack;


@synthesize operandStack = _operandStack; 
```

#### 编译器的强大

不管是ARC还是后面的get、set方法。都是再编译的时候编辑器加上去的。（猜测）然后各种听说Apple为了让App再IOS跑的更好更流畅。一直再不停的优化llvm。这也是搭载Android的设备硬件远超过IOS设备的情况下还是没有IOS用着爽的底层原因之一吧。 所以，我觉得应该百分百相信编译器。如果出错了。肯定是程序写的有问题。
