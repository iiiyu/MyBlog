title: iOS笔记(34) 格志周年系列之夏令时(二)
date: 2014-03-19 20:37:48
tags: iOS
---


本文仅作为个人学习记录使用,也欢迎在[许可协议](http://creativecommons.org/licenses/by-nc/3.0/deed.zh)范围内转载或使用，请尊重版权并且保留原文链接，谢谢您的理解合作。如果您觉得本站对您能有帮助,您可以使用[RSS](https://iiiyu.com/atom.xml)方式订阅本站,这样您将能在第一时间获取本站信息.



## 开篇扯淡

恩，两月没更新blog，hexo都出来新主题来着。其实昨天为了找个背景图找了一小时我会随便乱说。就是为了找一个配合网站标题的背景图。其实hexo默认的就蛮好了，但是为了显示那么一点点与众不同还是替换了一下。

扯淡结束，接上一篇[格志周年系列之夏令时(一)](https://iiiyu.com/2014/03/18/learning-ios-notes-thirty-three/)

## 第一阶段Bug

上次说过一个中国高富帅用户发Email来说，他去泰国旅游的时候，日记都不见了。

其实不是日记不见了，日记都好好的躺在sqlite文件里面。而是查询不出来了。日记的保存是用了一个函数去获得了每天的00:00:00. 然后作为唯一标识来区别和查询。

那日期出了啥问题？

我们来快速的分析一下

调用的是

```
- (NSDate *) dateAtStartOfDay
{
	NSDateComponents *components = [CURRENT_CALENDAR components:DATE_COMPONENTS fromDate:self];
	components.hour = 0;
	components.minute = 0;
	components.second = 0;
	return [CURRENT_CALENDAR dateFromComponents:components];
}
```

里面有两个宏

```
#define DATE_COMPONENTS (NSYearCalendarUnit| NSMonthCalendarUnit | NSDayCalendarUnit | NSWeekCalendarUnit |  NSHourCalendarUnit | NSMinuteCalendarUnit | NSSecondCalendarUnit | NSWeekdayCalendarUnit | NSWeekdayOrdinalCalendarUnit)
#define CURRENT_CALENDAR [NSCalendar currentCalendar]
```

假设你使用过Cocoa时间这些类的话能很容易的看出。dateAtStartOfDay函数就是把你持有的date以当前日历为基础，其他不改，小时，分钟，秒钟设置都为0。这样就能得到一个基于当前日历下的date这一天的00:00:00。

简单看上去没有什么问题，回到高富帅的问题。他出国玩一圈咋时间就变了呢？答案是[NSCalendar currentCalendar]改变了。NSCalendar的改变使得dateAtStartOfDay返回的时间也变了。debug到这一步才发觉靠当初为什么没有想到有时区的这个问题。

自己给自己找一个理由就是到目前为止，我只用过大天朝的+8时间。潜意识里面根本没有说换一个时区这样的概念。(后来某一天我翻了本C语言的书第一章就说了国际化时间的问题，再后来weibo上大家都纷纷表示时间是编程里面一个基础点而且做好不容易，只能说我还是太菜太年轻了。这些是后话了)

说道这里那就开始科普一下地球上时间的问题

<!--more-->

## 世界时间

由于人类历史的缘由，地球上以大概的地理位置画分时区。[Time Zone Wiki](http://en.wikipedia.org/wiki/Time_zone)

我认为时间是大爆炸以后产生的扩张现象，在可预见的人类的历史上肯定是不可逆转的。所以每一个时刻对于人类来说应该是唯一的。但是，在这同一个时刻里面，在地球的不同地方使用着不同的时间格式来表现着。我认为这是相当不科学的一个事情。随着人类文明的进步，我觉得最后应该会全世界时间大统一的。

回到现实中还是要解决现在因为时区而产生的问题。

## iOS中的时间

在Cocoa里面，获取一个当前时间是

```
NSLog(@"Now:%@", [NSDate date]);
```

Log出来应该是一个 xxxx-xx-xx xx:xx:xx +0000。但是聪明的你，人如果处在大天朝内马上发现这个时间比你电脑上的时间少了8小时。

恩,因为NSDate记录的是一个绝对的值。这个值代表的意思是UTC时区的绝对时间。我们就把它看作为我们写Cocoa程序的一个绝对时间，千万要记住这一点。因为接下来的一堆概念会把人弄晕的。（我就晕了好久#_#）

[UTC Wiki](http://zh.wikipedia.org/wiki/協調世界時)

与UTC有点关系的GMT时间，稍作了解避免搞混了。（实在分不清，你就记住UTC是一个更加精准的标准时间）

[GMT Wiki](http://zh.wikipedia.org/zh-cn/格林尼治平时)

那NSDate存储了一个什么值来代表时间呢？可以简单的认为他是记录了一个浮点数。这个浮点数代表什么呢？我们注意到头文件里面有个这样的方法timeIntervalSince1970。

```
// seconds
NSLog(@"seconds %lf", [[NSDate date] timeIntervalSince1970]);
```

在写文章的此时此刻打印出来是“1395324408.535384”。简单运算一下

	1395324408.535384 / 60 / 60 / 24 / 365 ≈ 44.245446745

	2014.x - 44.x ≈ 1970

看到这里，就可以很明确的认为用NSDate来存储的时间是从1970-01-01 00:00:00 到那个时刻所经历的秒数。

为什么是1970？如果你接触过一些计算机的知识或者其他编程语言或者数据库等。你都可能在时间相关的地方会发现1970很眼熟的样子。[放鸭](https://duckduckgo.com)搜索了一下。找到一些比较权威的说法。

首先能找到权威解释是[Unix time](http://en.wikipedia.org/wiki/Unix_time)。简单粗暴的来说就是我们现在用的 *nix 以及基于 *nix的一堆东西（包括了现在使用的绝大部分东西）都是1970年极其以后出现的。那时候大家觉得就从1970-01-01 00:00:00开始算时间是个不错的主意。就这么一直延续下来了。（PS：忽略了一堆32bit or 64bit的点。需要详细解释的看url）

看NSDate头文件还

发现一个有意思的宏

```
#define NSTimeIntervalSince1970  978307200.0
```

稍微做了一下运算。可以知道这是从1970-01-01 00:00:00 到2001-01-01 00:00:00经过的秒数。稍微思考一下应该是用来进行了优化运算的。

待续。。。

## 小结

这么一点字又写了两天，还是要每天坚持啊。
