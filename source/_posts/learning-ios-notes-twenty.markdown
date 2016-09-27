title: iOS笔记(20)
date: 2013-04-15 14:02:11
tags: iOS
---

# UITableView简单解析

## 序

UITableView是在iOS开发中，展示大量内容的首选。我个人认为的原有有一下几点：

1. UITableView的展现形式是为移动设备专门设计过的。有较好的人机交互体验。
2. 从技术角度来说UITableView具有重用和延迟加载等特性。如果使用恰当。可以获得一个App流畅的用户体验。

这样，使得UITableView在iOS App中随处可见。

原生应用

![](http://ww4.sinaimg.cn/large/a74ecc4cjw1e3q5jk1wrtj.jpg)


一些有名的App.图片信息较老

![](http://ww4.sinaimg.cn/large/a74eed94jw1e3q5kl91luj.jpg)


包括游戏

![](http://ww1.sinaimg.cn/large/a74e55b4jw1e3q5lb59vwj.jpg)



这些都说明UITableView在一个App中其实是一个很常用的控件。我应该好好的学习它。

<!--more-->


## 关于数据的思考

### 没有UITableView的时候我是这样想的

首先思考为什么会有UITableView这样的控件。我们做一个App的时候，就会有大量的数据需要显示。比如weibo的每一个状态。比如一个新闻App的很多条新闻。这些数据都会有一个特点就是他们的组织形式一样，只是内容变化。有时候我们可能会根据一些条件进行分组。使得看来了是分组的。例如：联系人里面会按照首字母来进行分组一样。我们还可能会点击数据以便查看更详细的内容。通过上面的简单描述，如果来自己实现一个类似UITableView的结构。需要得到最核心的：

1. 需要得到一共多少条数据
2. 数据的具体内容是什么

如果我们数据需要更加仔细的描述展示：

1. 全部的数据一共有多少组
2. 每一组有多少个数据
3. 每一条数据的具体内容是什么

### UITableView是怎么做的
在UITableView中。最重要的就是data source中的两个方法。

```
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section;

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath;
```

什么是data source。字面意思就是很明白，数据的来源。一般情况下我们会设置拥有UITableView的这个UIViewController为他的data source。因为根据MVC来说。UITableView是View，UIViewController是Controller。View需要的数据，应该是Controller去跟Model协调然后获得，以后由Controller去给View来进行显示。View永远的不去直接跟Model联系。这样当UITableView初始化的时候。他就会去问他的data source。我需要显示多少行数据啊。每一行的数据都是什么内容啊。这时候UIViewController应该已经从Model拿到了数据。然后通过- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section;告诉UITableview，恩你的这一组要显示n条数据。又用- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath函数告诉UITableView说，第几组第几条数据的具体内容是什么。


UITableView还有一个比较犀利的地方就是如果你的数据有10000条。它肯定不是把10000条都加载进来。而是只加载需要显示的条目数据。这样设计，使得UITableView的流畅程度大大提高。值得注意的是，如果Cell里面的数据是从网络 or Core Data等其他地方读取的。我们应该把读取动作写成异步的。不阻塞主线程。取到数据以后在回答主线程去刷新UI。


UITableView从创建到显示的调用顺序如下图

![](http://ww4.sinaimg.cn/large/a74ecc4cjw1e3sfatxy0gj.jpg)


- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView这个函数是最先调用，但是它默认返回1.所以并不是必须的。如果你的UITableView分了好几组，这个就是用来返回组的数量的。


这样完成了设置UITableView的data source设置。在完成

```
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section;

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath;
```

这两个函数。我们就可以得到一个能显示的UITableView了。

现在是是仅仅解释了UITableView的数据是怎么来的，然后怎么对应到UITableView上面。


## UITableViewCell

UITableViewCell具体的每一条数据展示的具体View。首先在UITableView里面有这样一个特点，每一个Cell的大概样子都长的差不多，只是里面具体的内容稍有变化。这样在UI里面我们应该是重用相同的部分，改变不同的部分。这样才能提高效率。因为在UiTableView这个视图里面，用户习惯性快速的滚动，视图和数据内容都会快速的变化，如果效率问题处理不好，很容易有卡顿的现象。造成用户体验的降低。

如果使用默认的UITableViewCell风格，有以下四种


UITableViewCellStyleDefault
	
![](http://ww2.sinaimg.cn/large/a74eed94jw1e3sha1cl2oj.jpg)

UITableViewCellStyleSubtile

![](http://ww4.sinaimg.cn/large/a74e55b4jw1e3shcdevpbj.jpg)

UITableViewCellStyleValue1

![](http://ww1.sinaimg.cn/large/bfadf3bejw1e3shcssq55j.jpg)

UITableViewCellStyleValue2

![](http://ww4.sinaimg.cn/large/a74ecc4cjw1e3shdbgbrgj.jpg)

当然如果是需要自定义Cell也是很简单。 用xib拖一个，完全可以GUI的方式来创建，很是方便。

(Google一个栗子吧，写不动了)


插一句，Cell的重用是需要下面这样的实现方法

```
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    static NSString *Cell = @"MyCell";
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:Cell];
    if (!cell) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:Cell];
    }
    return cell;
}
```


重要的是UITableView的dequeueReusableCellWithIdentifier方法。

dequeueReusableCellWithIdentifier去一个队列里面需找有没有相同ID的的Cell。如果有就提出来重用。可以重用的部分。如果没有就跳进if里面去创建。所以我们在if里面创建的时候，不会改变的内容都可以在里面创建，这样就只用创建一次。需要改变的内容我们就放到if后面去写。 这样我们就能完成高效的UITableView。当然，理论上来说，你可以不用这样的机制，而去直接每次创建一个Cell。不过这是非常浪费资源的一个做法，直接不提倡。




## UITableView Delegate
当然，在写UITableView肯定想控制的更多。才能完成设计师们辛辛苦苦画出来的稿。这样我们可以去看看Data source里面剩下的函数和Delegate。

当然就说一点

```
//选中Cell响应事件
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
{
    //选中后的反显颜色即刻消失
    [tableView deselectRowAtIndexPath:indexPath animated:YES];
}
```

这个是选中Cell时候会的出发点。如果要点击以后做什么事情 就在这里做了。 

[找了篇目测还ok的中文blog可以先看看了解](http://www.devdiv.com/home.php?mod=space&uid=39974&do=blog&id=2573)


## 数据刷新

如果我们的modle更新了。相应的要体现到UITableView上面。简单的我们可以reload整个TableView。这样做很方便，而且数据上没有问题。唯一的问题就是，reload整个TableView的效率太低了。而且，往往我们只是少数的Cell内容变化。所以没有必要去reload整个TableView。而是那条数据变化去刷新对应的Cell就好了。这样做效率提高很多。

具体涉及到的几个函数

```
- (void)beginUpdates;
- (void)endUpdates;
 
- (void)insertSections:(NSIndexSet *)sections withRowAnimation:(UITableViewRowAnimation)animation;
- (void)deleteSections:(NSIndexSet *)sections withRowAnimation:(UITableViewRowAnimation)animation;
- (void)reloadSections:(NSIndexSet *)sections withRowAnimation:(UITableViewRowAnimation)animation;
 
- (void)insertRowsAtIndexPaths:(NSArray *)indexPaths withRowAnimation: (UITableViewRowAnimation)animation;
- (void)deleteRowsAtIndexPaths:(NSArray *)indexPaths withRowAnimation: (UITableViewRowAnimation)animation;
- (void)reloadRowsAtIndexPaths:(NSArray *)indexPaths withRowAnimation:(UITableViewRowAnimation)animation;
```

[这里有实例代码可以参考](http://www.cnblogs.com/smileEvday/archive/2012/06/28/tableView.html)


[官方教程](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/TableView_iPhone/ManageInsertDeleteRow/ManageInsertDeleteRow.html#//apple_ref/doc/uid/TP40007451-CH10-SW1)


里面写了如何优化UITableView：

[又找了一篇blog写的很不错的样子，优化UITableView性能](http://www.keakon.net/2011/08/03/优化UITableView性能)

## 后记

总结就是UITableView是一个高度设计的控件。它具有重用，分组，异步加载数据等方面需要我们注意。

其实这里插进来写UITableView是为了写NSFetchedResultsController。因为NSFetchedResultsController就是为UITableview量身打造的Core Data的类。 

所以，先说明以下UITableView，然后再写NSFetchedResultsController。




























