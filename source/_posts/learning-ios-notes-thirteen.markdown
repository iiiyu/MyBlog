---
layout: post
title: "iOS笔记 (13)"
date: 2012-10-21 17:50
comments: true
tags: iOS 
---


# UIViewController的一些事儿

## 序
这篇主要是想写 Contaner View Controller。然后才好写我计划里面的下一篇讲UIStoryboardSegue的blog。

先贴一个链接 [Container View Controller](http://geeklu.com/2012/05/custom-container-view-controller/).他写的很好，算是中文blog里面我见过写 Container View Controller最好的一篇blog。

另外一篇中文blog[UIViewController的误用](http://www.onevcat.com/2012/02/uiviewcontroller/)

然后是apple的官方文档

[UIViewController Class Reference](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UIViewController_Class/Reference/Reference.html)

[View Controller Programming Guide for iOS](http://developer.apple.com/library/ios/#featuredarticles/ViewControllerPGforiPhoneOS/Introduction/Introduction.html)

<!--more-->


## 什么是UIViewController
![keynote](http://farm9.staticflickr.com/8448/7780946470_05ee760924.jpg)

UIViewController就是MVC模型在iOS当中对应Controller的具体类型。最基础普通的情况下，每一个屏就要对应一个UIViewController的子类。也就是说你向storyboard拖入一个view controller的时候。你还必须新建一个接管你这个storyboard的UIViewController子类。然后把他们两个连接起来。这样就可以获得了一个MVC里面的VC了。然后这个UIViewController其实是管理着一组View Hierarchy。因为你的屏幕上可能有各种控件。每一个控件都是一个View。他们构成了一组View Hierarchy。

so这个UIViewController的子类控制着这组View的行为。包括View的显示，消失，改变，动画等等。
这种说明有点抽象了，来点简单的。UIViewController就是View的主子。一个ViewController应该且只应该管理一个view hierarchy，而通常来说一个完整的view hierarchy指的是整整占满的一个屏幕。

之前有一篇讲过了一点View Controller生命周期的blog[iOS笔记(5)](http://www.iiiyu.com/blog/2012/04/03/learning-ios-notes-five/)。可以看看。



## 什么是Contaner View Controller

![Contaner View Controller](http://developer.apple.com/library/ios/featuredarticles/ViewControllerPGforiPhoneOS/Art/navigation_interface_2x.png)

通常情况下，我们的App不会只有一个View Controller这么简单。会有很多个View conroller。我们不能让我们的App里面的View Controller杂乱无章。我们需要一个东西来能够管理这些View Controller。这个东西就叫Contaner View Controller。Contaner View Controller也是继承于UIViewController。这里我们可以得到两个信息: 

1. UIViewController其实可以自己管理自己。我们就可能自己写Contaner View Controller。
2. 加上上图的描述。Contaner View Conroller（Navigation Controller）其实是一直存在的。

### 常见的Contaner View Controller
其实常见的Contaner View Controller也就两个UINavigationController和UITabbarController。他们肯定都有一些特征：(摘抄第一篇blog)

1. 提供对Child View Controller进行管理的接口,比如添加Child View Controller,切换Child View Controller的显示,移除Child View Controller 等
2. 容器“拥有”所有的Child View Controller
3. 容器需要负责 Child View Controller的appearance callback的调用(viewWillAppear,viewDidAppear,viewWillDisaapper,viewDidDisappear),以及旋转事件的传递
4. 保证view hierarchy 和 view controller hierarchy 层级关系一致,通过parent view controller将child view controller和容器进行关联

其实如果我们进行一般的iPhone App快速开发。这两个Contaner View Controller已经足够用了。

## UINavigationController
简单的UINavigationController如系统的Setting。

![Setting](http://developer.apple.com/library/ios/documentation/uikit/reference/UINavigationController_Class/Art/navigation_interface.jpg)

很简单。就是一个满屏一个满屏的推进，然后回退。学过程序的你看到那些描述也很亲切。可以简单理解为数据结构中的栈的图形方式。Push为进栈，Pop出栈。你还可以Pop全部已经压入的数据。

当然UINavigationController肯定要比一个栈复杂的多。下图是UINavigationController的层次结构

![The views of a navigation controller](http://developer.apple.com/library/ios/documentation/uikit/reference/UINavigationController_Class/Art/NavigationViews.jpg)

肯定的，我们在相应的场景里面可以选择把Navigation bar或者 Naviagtion toolbar设置为显示或者不显示。这完全取决于你的设计。


## UITabbarController
和UINavigationController不同，UITabbarController使用的场景更可能是不太相关连的View Controller组合到一起。而UINaviagtionController强调的是层层递进的关系。
![The tab bar interface in the Clock application](http://developer.apple.com/library/ios/DOCUMENTATION/UIKit/Reference/UITabBarController_Class/Art/tabbar_compare.jpg)

所以，UITabbarController管理下的View Controller所做的操作是切换，而不是Push。

UITabbarController的层次结构
![The primary views of a tab bar controller](http://developer.apple.com/library/ios/DOCUMENTATION/UIKit/Reference/UITabBarController_Class/Art/tabbar_controllerviews.jpg)

## 后记
当然UINavigationController和UITabbarController是可以放到一起使用的。UINavigationController代表的纵向的View Controller推进方向。UITabbarController代表着横向的View Controller切换方向。 

喜欢这些碎碎叨对你有用。欢迎批评指正错误。thx。









