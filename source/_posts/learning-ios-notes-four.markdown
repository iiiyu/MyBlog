---
layout: post
title: "iOS笔记 (4)"
date: 2012-03-21 13:38
comments: true
tags: iOS
---

# iOS笔记——九宫解锁基础部分


## 目的
为了第一个App，一步一步的做出来。所以先弄最基本的想做一个类似Android九宫解锁的View。一开始完全没有思路啊。后来看了好几遍老头的画笑脸以后才有点思路。所以来一步一步做吧。

<!--more-->

## 继续扯淡MVC
作为一个从iOS5开始学起开发的娃来说，什么nib xib文件都是浮云。哥只懂在Xcode里面的storyboard里面拖拖拽拽。奈何没几个代码实例是Storyboard写的。让想重用代码的我，无法直接⌘-c ⌘-v。很是郁闷哈。不过经过慢慢理解，对于Xcode4里面用stroyboard有了一些小小的心得。分享一下。

一个VC是怎么构成的。在新建工程storyboard里面，我新拖入一个view
如图

![storyboard1](http://farm8.staticflickr.com/7252/6856181868_05dbe1fd8f.jpg)


这是此时，这个仅仅是一个无人管的一段xml而已。因为你的程序里面没有相应的类去接管这个view。就像一个没人要的狗腿子。好可怜的。你需要把他找一个C的主子管着他，让view知道他应该怎么显示什么东西，对发生的事情做出什么样的反应。

* 首先，你至少已经建立了接管这个view的controller类。

* 接着点击你view的这里

![storyboard2](http://farm7.staticflickr.com/6222/7002295489_c0ea10a055_s.jpg)

* 从右侧选择你看见的这里的class的下拉框，选中你的controller

![storyboard3](http://farm8.staticflickr.com/7240/6856181962_678f0eb0c4.jpg)

有时候拖出来的view，并不能够满足你的需求。你可能会写一个subclass继承UIview来进行扩展。你也需要指定你拖出来的view他本身指向这个subview。

这是跟刚刚大同小异

* 首先，你至少已经建立了这个继承UIView的view类。

* 接着点击你view的这里

![storyboard4](http://farm7.staticflickr.com/6058/7002295577_c22eb920e6.jpg)

* 从右侧选择你看见的这里的class的下拉框，选中你的controller

![storyboard5](http://farm8.staticflickr.com/7075/7002295599_3c2b3a65ea.jpg)


这样你就完成了一对VC的创建了。


## 程序基本要做的事情

* 有一个view，view上有背景色
* view上画9个按钮
* view上能画线
* controller能控制手指开始的地方，移动的地方，结束的地方。




## 一步一步

### clone工程

```
git clone git://github.com/iiiyu/Draw-Lock-Login.git
```

项目结构

![ios4-1](http://farm7.staticflickr.com/6037/7002295643_06f72a4961.jpg)

### Draw_Lock类分析(UIView)
点开Draw_Lock.m可以看到
两个方法,其中一个还是注释掉的。

``` objc
- (id)initWithFrame:(CGRect)frame
{
    self = [super initWithFrame:frame];
    if (self) {
        // Initialization code
    }
    return self;
}

// Only override drawRect: if you perform custom drawing.
// An empty implementation adversely affects performance during animation.
- (void)drawRect:(CGRect)rect
{

}
```

initWithFrame虽然看起来像初始化函数的样子，但是如果我们想初始化做一些事情的时候。是要重新写一个函数

```
// 老头说过不直接调用initWithFrame 而是调用awakeFromNib来进行初始化
// 进过测试 initWithFrame也不会被调用
- (void)awakeFromNib
{
    
}
```

如果需要初始化做一些事情，请重载awakeFromNib。

drawRect函数一开始是被注释掉。但是此函数可以在view上进行绘画。关于Quartz请参考相关官方文档。

仅仅记录我觉得重要的地方。


```
// 这个context就是画布，官方叫上下文。每次作画前要有。
CGContextRef context = UIGraphicsGetCurrentContext();
```

一开始我不会设置view的背景，就想把画一个和view相等的矩形。这样不就是背景了。后来发现是可以直接设置的。


```
// 设置view的背景颜色
self.backgroundColor = [UIColor darkGrayColor];
```

把要加入的9个宫。看做一个个的view。我们先创建这些view。然后用addSubview加入Draw_Lock这个view当中，最后调整这些宫的位置就完成了图像的绘制。

每一个宫由两张图片构成。一张为默认，一张为highlighted属性的。当我们碰到宫时候，设置他的highlighted为YES。

然后是线的绘制。当手指触碰到宫的时候，才吧宫的中心作为起点开始绘制线。绘制的线在没有遇到第二个宫的时候跟随手指，如果遇到宫。就绘制完成一条直线。然后把终点的坐标为下一条线的起点。直到手指离开屏幕。


### ViewController类分析（UIViewController）


手指触摸开始

```
- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event
{

}
```

手指移动

```
- (void)touchesMoved:(NSSet *)touches withEvent:(UIEvent *)event
{
    
}
```

触摸结束

```
- (void)touchesEnded:(NSSet *)touches withEvent:(UIEvent *)event
{
    
}
```


### 最后

大概在山寨这个九宫的时候就遇到很多问题。隔了一天，能想到一一记录。当然，还有更多的问题。所以最好就直接看github项目里面有更详细的注释和完整的代码。很多时候，觉得写代码比写blog轻松多了。






























