title: iOS学习笔记(28) ReactiveCocoa 迎接下一个更加美好的世界（2013-10-13 update 2.0）
date: 2013-09-11 20:45:13
tags: iOS
---


## 扯淡

习惯了，每次再写技术的东西的时候总要唠叨几句。本来唠叨的东西我应该会写成另外的blog。不过每次给自己下了一个底线要少少的写这些唠叨的话语。原因一是觉得我爱唠叨的话语可能会导致blog被墙。原因之二我不希望我变成一个IT评论家。

对了,我发现我还是挺爱挖坑的。目前有两坑没有填完。一个是Core Data系列。一个是iCloud系列。两个系列我都只写了一个Hello World级别并没有再深入的继续写。恩，要抓紧了。其实ReactiveCocoa这个我觉得也可以作为一个系列来写。不过想了想我这种Hello World的水平。也写不出这么多来。就暂时写一篇好了。

<!--more-->

## 什么是ReactiveCocoa

如果你有看Github的Trending Objective-C榜单，那你肯定是见过ReactiveCocoa了。如果你在weibo上关注唐巧、onevcat等国内一线知名开发者。那也应该听说过ReactiveCocoa了。

ReactiveCocoa更加被Mattt Thompson大神称为开启一个新Objective-C纪元。

当然也有人声称ReactiveCocoa是Cocoa的未来。[ReactiveCocoa: The Future of Cocoa Programming](http://spin.atomicobject.com/2013/04/28/reactivecocoa/)

我自己粗犷把现在的Objective-C分为两个阶段。

第一个阶段就是我学Cocoa开发之前的阶段：就是把Objective-C做出来的那群NeXT的大神，确定面向对象思想，确定消息机制，确定各种模式最后变成了Apple的主力开发语言。到后面OS X的各种库。iOS的各种库。

第二个阶段就是我学Cocoa开发之后的阶段：开始clang发力，配合Objective-C的快速进化：ARC，block，现代Objective-C语法。使其Objective-C不断获得现代语言类如Ruby, Python的优秀特性。

现在，说的最多的就是ReactiveCocoa将会把Objective-C带到下一个里程碑中。

ReactiveCocoa是一个基于Functional Reactive Programming编程思想的Objective-C实现开源的第三方库。最初的作者是Github的大神（Josh Abernathy & Justin Spahr-Summers）。应该是再开发Github For Mac时候的附属产物。当然，我们必需得明白有时候附属产物要比真主牛逼的多了去了。比如万艾可，再比如青霉素，再再比如老干妈。


等等 什么是Functional Reactive Programming

### Functional Reactive Programming

[wiki解释](http://en.wikipedia.org/wiki/Functional_reactive_programming)

#### Reactive Programming

[wiki 响应式编程](http://zh.wikipedia.org/wiki/响应式编程)

#### Functional programming

[wiki Functional programming](http://en.wikipedia.org/wiki/Functional_programming)

恩 简单来说 

	Functional Reactive Programming = Functional programming + Reactive Programming
	
(PS:特么太偷懒了还是解释一下)

简单以

a = b + c

为例

通常情况下在执行a = b + c的值的时候b和c当时是什么值。a就是当时的b+c。然后后来不管b和c怎么变化，a都不会改变。

但是在Execl中设置a格子=b格子+c格子的值。a的值就会随着b和c值的改变而改变。然后我还可以搞的高级一点f = a + d。 f格子的值也会随着b、c、d的值而改变。这就是使用Functional Reactive Programming以后会发生的情况。

Reactive的特性使得可以随时响应变化。Functional的特性使得他们可以串起来。



### 来自微软实验室的编程思想
在[ReactiveCocoa的readme](https://github.com/ReactiveCocoa/ReactiveCocoa/blob/master/README.md#more-info)我们看到ReactiveCocoa是基于.NET的Reactive Extensions(Rx)来的。啧啧身为一个脑残果粉不解释的我。也必须承认微软其实曾经一度集聚这世界上一大批大牛。这批大牛在闲着玩的时候做出来的玩具也指不定哪天就拯救世界了。

#### Rx
所以我也找了点Rx的资料来看看

中文里面比较全面的是这个
[Reactive Extensions入门](http://www.cnblogs.com/yangecnu/archive/2012/11/03/Introducting_ReactiveExtensions.html)

里面有一堆链接和一个视频。c#实在看不懂，就只看了15分钟左右。不过那看上去蛮帅的哥们一开始说他在编程中遇到的问题。恰巧是我最近遇到的问题：就是我有两个在非主线程的异步操作。而我有可能需要等两个异步操作都完成的时候进行下一步操作。这种情况我一直没有找到比较优美的解决方法。直到遇见ReactiveCocoa，仿佛看见了希望。

#### Model-View-ViewModel

[洋文wiki](http://en.wikipedia.org/wiki/Model_View_ViewModel)

[中文wiki](http://baike.baidu.com/view/3507915.htm)

FRP倾向于技术理论上的方法论。MVVM则是程序模式的方法论。就好比FRP给了一把解牛的刀。MVVM就是如何解牛的方法论。

相对来说MVVM通常跟MVC拿来比较。在我看来，没有绝对的好和坏。找个适合的就好了。再说我对两个东西的了解不够深入。

[Basic MVVM with ReactiveCocoa](http://cocoasamurai.blogspot.com/2013/03/basic-mvvm-with-reactivecocoa.html)

这篇blog应该就写的蛮清楚了。

这是github上iOS的 MVVM例子[MVVM-IOS-Example](https://github.com/Machx/MVVM-IOS-Example)

大家可以感受一下。


## ReactiveCocoa的基本使用方法

(终于写到正主了,泪流满面)

这里借用Limboy的[blog](http://blog.leezhong.com/ios/2013/06/19/frp-reactivecocoa.html)中的一段话作为开场解释。(因为我想了好久都没有想出超过他的比喻方法)

	ReactiveCocoa是github去年开源的一个项目，是在iOS平台上对FRP的实现。FRP的核心是信号，信号在ReactiveCocoa(以下简称RAC)中是通过RACSignal来表示的，信号是数据流，可以被绑定和传递。
	可以把信号想象成水龙头，只不过里面不是水，而是玻璃球(value)，直径跟水管的内径一样，这样就能保证玻璃球是依次排列，不会出现并排的情况(数据都是线性处理的，不会出现并发情况)。水龙头的开关默认是关的，除非有了接收方(subscriber)，才会打开。这样只要有新的玻璃球进来，就会自动传送给接收方。可以在水龙头上加一个过滤嘴(filter)，不符合的不让通过，也可以加一个改动装置，把球改变成符合自己的需求(map)。也可以把多个水龙头合并成一个新的水龙头(combineLatest:reduce:)，这样只要其中的一个水龙头有玻璃球出来，这个新合并的水龙头就会得到这个球。
	
	

### 替代KVO

~~官方例子：官方的例子貌似用了比较老的函数。我改完以后看见[什么是函数响应式编程(Functional Reactive Programming:FRP)](http://www.jdon.com/45581)他也是这么改的。说明一下。~~

经过后来的使用才发现特么官方例子是2.0的。 现在重新改一下。随便说一句，用cocoapods安装的2.1.编译不过。具体原因还没有看。建议使用2.0

```
@property (strong) NSString *username;

[RACObserve(self, username) subscribeNext:^(NSString *newName) {
								NSLog(@"%@", newName);
							}];
```

在这句代码以后，只要你的username有变化。都可以打印出来。实现了KVO的功能却减少了无数的代码。体现了绑定和响应。

高级一个点的官方例子

```
[[RACObserve(self, username)
  filter:^(NSString *newName) {
		return [newName hasPrefix:@"j"];
	}]
 subscribeNext:^(NSString *newName) {
	 NSLog(@"%@", newName);
 }];
```

第一个例子是简单的所有变化都会响应到。但是可能我只想响应部分情况。这时候就用filter来过滤。filter的block返回YES的情况就是需要触发的情况。其他就补返回。所以这代码以后。 username以j开头的才能打印出来。

### 流的实现

以下是[Getting Started with ReactiveCocoa](http://www.teehanlax.com/blog/getting-started-with-reactivecocoa/)的例子和图片

如何以最少的代码实现一个时钟应用



```
RAC(self, timeLabel.text) = [[[RACSignal interval:1 onScheduler:[RACScheduler currentScheduler]] startWith:[NSDate date]] map:^id (NSDate *value) {
	NSLog(@"value:%@", value);
	NSDateComponents *dateComponents = [[NSCalendar currentCalendar] components:NSHourCalendarUnit |
	 NSMinuteCalendarUnit | 
	 NSSecondCalendarUnit fromDate:value];
	return [NSString stringWithFormat:@"%02ld:%02ld:%02ld", (long)dateComponents.hour, (long)dateComponents.minute, (long)dateComponents.second];
}];
```



实现的逻辑顺序是这样的。设置一个间隔为一秒。从现在开始调用的函数。并把当前实际传入。 这个函数返回一个NSString。 然后把这个NSString和界面上的textField绑定在了一起。从而实现了我认为我见过最简单时钟程序。表现了流和绑定响应。

![原blog中对上述代码的流的形容图](http://teehanlax.com.s3.amazonaws.com/wordpress/wp-content/uploads/chaining.png)

### 组合

几乎每个ReactiveCocoa的Demo里面都会出现的例子。

(继续盗图图)

![](http://blog.leezhong.com/image/FRP_register_demo.png)

这个是用的leezhong的图。 应该是从[这个演讲PPT里面来的，点过去还有视频哦](https://speakerdeck.com/andrewsardone/reactivecocoa-at-mobidevday-2013)

就是说在必需验证每个所填写的数值符合标准。Button才能点击。



```
RAC(self, submitButton.enabled) = [RACSignal combineLatest:@[self.usernameField.rac_textSignal,
				 self.passwordField.rac_textSignal]
	reduce:^id (NSString *userName, NSString *password) {
	return @(userName.length >= 6 && password.length >= 6);
}];
```



简单的解释就是把usernameField和passwordField的信号绑定在了一起做reduce处理以后。返回一个BOOL值去跟self.submitButton.enabled进行绑定。

![](http://teehanlax.com.s3.amazonaws.com/wordpress/wp-content/uploads/combining.png)


囧。再次发现和leezhong借用的代码和图都是一样的。


### 异步和网络

(丢个链接 等心情好了 在补。。。)

[when-to-use-reactivecocoa](https://github.com/ReactiveCocoa/ReactiveCocoa#when-to-use-reactivecocoa)



## 总结

其实我对ReactiveCocoa了解还是在hello world阶段。 很多东西都理解的很粗糙。上面这一大陀blog。很多地方写的肯定不好。欢迎指出。其实很多时候觉得确实中文原创的技术文章十分少。很大一部分都是翻译的。国外的技术文章也确实写的牛写的好。但是总觉得自己应该写点自己思考的东西。

对于ReactiveCocoa来说，我认为它确实是一个好东西。正如leezhong所说
	
	RAC统一了对KVO、UI Event、Network request、Async work的处理，因为它们本质上都是值的变化(Values over time)。
	
其实App上90%的操作不就只有这些么。所以我会一直对ReactiveCocoa投入时间去学习和使用。顺便说一句。阿里Mac Lab出品的Xiami for Mac。是我见过国内最好的Mac App之一。他们也用了ReactiveCocoa。然后你可以想象对他们做出那些响应交互ReactiveCocoa为他们提供了强有力的输出。


## 参考资料

### FRP

[wiki Functional reactive programming](http://en.wikipedia.org/wiki/Functional_reactive_programming)

[趣味编程：Functional Reactive Programming](http://blog.zhaojie.me/2009/09/functional-reactive-programming-for-csharp.html)

[haskell Functional Reactive Programming](http://www.haskell.org/haskellwiki/Functional_Reactive_Programming)

[wiki 响应式编程](http://zh.wikipedia.org/wiki/响应式编程)

[wiki Functional programming](http://en.wikipedia.org/wiki/Functional_programming)

[函数式反应型编程(FRP) —— 实时互动应用开发的新思路](http://www.infoq.com/cn/articles/functional-reactive-programming)

[什么是函数响应式编程(Functional Reactive Programming:FRP)](http://www.jdon.com/45581)


[Reactive Extensions入门](http://www.cnblogs.com/yangecnu/archive/2012/11/03/Introducting_ReactiveExtensions.html)


### ReactiveCocoa

[ReactiveCocoa与Functional Reactive Programming](http://blog.leezhong.com/ios/2013/06/19/frp-reactivecocoa.html)

[Better Code for a Better World by Josh Abernathy](https://speakerdeck.com/joshaber/better-code-for-a-better-world)


[nshipster Reactive​Cocoa](http://nshipster.com/reactivecocoa/)

[ReactiveCocoa: The Future of Cocoa Programming](http://spin.atomicobject.com/2013/04/28/reactivecocoa/)


[Getting Started with ReactiveCocoa](http://www.teehanlax.com/blog/getting-started-with-reactivecocoa/)

[Functional Reactive Programming on iOS with ReactiveCocoa](http://www.teehanlax.com/blog/reactivecocoa/)

[Basic Operators](https://github.com/ReactiveCocoa/ReactiveCocoa/blob/master/Documentation/BasicOperators.md)

[Basic MVVM with ReactiveCocoa
MVC - One Pattern to Rule them all](http://cocoasamurai.blogspot.com/2013/03/basic-mvvm-with-reactivecocoa.html)

[How I Wrote Vinylogue for iOS With ReactiveCocoa](http://twocentstudios.com/blog/2013/04/03/the-making-of-vinylogue/#design)

[来自好友RoCry的推荐](https://github.com/RoCry/rocry.github.com/wiki/Project_ReactiveCocoa)

[ReactiveCocoa at MobiDevDay 2013](https://speakerdeck.com/andrewsardone/reactivecocoa-at-mobidevday-2013)

[ReactiveCocoa at MobiDevDay 2013视频](https://vimeo.com/65637501)

[Input and Output](http://blog.maybeapps.com/#fn:p42894317939-5)

