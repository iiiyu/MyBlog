title: iOS笔记(23) iOS进行单元测试OCUnit+xctool
date: 2013-05-23 14:51:31
tags: iOS
---


# iOS进行单元测试OCUnit+xctool

## 单元测试

### 什么是单元测试

[wiki解释](http://zh.wikipedia.org/wiki/单元测试)

简单说来就是为你的方法多专门写一个测试函数。以保证你的方法在不停的修改开发中。保持正确。如果出错，第一时间让你知道，这样从最小单位开始监控来保证软件的质量。

### 我为什么要单元测试
其实要开始写单元测试的原因是，由于我的原因格志的存储逻辑一直有问题。 一个是代码写的比较搓，一个是修改存储的逻辑的话。影响面比较大。可能修复了一个bug而引入了未知的多个bug。为了Sumi早日达到国际化大厂的标准。决定上单元测试于格志。其实最根本的目的还是想要项目变的更加可靠。

### 单元测试的一般方法

关于测试的书，一搜就一大把。都有高深的理论和方法来指导怎么写单元测试的方法。我觉得嘛不用搞了这么复杂。 无非就3种时候会去想写测试：

1. 代码完成以后
2. 开始写代码之前
3. 修复了一个bug以后

第一种是完成了代码，恩我要测试一下我写的这些方法可靠不可靠。那这时候可以写测试。

第二种一个著名的方法论TDD。主要思想就是在写代码之前，就全部设计好借口。函数名字什么的。然后在写能通过测试的函数。

第三种就是发现了bug，我修复了这个bug。为了确保修复是成功的。那就写个测试吧。

我觉得啊，着三种都没有什么好或差。能写测试的少年都是好少年。何必这么在意什么时候去写呢。

一个完整的测试类组成像下图

![](http://ww2.sinaimg.cn/large/686e6613jw1e58jl2toiej20fh0a1aam.jpg)

在一开始可能测试方法里面需要一些上下文环境。这些可以在Setup里面去完成。然后才可是执行自己写的测试方法。 然后测试结束以后，可能产生了一些垃圾数据文件什么的。这时候你可以在TearDown方法里面把他们处理掉。


以上大部分都是我自己的粗浅理解，如果你需要更多关于单元测试请阅读更加系统专业的书籍。

<!--more-->


## OCUnit

OCUnit是xCode里面自己带的单元测试框架。不必安装第三方的其他库就可以使用。最简单的就是创建项目的时候你把单元测试的那个勾点上。xCode就会自动的为你加入一个单元测试的target。快捷键Command + U。就可以运行测试。最喜欢这样方便又好用的东西了。当然运行OCUnit的测试输出的内容实在惨不忍睹。

### 创建一个OCUnit的Unit Test

#### 新项目使用OCUnit

![](http://ww1.sinaimg.cn/large/686e6613jw1e58kyt9nyhj20k80dnabp.jpg)

选上Unit Test

![](http://ww1.sinaimg.cn/large/686e6613jw1e58kzfmxkij20ei0c93zt.jpg)

会自动的建立一个Target

![](http://ww4.sinaimg.cn/large/686e6613jw1e58l0l72wvj20uf0b4q51.jpg)

自动的为你添加好需要的类库

![](http://ww3.sinaimg.cn/large/686e6613jw1e58l164wjkj20al0d7dh0.jpg)

为你添加一个Unit Test的类


![](http://ww2.sinaimg.cn/large/686e6613jw1e58l1uogchj20an03cq39.jpg)

看一眼Scheme 只有一个。

![](http://ww1.sinaimg.cn/large/686e6613jw1e58l315fwmj20jg0d70ua.jpg)

看看Scheme里面是怎么写的。

![](http://ww1.sinaimg.cn/large/686e6613jw1e58l4n4ozrj209c04vdg1.jpg)

用Command + U运行一下测试看看结果。这是没有通过的。因为自动生成的模板就是不通过的。具体一会儿分析代码。


![](http://ww3.sinaimg.cn/large/686e6613jw1e58l6bue5gj20zc070mze.jpg)

看看终端的输出。渣一般的难看。根本无法高识别度的分清。

#### 已经存在的项目使用OCUnit

![](http://ww1.sinaimg.cn/large/686e6613jw1e58t46ki0fj20770cgdgd.jpg)

这是一个一开始没有选择过Unit Test的项目

![](http://ww4.sinaimg.cn/large/686e6613jw1e58t52kpxhj20ce0jg0tn.jpg)

点击增加Target


![](http://ww2.sinaimg.cn/large/686e6613jw1e58t5vnb0bj20k80dndhf.jpg)

选择Unit testing Bundle

![](http://ww1.sinaimg.cn/large/686e6613jw1e58t6jsonij20k80dnmys.jpg)

为我们的测试bundle取一个名字

![](http://ww3.sinaimg.cn/large/686e6613jw1e58t75sbt3j20bq03h3yu.jpg)

我们可以看到Scehme多出来了一个。这时候如果选择的是App的Scehme。Command + U。是没有运行测试的。要选择我们新建立的Test Scehme。再按Command + U.就运行了测试了。

![](http://ww3.sinaimg.cn/large/686e6613jw1e58t96mpj1j20jg0d775q.jpg)

如何为App的Scehme添加Test。使得不用切换Scehme，就可以运行Unit Test。

![](http://ww4.sinaimg.cn/large/686e6613jw1e58thyodnkj20b40cswet.jpg)

然后选择你建立的Unit Test bundle。 打完收工。


### OCUnit使用的宏



STAssertEqualObjects(a1, a2, description, ...)

STAssertEquals(a1, a2, description, ...)

STAssertEqualsWithAccuracy(a1, a2, accuracy,description, ...)

STFail(description, ...)

STAssertNil(a1, description, ...)

STAssertNotNil(a1, description, ...)

STAssertTrue(expr, description, ...)

STAssertTrueNoThrow(expr, description, ...)

STAssertFalse(expr, description, ...)

STAssertFalseNoThrow(expr, description, ...)

STAssertThrows(expr, description, ...)

STAssertThrowsSpecific(expr, specificException, description, ...)

STAssertThrowsSpecificNamed(expr, specificException, aName, description, ...)

STAssertNoThrow(expr, description, ...)

STAssertNoThrowSpecific(expr, specificException, description, ...)

STAssertNoThrowSpecificNamed(expr, specificException, aName, description, ...)



### 比较经常使用的宏
STAssertTrue(expr, description, ...)
STAssertFalse(expr, description, ...)
STAssertNil(a1, description, ...)
STAssertNotNil(a1, description, ...)
STAssertEqualObjects(a1, a2, description, ...)
STAssertEquals(a1, a2, description, ...)
STFail(description, ...)
STAssertThrows(expr, description, ...)


### 写了几个测试方法的例子

```
- (void)testOne {
	NSString *string1 = @"test";
	NSString *string2 = @"test";
	STAssertThrows([string1 isEqualToString:string2], @"FAILURE");
}

- (void)testTwo
{
    int i = 0;
    int j = 1;
    STAssertTrue(i < j, @" i: %d, j: %d", i,j);
}

- (void)testThree
{
    
    NSString *oneStr = @"hello";
    NSString *twoStr = @"world";
    STAssertFalse([oneStr isEqualToString:twoStr], @"oneStr:%@, twoStr:%@", oneStr, twoStr);
}

- (void)testFour
{
    NSArray *array = nil;
    STAssertNil(array, @"array:%@", array);
}


- (void)testFive
{
    NSDictionary *dict = @{@"hello": @"word"};
    STAssertNotNil(dict, @"dict:%@", dict);
}

- (void)testSix
{
    NSNumber *oneNum = @100;
    NSNumber *twoNum = @200;
    STAssertEqualObjects(oneNum, twoNum, @"oneNum:%@ twoNum:%@",oneNum, twoNum);
}

- (void)testSeven
{
    NSArray *oneArray = @[@11, @22, @33];
    NSArray *twoArray = [oneArray copy];
    STAssertEqualObjects(oneArray, twoArray, @"oneArray:%@, twoArray:%@", oneArray, twoArray);
}


- (void)testEight
{
    NSUInteger uint_1 = 4;
	NSUInteger uint_2 = 4;
	STAssertEquals(uint_1, uint_2, @"FAILURE");
}
    
    
- (void)testExample
{
    STFail(@"Unit tests are not implemented yet in HelloAfterAddOCUnitUnitTest");
}
```

一会儿用xctool跑个华丽丽的出来看。


## xctool

### xctool是什么

xctool是Facebook开源的一个命令行工具，用来替代苹果的xcodebuild工具。

[github](https://github.com/facebook/xctool)

官方演示

![](https://fpotter_public.s3.amazonaws.com/xctool-uicatalog.gif)

你可以用它来Build你的App。跑Tests。而且它跑Test输出是华丽丽的彩色。比xCode自带的不知好看多少倍。OCUnit本来被吐槽无数，遇上了xctool以后就逆袭了啊。

世界上最最牛的SNS出品，肯定不会坑爹啊。

### 安装xctool
最方便 最推荐的是用Homebrew。如果你的Mac里面没有安装Homebrew我觉得是你的损失。

```
brew update
brew install xctool
```

### 使用xctool来跑OCUnit测试
关于如何使用xctool，你去看官方文档肯定要比我结束好的多。 我就是过来跑测试的例子给你看而已。

#### 测试一
```
xctool -project HelloOCUnit.xcodeproj -scheme HelloOCUnit  test
```

![](http://ww4.sinaimg.cn/large/686e6613jw1e58zjmagi8j20eh0li0wx.jpg)

#### 测试二

```
xctool -project HelloAfterAddOCUnit.xcodeproj -scheme HelloAfterAddOCUnit  test

xctool -project HelloAfterAddOCUnit.xcodeproj -scheme HelloAfterAddOCUnitUnitTest  test
```

![](http://ww4.sinaimg.cn/large/686e6613jw1e58zk5dk34j20h70li43i.jpg)


[项目一下载](http://d.pr/f/EUCR)

[项目二下载](http://d.pr/f/LPaY)



## 总结
以上就是OCUnit的使用建议。建议OCunit+xctool。来进行你的单元测试构建。更多信息请阅读相关资料。我这里只是写了入门而已。接下来的测试之路就靠你了。


