title: iOS学习笔记(29) 爱不释手的ReactiveCocoa之UIButton
date: 2013-10-15 21:53:11
tags: iOS
---

## 开场扯淡

ReactiveCocoa的迭代速度相当快，一群富有才华和激情的人们在不断的进化ReactiveCocoa。欣欣向荣的景象啊。我这种hello world级别的也就只能使用他们的劳动成果了。上篇blog的时候我还在用1.9.x的版本 现在我已经全面转向2.x了。值得注意的是霓虹友人提交的cocoapods上ReactiveCocoa 2.1 版本我无法编译通过。目前我使用的还是2.0的版本。

介于一个月没有更新blog的速度，这次来写少一点的内容。

<!--more-->


## 传统的UIButton Target Action 方式

之前我们使用UIButton的点击方法一般有两种。一种是直接从xib里面拖一个IBAction出来在里面写代码。

另外一种是代码创建的 比如这样 

```
UIButton *myButton = [[UIButton alloc] init...];
[myButton addTarget:something action:@selector(myAction) forControlEvents:UIControlEventTouchUpInside];
```

然后在下面写一个myAction的方法来进行操作。

这样对我来说存在两个问题：

1. button对应的方法分开了。在阅读代码的时候，当我想知道这个button对应的方法或者反过来action方法对应的button。通常需要跳转一次以上才能知道。(也许是我的阅读代码习惯比较原始)
2. 我在action方法里面如果需要引用一个变量的时候，无法使用局部变量。通常就需要把这个资源设计为一个property。尽管这个资源或者变量只是在action里面调用一次。(这个也许是我写代码的问题)

这两个可能在我遇见ReactiveCocoa都不能叫问题。但是在ReactiveCocoa里面我发现了更加优美的解决方法。好开心。

## ReactiveCocoa方式的UIButton

由于ReactiveCocoa高深的知识点，我也弄不太清楚。下面我只是说明怎么用的hello world级别。更多内容请阅读github上的项目主页。

如果使用xib。只需拖一个IBOutlet的property出来。比如这样

```
@property (weak, nonatomic) IBOutlet UIButton *xibButton;
```

```
NSString *helloWorld = @"hello world!!!";
self.xibButton.rac_command = [[RACCommand alloc] initWithSignalBlock:^RACSignal *(id input) {
	NSLog(@"%@", helloWorld);
	return [RACSignal empty];
}];
```

如果是代码创建一切照旧

```
NSString *helloWorld = @"hello world!!!";
UIButton *myButton = [UIButton buttonWithType:UIButtonTypeSystem];
myButton.frame = CGRectMake(0, 0, 100, 50);
[myButton setTitle:@"Say" forState:UIControlStateNormal];
[self.view addSubview:myButton];
myButton.rac_command = [[RACCommand alloc] initWithSignalBlock:^RACSignal *(id input) {
	NSLog(@"%@", helloWorld);
	return [RACSignal empty];
}];
```

这些代码我一般写在viewDidLoad方法里面。当然你可以在正确的地方使用他们。
运行试试。哇。魔法一般。完全解决我在传统的UIButton遇到的两个问题。


## 总结

嗯嗯，这种小主题的blog写起来轻松愉快。大概40分钟就可以完成。以后要多多写小主题。这样跟写程序一样，化繁为简。这次就少扯淡了。期待我的下篇blog。啊哈哈



