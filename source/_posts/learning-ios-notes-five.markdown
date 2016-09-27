---
layout: post
title: "iOS笔记 (5)"
date: 2012-04-03 23:31
comments: true
tags: iOS
---

# iOS笔记——ViewController的生命周期


## 生命周期

这个宇宙中，我们认知当中所有事物都是有一个起点然后到达一个终点。在四维的世界里面，衡量的介质就是时间。所以我们写的程序也是一样的，不管是C还是iOS程序里面，每一个东西在时间流逝中它都有自己的一个起点，终点。

了解程序里面大部分东西的起点和终点的意义是，我们想在这个东西诞生时候和结束的时候做一些事情，一些是我们自愿加入的(比如初始化一个美女的图片)，有些是不得不做的(比如指向这个图片已经销毁，但是这个指针你没有赋值nil，其他的地方还在调用它，就会出错)。

<!--more-->

## ViewController的生命周期中的过程和方法

### viewDidLoad
在UIViewController初始化完毕和outlets进行hooked完成之后，第一个调用的是viewDidLoad方法。

``` objc
- (void)viewDidLoad;
```

我们可以把大部分初始化需要做的事情通过重载viewDidLoad来完成。仅仅需要小心的事情只有一个就是如果你要设置view的大小(比如:bounds)你应该使用下面这个方法。

### viewWillAppear

``` objc
- (void)viewWillAppear:(BOOL)animated;
```

viewWillAppear这个方法在你的view正要显示的时候调用，因为此时view已经初始化了，但是没有显示出来。如果想要改变view大小，view的排列，view的形状。重载这个方法是很好的时机。

viewWillAppear还适合做另外的一个事情——lazily to stuff。(没听太懂大概是在初始化时候做的一些事情)

如果viewWillAppear里面要做一个耗时很长的事情，这时候，我们一应该开一个线程去做，而且不是这个主进程在阻塞等待。然后再view上显示一个风火轮(spinning wheel)

viewWillAppear适合做的两件事情：

* 在显示view的最后时刻加载一些高开销的操作
* 改变view的几何特性

### viewWillDisappear
viewWillDisappear是view将要从屏幕消失的时候调用的方法。这时候你可能想要记住一些view上此时的状态，比如真正浏览的女神图片的网址或者一些你想稍后恢复NSUserDefaults用户的设置。

老头说在这个方法下他有两种比较倾向的做法:

* 另开进程处理UI的变化
* 把所有的配置写入长期存储器中存储

第一个理由很简单，为了更快更好的用户体验，第二个也是，可以保存用户的数据，防止意外发生。

### viewDidAppear viewDidDisappear
这两个方法对应上面的两个方法。他们都在view显示之后调用。老头的举例是说view出现以后你想有一个动画，这时候就可以在这两个方法里面调用。

不管是Wills的方法还是Dids的方法。想要调用时候都是用super，例如

``` objc
- (void)viewWillDisappear:(BOOL)animated
{
	[super viewWillDisappear:animated]; // call super in all the viewWill/Did... methods // let’s be nice to the user and remember the scroll position they were at ...
￼￼	[self rememberScrollPosition]; // we’ll have to implement this, of course // do some other clean up now that we’ve been removed from the screen
	[self saveDataToPermanentStore]; // maybe do in did instead?
// but be careful not to do anything time-consuming here, or app will be sluggish
// maybe even kick off a thread to do what needs doing here
}
```

### view{Will,Did}LayoutSubviews;
frame改变之前会调用viewWillLayoutSubviews(例如，横向iPhone时候frame就改变了。)

同理可得viewDidLayoutSubviews是frame改变之后调用。

这样我们就可以根据不同的屏幕状态来调整显示，以便获得最佳效果。

### viewDidUnload
如果iOS中内存过低，这时候，系统会自动的卸载view。意味着系统把viewController从内存里面干掉，也就是停止所有指向viewController的strong类型的指针。做完这些以后，viewDidUnLoad会被调用。也就是self.view设置为nil时候调用。

直觉上来说，这时候貌似不用做什么事情。可是为了最佳实践。虽然你的outlets都是weak类型的。但是你还是需要手动在viewDidUnload把它们设置为nil。这起码算是一种好的iOS编程习惯,我们应该把所有的outlet都设置为nil。


```
- (void)viewDidUnload
{
    self.faceView = nil;
}
```

如果你还是想知其然：大概就是可能在其他地方，有个指向这里某个outlet的strong类型的指针。根据ARC，这时候你的outlet是已经释放了。但是那个strong指针仍然指向了之前的那个内存地址，如果这时候使用，内存就会错误，而设置了为nil以后，向nil是可以发送任何消息，不会错误的。(我是这样理解，可能跟现实有出入。欢迎指正。)

随便说一声，不管内存有多低，正在现实的view是不会被unload的。所以只是针对屏幕以外的viewController进行unload。











