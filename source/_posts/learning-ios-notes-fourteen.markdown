---
layout: post
title: "iOS笔记 (14)"
date: 2012-10-26 07:42
comments: true
tags: iOS
---

# 使用CocoaPods

## 序

在iOS开发中，经常性的引用第三方开源的库。github上大量的库为我们开发iOS提供了强大丰富多样的资源。从新手一直过来的我们，面对使用越来越多的第三方库。手足无措。CocoaPods的出现使得一切变得Hacker起来。使用CocoaPods一定会让每次copy文件到项目的你喜极而泣的。

<!--more-->

## 之前的方式

在使用CocoaPods一般情况下是这样应用第三方库的。

1. 人工copy —— 为了使用，首先用git clone到某个目录。然后自己手动copy到自己的项目里面。(之前的大部分时间里面我都是这么干的)缺点: 跟原来的版本控制失去联系，如果库有升级，你需要从头来一次才能进行升级。然后自己的项目也会顺带管理这些代码，显得臃肿。
2. 在尝试很多次人工copy以后，发现一些github上的第三方库使用git submodule来进行第三方库的版本管理。具体使用方法参考[这里](http://josephjiang.com/entry.php?id=342).这样我的第三方库也能保持更新而不用重新进行人工copy。缺点是，每次使用的时候我都需要打一堆我记不住的命令和打开浏览器查找第三方库的git地址。


## 什么是CocoaPods
[CocoaPods](http://cocoapods.org)是Objective-C项目中最好用的第三方依赖包管理工具。只要安装好CocoaPods，在自己的iOS项目路径下建立一份Podfile配置文件，在里面说明要使用哪些第三方库，CocoaPods就会帮忙你搞定所有第三方库的相依性，从clone第三方库甚至到设定好Xcode的项目设置，一切都相当的轻松愉快。你能想像这些工作都是CocoaPods帮我们做好了。

[官网](http://www.cocoapods.org/)

[项目地址](https://github.com/CocoaPods/CocoaPods)

## Get Started
CocoaPods 是一个 Ruby 的 Gem，所以只要在 Terminal 下输入一下就ok了。(因为os x自带了ruby，但是如果你要升级ruby这是另外一个事情了)

### 安装

``` sh
$ [sudo] gem install cocoapods
$ pod setup
```

### 搜索
我们可以用cocoapods来先看看有没有我们需要添加的第三方库

比如我们想用一个json库

```
pod search json                                                                                                                                   

-> AnyJSON (0.0.1)
   Encode / Decode JSON by any means possible.
   - Homepage: https://github.com/mattt/AnyJSON
   - Source:   https://github.com/mattt/AnyJSON.git
   - Versions: 0.0.1 [master repo]


-> JSONKit (1.5pre)
   A Very High Performance Objective-C JSON Library.
   - Homepage: https://github.com/johnezang/JSONKit
   - Source:   git://github.com/johnezang/JSONKit.git
   - Versions: 1.5pre, 1.4 [master repo]


-> MTJSONDictionary (0.0.4)
   An NSDictionary category for when you're working with it converting to/from JSON. DEPRECATED, use MTJSONUtils instead.
   - Homepage: https://github.com/mysterioustrousers/MTJSONDictionary.git
   - Source:   https://github.com/mysterioustrousers/MTJSONDictionary.git
   - Versions: 0.0.4, 0.0.3, 0.0.2 [master repo]


-> MTJSONUtils (0.0.1)
   NSObject category for working JSON convertions with suport keypaths ([dict valueForComplexKeyPath:@"parents[0].children[first].toys[last]"])
   - Homepage: https://github.com/mysterioustrousers/MTJSONUtils.git
   - Source:   https://github.com/mysterioustrousers/MTJSONUtils.git
   - Versions: 0.0.1 [master repo]


-> SBJson (3.1.1)
   This library implements strict JSON parsing and generation in Objective-C.
   - Homepage: http://stig.github.com/json-framework/
   - Source:   https://github.com/stig/json-framework.git
   - Versions: 3.1.1, 3.1, 3.0.4, 2.2.3 [master repo]


-> TouchJSON (1.0)
   TouchJSON is an Objective-C based parser and generator for JSON encoded data.
   - Homepage: https://github.com/TouchCode/TouchJSON
   - Source:   https://github.com/TouchCode/TouchJSON.git
   - Versions: 1.0 [master repo]
```

恩出现很多 我们选一个自己会用的就好了。

### 添加
我们进入我们的iOS项目目录下，然后新建立一个配置文件Podfile

``` 
touch Podfile
vim Podfile
```

然后加入我们想要的第三方库和库的Version号(Podfile内容)

```
platform :ios
dependency 'JSONKit', '~> 1.5pre'
```

保存

执行  

```
pod install
```

完成以后
你在目录下可以看见一个后缀为.xcworkspace的文件(其实是一个目录，代表着xCode的workspace) 然后。以后你写项目都打开这个就好了。
![xcworkspace](http://farm9.staticflickr.com/8185/8126953991_7164f1e696_b.jpg)
打开看以后会发现其实cocoapods做了一个事情就是他把第三方库都编译成了一个库文件。然后你的项目中去包含头文件，libPods.a被作为framework集成到目标项目。Pods.xcconfig 中配置了XCode编译的查找路径，通过这种方式将所有的第三方包来引入到项目中。

啊哈哈，立马感觉引入一个第三方包都变得很现代有木有。

如果需要加入新的库编写Podfile里面 再次执行

```
pod install
```
就好了。

### 向cocoapods提交新的第三方库
如果你在pod search里面没有找到你想要的库怎么弄。cocoapods允许你自己提交，然后他们审核发布。(太人性化了，感动的想哭啊)

简单的说，Spec就是每个包在CocoaPods中的配置文件，其中包括Package的名字，版本号，每个版本对应的下载地址，编译时的参数。等等。

这是该项目的地址，https://github.com/CocoaPods/Specs，在页面上有介绍如何创新新的包，可以Fork该项目，然后通过pull request提交所建的新包。

[官方有视频教程](http://nsscreencast.com/episodes/28-creating-a-cocoapod)

[wiki](https://github.com/CocoaPods/CocoaPods/wiki)




## 参考

[Git Submodule 的認識與正確使用！](http://josephjiang.com/entry.php?id=342)

[CocoaPods：管理 Objective-C 專案裡頭各種 Library 關聯性最棒的方式](http://tw.polydice.com/2012/07/04/cocoapods/)

[Use CocoaPods for Packages Management in Xcode](http://www.cnblogs.com/dragonstreak_1/archive/2012/10/19/2730979.html)




