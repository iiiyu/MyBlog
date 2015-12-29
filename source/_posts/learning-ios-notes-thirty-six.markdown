title: iOS 学习笔记 (36)  ReactiveCocoa 用 RACSignal 替代 Delegate
date: 2014-12-26 18:36:31
tags: iOS
---

本文仅作为个人学习记录使用,也欢迎在[许可协议](http://creativecommons.org/licenses/by-nc/4.0/deed.zh_TW)范围内转载或使用，请尊重版权并且保留原文链接，谢谢您的理解合作。如果您觉得本站对您能有帮助,您可以使用[RSS](http://iiiyu.com/atom.xml)方式订阅本站,这样您将能在第一时间获取本站信息.


## 开篇扯淡

最近又在看  ReactiveCocoa 了（下面用 RAC 来替代 ReactiveCocoa）。虽然依然是 hello world 级别。但是 hello world 也是可以分级别的。这次自我感觉是一个偏向中级的 hello world。

我们先来张图：

![](http://ww4.sinaimg.cn/large/a6d3226bgw1enpq6o5sz5j20ai09uglz.jpg)

在 RAC 的文档和一些介绍 RAC 的 Keynote 资料里面我们可以看到说 RACSignal 可以来替代 Delegate、 Block Callbacks、Target Action、KVO、Notifications。

但是貌似没有一种 hello world 的方式来进行说明如何替代的。

插嘴:在中文 blog 里面见过几个写 RAC 的比较好哒。一个是[limboy大大](http://limboy.me)的几篇深入浅出令人叹为观止，李忠大大不但研究透彻了然后还结合自己的实战经验写成很好的文章来分享。 另一个是[sunnyxx的Reactive Cocoa Tutorial系列](http://blog.sunnyxx.com/tags/Reactive%20Cocoa%20Tutorial/)这个系列比较偏向研究 RAC 是如何实现和工作的。

我这个人比较笨，最喜欢写 hello world。那就找时间一个一个来写呗。

写之前 Google 了一下。所以以下内容大量参考:[Replacing the Objective-C “Delegate Pattern” with ReactiveCocoa](http://spin.atomicobject.com/2014/02/03/objective-c-delegate-pattern/)。能看原文就去看看。然后忽略掉以下的 hello world 就好了。

<!--more-->


## 实现功能说明

本来想改成 TableView 的。改着改着感觉 TableView 的话。可能会牵扯到 MVVM 的问题。 才能架构出来一个正确的程序结构。而我只想说明简单的写清楚如何替代Delegate。所以相当于一个中文简化版本的 [Replacing the Objective-C “Delegate Pattern” with ReactiveCocoa](http://spin.atomicobject.com/2014/02/03/objective-c-delegate-pattern/)了。

那就跟他一样写搜索把。然后实现过程中发现 iOS 8 用新的 UISearchController 来替代了 UISearchDisplayController 了。 

## UISearchController Delegate 常规实现
一般来说我们会设置protocol.

```
    self.searchController.searchResultsUpdater = self;
    self.searchController.delegate = self;

```

然后去委托的类里面实现相关的方法

```
#pragma mark - UISearchResultsUpdating

- (void)updateSearchResultsForSearchController:(UISearchController *)searchController
{
    if (searchController.searchBar.text.length > 0) {
        self.searchResults = [self search:searchController.searchBar.text];
    } else {
        self.searchResults = self.searchTexts;
    }

    [self.tableView reloadData];
}
```

```
#pragma mark - UISearchControllerDelegate

- (void)willPresentSearchController:(UISearchController *)searchController
{
    self.searching = YES;
}

- (void)willDismissSearchController:(UISearchController *)searchController
{
    self.searching = NO;
    [self.tableView reloadData];
}
```

普通情况下我们就是这样来使用 Delegate 的。

平淡无奇。下面我们来用 RACSignal 的实现方法。

## UISearchController Delegate RACSignal 实现

第一个要明确的是：我们要做什么。

### 常规模式

根据常规代码来看:

1. 我们需要在每次输入词变化的时候进行搜索。
2. 需要在进入和退出搜索的时候知道当前状态

1对应的就是实现- (void)updateSearchResultsForSearchController:(UISearchController *)searchController

2对应的是实现- (void)willPresentSearchController:(UISearchController *)searchController 和 - (void)willDismissSearchController:(UISearchController *)searchController。

现在，让我们来转换为 RAC 的思维模式思考问题。 

### RAC 模式

1. UI 上需要搜索结果的 NSArray
2. 搜索结果由搜索关键字得来。
3. 每次修改关键字都应该更新搜索结果。

因此我们要想办法吧 UI 上需要的数据和修改关键字这个动作绑定起来。 

同理可以很容易想到。我们也需要把当前 UI 是否处于搜索状态跟会改变搜索状态的动作绑定起来。 

要怎么绑定呢？ 拥有刚刚 RAC 超过 Hello World 实力的我，想到，我需要构建出来两个RACSignal。

然后进行类似：

```
RAC(self, searchResults) = SignalA;
RAC(self, searching) = SignalB;
```

这样的绑定就皆大欢喜了。 

主要用到了下面两个 RAC 的方法：

**rac_signalForSelector:fromProtocol:**

这个方法主要是把 protocal 转为一个 Signal 便于使用。值得注意的是这个函数返回的是一个 RACTuple。 这个 RACTuple 包含了 Selector 方法里面所有的参数。这样需要用到的时候主要按照顺序来获取。

**rac_liftSelector:withSignalsFromArray:**

这个方法它的意思是当传入的 Signals 都至少sendNext过一次，接下来只要其中任意一个signal有了新的内容。就会去触发第一个 selector 参数的方法。


构造两个 Signal 的代码如下 

```
//  UISearchController+RAC.m

- (RACSignal *)rac_textSignal
{
    self.searchResultsUpdater = self;
    RACSignal *signal = objc_getAssociatedObject(self, _cmd);
    if (signal != nil) {
        return signal;
    }

    signal = [[self rac_signalForSelector:@selector(updateSearchResultsForSearchController:) fromProtocol:@protocol(UISearchResultsUpdating)] map:^id(RACTuple *tuple) {

        UISearchController *searchController = tuple.first;
        return searchController.searchBar.text;
    }];

    objc_setAssociatedObject(self, _cmd, signal, OBJC_ASSOCIATION_RETAIN_NONATOMIC);
    return signal;
}

- (RACSignal *)rac_isActiveSignal
{
    self.delegate = self;
    RACSignal *signal = objc_getAssociatedObject(self, _cmd);
    if (signal != nil) return signal;

    RACSignal *willPresentSearching = [[self rac_signalForSelector:@selector(willPresentSearchController:) fromProtocol:@protocol(UISearchControllerDelegate)] mapReplace:@YES];
    RACSignal *willDismissSearching = [[self rac_signalForSelector:@selector(willDismissSearchController:) fromProtocol:@protocol(UISearchControllerDelegate)] mapReplace:@NO];
    signal = [RACSignal merge:@[willPresentSearching, willDismissSearching]];
    objc_setAssociatedObject(self, _cmd, signal, OBJC_ASSOCIATION_RETAIN_NONATOMIC);
    return signal;
}
```

最终绑定代码如下：

```
//  RACMasterViewController.m

    RAC(self, searchResults) = [self rac_liftSelector:@selector(search:) withSignalsFromArray:@[self.searchController.rac_textSignal]];
    [self.searchController.rac_textSignal subscribeNext:^(id x) {
        [self.tableView reloadData];
    }];
    RAC(self, searching) = [self.searchController rac_isActiveSignal];
```

这样我们就写完了一个用 RAC 来替代 Delegate （protocol） 的例子


## 总结

使用 RAC 其实最重要的思维的转变。 这个转变在写代码的时候如果我没有思考的很清楚。 那我写出来就一团乱麻。还是需要多加锻炼 MVVM 的思维。

[实例代码已经上传Github](https://github.com/iiiyu/RACSignalDemo)

下集预告 用 RACSignal 替代 Block Callbacks。有人会期待么？

## 资料

1. [limboy大大](http://limboy.me)
2. [sunnyxx的Reactive Cocoa Tutorial系列](http://blog.sunnyxx.com/tags/Reactive%20Cocoa%20Tutorial/)
3. [Replacing the Objective-C “Delegate Pattern” with ReactiveCocoa](http://spin.atomicobject.com/2014/02/03/objective-c-delegate-pattern/)
4. [ReactiveCocoa Lessons Learned](https://speakerdeck.com/robpearson/reactivecocoa-lessons-learned)