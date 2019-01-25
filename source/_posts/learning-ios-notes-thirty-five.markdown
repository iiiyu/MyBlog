title: iOS笔记(35) 格志周年系列之夏令时(三) 临时花絮
date: 2014-04-08 22:49:48
tags: iOS
---

本文仅作为个人学习记录使用,也欢迎在[许可协议](http://creativecommons.org/licenses/by-nc/3.0/deed.zh)范围内转载或使用，请尊重版权并且保留原文链接，谢谢您的理解合作。如果您觉得本站对您能有帮助,您可以使用[RSS](https://iiiyu.com/atom.xml)方式订阅本站,这样您将能在第一时间获取本站信息.

## 开篇扯淡

说曹操，曹操到。嘛当，还说总结一下时间的问题。这不blog这个系列没有写完。又爆了出一个时间相关的Bug。我只能说编程路茫茫，吾将上下求索。这次就着热乎着，来说是一个遇到了什么问题。

## 遇到的问题
有日本用户反馈，新版本更新以后，他日历上的时间全部乱了。而且无法写入日记。经过与用户沟通（感谢喵神onevcat的日文人肉翻译）分析得到用户使用和历（日本日历）。然后debug进去果然日期全部乱了。跟进去debug了一番，发现是之前解决夏令时的函数只考虑了公历！！！而iOS系统默认有三种日历。公历、和历、佛历。又一次无情的证明了我是一个天朝土包子。

<!--more-->

## iOS && OS X 支持的日历

这个问题的出现提醒了我，地球上不同的人们其实是使用着不同的日历。（在这之前学习到的是地球上的人们过着不同的时区，有夏令时和没夏令时的时间）。

iOS从Settings->General->International->Calendar。

![](http://ww4.sinaimg.cn/large/a6d3226bgw1ef8l1icf7pj20hs0vkdgp.jpg)

可以看到是默认只是支持公历、和历、佛历。

![](http://ww4.sinaimg.cn/large/a6d3226bgw1ef8lfdww4aj20il0cewfz.jpg)

OS X 要多一些...（好吧，大部分我都不认识）

那到底iOS && OS X支持哪些日历呢。

看这里内建的有这些[NSLocale Calendar Keys](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSLocale_Class/Reference/Reference.html#jumpTo_50) (PS: 经过测试农历那个之前处于半残疾状态)


## 用代码测试一下
我遇到的问题是我获得00:00:00的方法是直接用string然后反向设置小时、分钟、秒数得来的。所以，当日历不同的时候date->string就不一样了！！！这样我只判断了公历的时候，和历和佛历就错了。

详细代码可以查考这个[TestNSDateFormatterOnMac](https://github.com/iiiyu/TestNSDateFormatterOnMac)

我fork了一个霓虹大神改动的。 添加了佛历的对比。

![](http://ww1.sinaimg.cn/large/a6d3226bgw1ef8m65ylz2j20lq0gtgo8.jpg)


## 总结

做好一个App其实挺难的，做好一款全球化的App更是难上加难。特别是我这种天朝土鳖，写程序的时候脑子里面就没全球化的意识。

附上一个今天在推上看到的心酸笑话：

编历法的玛雅码农想「我做个日历做上一千年应该就够用了吧」；编UNIX的码农想「我写个OS能用到2038年应该就够用了吧」；编RFC791的码农想「我整个IP能给42.9亿人用应该就够用了吧」——都给全世界添了乱子。我们做码农的一定要引以为戒 #读日语推有感#

## 一些参考

[农历 wiki](http://zh.wikipedia.org/wiki/農曆)

[希伯來历 wiki](http://zh.wikipedia.org/wiki/希伯來曆)

[ISO 8601 wiki](http://zh.wikipedia.org/wiki/ISO_8601)

[UNICODE LOCALE DATA MARKUP LANGUAGE (LDML)](http://www.unicode.org/reports/tr35/tr35-25.html#Date_Format_Patterns)

[NSDateFormatter で、和暦の変換に固定する](http://balunsoftware.jp/info/2013/06/nsdateformatter-japanese/)

[在开发iOS程序时对日期处理的总结](http://kevin-wu.net/ios-locale-and-calendar-tips/)
