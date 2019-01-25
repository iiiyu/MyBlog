title: iOS笔记(31) CocoaPods 手把手五分钟教你制作自己的podspec文件
date: 2013-12-19 20:11:16
tags: iOS
---

本文仅作为个人记录使用，也欢迎在[许可协议](http://creativecommons.org/licenses/by-nc/3.0/deed.zh)范围内转载或使用，请尊重版权并且保留原文链接，谢谢您的理解合作。如果您觉得本站对您能有帮助，您可以使用[RSS](https://iiiyu.com/atom.xml)方式订阅本站，这样您将能在第一时间获取本站信息。

##开篇扯淡
圣诞渐进，各种App都在黑五一波冰点。可以遇见的是12月25号前也会有一大波来临。但是！！！今年买软件貌似已经花了很多钱了。而且，也没有几个想入的了。所以就忍忍吧。

然后，做个宣传啊。我们国际化大厂Sumi的App Grid Diary在紧锣密鼓的开发2.0. 完全iOS7设计。各种给力。到时候希望能给大家带来一个好的App吧。

<!--more-->

##CocoaPods

CocoaPods 不必在介绍了吧。如果你是一个iOS or OSX的开发者。然后你跟我说你还没有用过CocoaPods。我肯定会觉得你不够潮(low爆了)。

其实我之前又写过两篇介绍CocoaPods的

只不过时间有点久远了。而且CocoaPods更新很快。有了很多新的特性和功能。 截至我写这篇blog。我的pod version是0.28.0。

不过还是具有参考价值

[使用CocoaPods](https://iiiyu.com/2012/10/26/learning-ios-notes-fourteen/)

[配置自己的CocoaPods库](https://iiiyu.com/2013/03/01/learning-ios-notes-sixteen/)


##五分钟手把手

在上面的[配置自己的CocoaPods库](https://iiiyu.com/2013/03/01/learning-ios-notes-sixteen/)里面。当时只是初步的使用pods。在学习的过程中。很多理解都很浅显。现在终于用了一年多CocoaPods。有点小心得就来记录一下。

github现在每天必看网站。一个是工作需要，另外一个是上面神奇的东西太多太多了。学无止境啊。

使用CocoaPods管理第三方库有时候就会遇到这样的尴尬。好不容易找到了一个心仪的库,却发现CocoaPods里面搜索不到。

怎么办 怎么办

来乖。手把手交你来写hello world。

栗子：  [XCAsyncTestCase](https://github.com/premosystems/XCAsyncTestCase)

首先，去把它fork到自己的项目里面去。(什么不会fork？去面壁去)

然后，把fork到自己帐号下的项目clone出来 cd进去

输入

```
pod spec create https://github.com/iiiyu/XCAsyncTestCase
```

接着你会看到

![](http://ww4.sinaimg.cn/large/686e6613gw1ebpcdvd99xj20lc09f0ui.jpg)

这个很正常，很多项目都没有tag。反正在自己的下面。可以瞎搞。给项目加入一个tag。以便pod能自动识别。

```
git tag -a 0.0.1 -m "Tag release 0.0.1”

git push —tags

rm -rf XCAsyncTestCase.podspec

pod spec create https://github.com/iiiyu/XCAsyncTestCase
```

接着你会看到

![](http://ww1.sinaimg.cn/large/686e6613gw1ebpcfwf0wsj20i6023mxh.jpg)

OK。 然后用你自己喜欢的编辑器打开。

```
mate XCAsyncTestCase.podspec
```


接着其实不用怎么改里面的内容

我把注释删掉 作者改成原来的作者。然后需要的源码位置改成正确的

大概就是这样

![](http://ww2.sinaimg.cn/large/686e6613gw1ebpcik0lh1j20ry0db0v9.jpg)


当然 最重要的是s.source_files这个。你要把你要包含的文件路径找对了。 然后用通配符匹配好了。就OK了。

当然其他项，你看看注释啥的 选择性的填一些。在这里是一个五分钟的hello world。不深入讨论

接着 把修改好的文件push到github上去

```
git add XCAsyncTestCase.podspec

git commit -am "add XCAsyncTestCase.podspec file”

git push

```


最后，在你项目的Podfile里面加入这个第三方库的地址。

```
pod 'XCAsyncTestCase', :git => 'https://github.com/iiiyu/XCAsyncTestCase.git'
```

就可以畅快的使用pod install了


五分钟打完收工有木有。很简单直白有木有。


当然这里只是一个hello world。 如果库中有一些高端设置比如要包含资源文件啊。 加入库依赖啊。 配置一些xcconfig。更多内容 请查看[越来越好看的官方网站](http://cocoapods.org)


##CocoaPods福利时间

以下是我平时使用经常用到的Podfile会用到的一些写法。

### 福利一
首先是有一些库编译时候会有警告。但是作为一个有洁癖的人呢不想看见这些

可以在platform :ios,  'x.0'的后面加入这句

```
inhibit_all_warnings!
```

这样编译这些第三方库的时候就没有那些烦人的小警告了。

### 福利二
使用福利一是不是很爽呢。但是有一个神库ReactiveCocoa。当你关闭所有警告的时候。它就编译不过了。可急坏了。其实很简单对他单独设置打开编译警告就好了

```
pod 'ReactiveCocoa', '~> 2.1.8', :inhibit_warnings => true
```

### 福利三
如果你有多个Targets需要pod的库怎么办
也很简单。Podfile的头部加入

```
link_with ['AAAAA', 'BBBBB']
```

AAAAA和BBBBB都是你target的名字，这样不同的target都会有pod库了。我主要是用来解决Unit Test需要pod install一些库的问题。当初也是找了老半天才找到。



##总结
CocoaPods很好用。而且一直在进化。我发现我怎么写介绍都只停留在很浅显的基础上。更多更深入的内容需要自己使用了。然后慢慢积累的。总之。不用CocoaPods的Cocoa开发太不潮了。
