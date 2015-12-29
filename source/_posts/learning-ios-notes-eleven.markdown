---
layout: post
title: "iOS笔记 (11)"
date: 2012-09-02 11:29
comments: true
tags: iOS
---

# 关于Target-Action设计模式

## 序
上回提到了在iOS的MVC中。delegate其实是一种View跟Controller直接交流的方式。

![keynote](http://farm9.staticflickr.com/8448/7780946470_05ee760924.jpg)

这次我们来说Target Action这种View跟Controller的交流方法。

<!--more-->

## 什么是selector
官方解释请看[这里](http://developer.apple.com/library/ios/#documentation/cocoa/conceptual/objectivec/Chapters/ocSelectors.html)

在Objective-C里面, selector有两种意思:

1. 当用源码向一个对象发送消息的时候,表示简单的方法名字。
2. 作为一个方法的唯一标识符去替换源码编译的时候的方法。

突然觉得我再怎么写也没有官方文档写的好。那就看文档把。

总结几个我理解以后的要点


1. SEL是一种类型，跟int float一样。
2. @selector 其实就是取得一个SEL。


``` objc
SEL   press;
press = @selector(pressed:);
UIButton*  btn = [UIButton alloc]init];
[btn addTarget:self action:press forControlEvents:UIControlEventTouchUpInside];
-(void)pressed:(id)sender
{
    .....
}
```
3. @selector()取得的其实一个ASCII的方法名字符串
4. 我们可以通过performSelector:, performSelector:withObject:, performSelector:withObject:withObject: 这三个方法去调用selector


``` objc
id   helper = getTheReceiver();
SEL  request = getTheSelector();
[helper performSelector:request];
```

## 什么是Target-Action设计模式
官方解释在[这里](Target-Action设计模式)
target-action是个设计模式。对象保持必要的信息，当事件发生的时候发送消息给其他对象。所保持的信息有两部分数据组成：

* action selector，定义要调用的方法名称标识；
* target，接收消息的对象。


当被称作action message的事件发生，将向target发送action selector定义的方法消息。

target-action模式一般用于自定义的controller按照应用规范定义的方式处理action message。

![ta](http://developer.apple.com/library/ios/documentation/general/conceptual/Devpedia-CocoaApp/Art/target_action.jpg)

## 怎么用Target-Action

### UIButton用法
在iOS里面有些控件是继承UIControl这个类的。比如UIButton,理论上只要继承这个UIControl这个类的子类都可以用Target-Action。

```
- (void)addTarget:(id)target action:(SEL)action forControlEvents:(UIControlEvents)controlEvents
```

UIControl的子类都可以覆盖这个方法，来调用自己的事件对应的函数。

``` objc
UIButton *btn = [UIButton buttonWithType:UIButtonTypeInfoLight];
[btn addTarget:self action:@selector(method:) forControlEvents:UIControlEventTouchUpInside];
[view addSubview:btn];

- (void)method:(id)sender
{
// do something
}
```


解释一下addTarget:self。就是把要去哪里查找实现方法的目标。action:@selector(method:)就是把动作要执行什么方法。forControlEvents:就是要添加的事件是什么。而- (void)method:(id)sender这里的sender就是发出Event的这个东西。在这里就是我们的这个UIButton。

简单的用@selector还有另外一个应用场景就是Notification。这个另外写一篇把。
























