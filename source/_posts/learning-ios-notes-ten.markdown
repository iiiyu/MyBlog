---
layout: post
title: "iOS笔记 (10)"
date: 2012-08-13 14:17
comments: true
tags: iOS
---


# 关于回调函数——Delegate的那些事

## 序

iOS笔记也终于迈入两位数之列。在此里程碑下。明显要拿出点诚意来好好写一写。所以，我决定写一下早已改写的delegate。

## 什么是回调函数


我们先不管delegate，我们先来说说回调函数(callback).[wiki](http://zh.wikipedia.org/wiki/回调函数)的解释在这里。

	回调函数，或简称回调，是指通过函数参数传递到其它代码的，某一块可执行代码的引用。
	
好我来说所自己理解过后的解释:

1. 如果不用回调函数可不可以写程序。(明显可以)
2. 那为什么要用回调函数这种东西。(现代开发软件过程中其实一直在强调一些相同的东西:抽象、复用)
3. 复用是为了提高生产效率。提高生产效率，才能创造更大的价值。
4. 怎么复用——抽象。只用抽象出来的东西才有复用的价值。
5. 从代码量各种角度来看，回调似乎是复用代码了。但是回调不仅仅如此。更大的作用是解耦。
6. 解耦简单说来就是让程序结构更好，更容易读，更容易修改，更容易修改的其中一个基本方法。
7. 回调怎么解耦。A做一把椅子，但是步骤很多。其中一个步骤上漆应该是B来做。因为B是专门做上漆(B有油漆，有刷子，有技术。B持有上漆这个步骤最佳实践)。正确的方法肯定不是A自己去买油漆刷子把这个步骤做了。而是叫B来把这个步骤做好。然后A继续组装椅子。直到椅子做好。 

扯了这么多。其实就是wiki上解释的哪些而已。如果还不明白，再看一次wiki。再多写写代码吧。

<!--more-->

## 那什么是delegate
找了一个比较专业的解释[这里](http://www.uml.org.cn/j2ee/200411036.htm)

对于delegate不是一个具体的技术，它是一个设计模式。他是面向对象编程思想里面的一种基础方法。好如果你看了我的第一个笔记的话，还记得我描述MVC的比喻么[这里](http://www.iiiyu.com/blog/2012/02/28/learning-ios-notes-one/)为了描述方便我还是搬运一次来这里把。
	
	controller一直叫唤狗腿子view，叫view干啥view就干啥。某天，view出事情了，这时候view想
	
	在出事情的时候通知一下controller主子。controller主子也不是没有人性，在controller设置
	
	一个target。这时候要是出现了什么事情view这个狗腿子要发一个action给主子controller就好了。有
	
	时候view狗腿子要和主子controller步调保持一致（synchronize 同步）这个时候要怎么做呢。在
	
	controller里面设置view狗腿的delegate（代表）这个delegate就通过一个protocol来设置
	
	（协议。哈哈 objective-c的语法出现了。）

	view狗腿是不会拥有显示的数据，如果狗腿需要数据的时候数据会通过协议来取得。controllers几
	
	乎都是data source（但不是model），因为controller是把model里面的信息格式化以后给view
	
	的。 那model能直接跟controller交流么？答案是不，model必须独立于UI之外。（好一个不为五
	
	斗米折腰）。 那model有一些忠义之言（信息更新）要进谏给controller怎么办。 model就自己想
	
	办法弄了一个广播电台（类似broadcast mechanism广播机制。PS：学过设计模式的童鞋还hold住
	
	么）controllers 或者其他model就可以“收听”到感兴趣的内容。view可能也有“收听”这个功能，
	
	但是很可能收到的不是model这个台。
	
其实这个故事是根据老头第一课的视频加keynote自己联想出来的。来回顾一下这张经典的keynote
![keynote](http://farm9.staticflickr.com/8448/7780946470_05ee760924.jpg)

1. 这个是iOS中MVC的最好诠释
2. 上面把iOS里面的回调方式常用的都画出来了。只差一个block样式的现代接口。(理论上来说，我应该会把每一个回调都写一篇blog，尽请期待)

所以，在我理解中MVC三者都是很独立自主的。他们之间的联系就靠这些回调来进行沟通。

1. target action
2. delegate & data source
3. Notification & KVO

根据我这几个月以来的编写感悟而言，这张keynote真的太经典了。delegate从view回来一般是三个函数(will should did)都是在view的不同阶段进行回调都是很有用的时候。比如view将要显示的时候(带有will的delegate函数)。已经完成了的显示的(带有did的函数)。所以delegate是什么时候用。

就是view狗腿在可能主子Controller需要知道的时候设置了一种通知方式。可以想象成，从大殿之外飞奔进来一个狗腿说："报~~~我准备去抢花姑娘了。圣上有何吩咐"。controller说:"带点长腿 A罩杯的回来" 。然后狗腿飞奔出去办事。过了一会狗腿又飞奔进来:"报~~~~抢花姑娘回来了，圣上有何吩咐"。controller说:"带到xx宫，洗干净等我"。狗腿的报告主子是可以不理会的。(这句主要是为了解释protocol里面的optional关键字)这个例子可能比较恶趣味+不怎么清楚。但是技术的东西我们还是弄娱乐点吧。


## 实现delegate

ok，说了这么多的东西。我主要想解释为什么会有delegate这种东西。它的应用场景。接下来，我会借助这些文章:

[文章1](http://marshal.easymorse.com/tech/objc-委托模式)

[文章2](http://haoxiang.org/2011/08/ios-delegate-and-protocol/) 

[文章3](http://popcornylu.blogspot.com/2011/07/delegate.html) 

来上面的例子来解释怎么用delegate。


对于delegate不是一个具体的技术，它是一个设计模式。这是我上篇开头说的。那objective-c当中怎么实现delegate呢。用protocol。

一般会这样写

狗腿.h

``` 
@protocol MyClassDelegate <NSObject>
@optional
- (void) will抢花姑娘:(MyClass*)myClass;
- (void) did抢花姑娘:(MyClass *)myClass;
@end

@interface MyClass
{
    id<MyClassDelegate> delegate;
    FlowerGirl *flowerGirl;
}
@property (nonatomic, weak) delegate;
@property (nonatomic, strong) flowerGirl;
@end
```

这里为什么要用weak。[这里](http://popcornylu.blogspot.com/2011/07/delegate.html)有详细的分析。自备云梯

还有一个注意的地方是optional这个key，如果没有这个key，那成为这个狗腿的主子都要把这些方法实现了(比如xxx侵犯了我朝领土，请圣上发令。这种肯定要处理，不处理的话就是臭sb。不配做老大)。而有这个key的话，在这个key(上面的方法一定实现)，key下面的方法是可选的不必一定实现(比如抢来花姑娘，可以不去做任何动作)。

这样，我们就完成了狗腿的delegate声明。

狗腿.m

``` 

@implementation MyClass

-(void) 抢花姑娘
{
	    if ([self.delegate respondsToSelector:@selector(will抢花姑娘:)]) {
        [self.delegate will抢花姑娘:self];
    }
    // do something 出去抢花姑娘
    // 并且抢到了
    
    	if ([self.delegate respondsToSelector:@selector(did抢花姑娘:)]) {
        [self.delegate did抢花姑娘:self];
    }
    
    
}

@end
```

这里要注意的是[self.delegate respondsToSelector:xxx]方法。这个方法用于检验主子有没有实现xxx方法，如果实现了，在做xxx。没有没有实现的话就不做xxx了。如代码所示.

主子.m

``` 
@interface 主子()<MyClassDelegate>{
    MyClass *myClass;
}
@end

@implementation 主子
- (void)init
{
   ...
   myClass = [MyClass new];
   myClass.delegate = self;
   [myClass 抢花姑娘];
   ...
}

- (void) will抢花姑娘:(MyClass*)myClass
{
    myClass.flowerGirl = flowerGirl(腿长，A);
}

- (void) did抢花姑娘:(MyClass *)myClass
{
    [self 送去xxx宫:myClass.flowerGirl];
}

@end
```

具体就是这样。代码是伪码一般，只是写着写着就想着对应我的恶趣味例子。鄙视一下自己。


## 总结
我觉得我扯了这么多，包括贴出来的文章地址看完以后，你应该对delegate有一个感性的认识了把。如果你对blog内容有满意or不满意和错误的地方。欢迎指出。我一定改进。谢谢。



















	
