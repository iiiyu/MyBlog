---
layout: post
title: "简单配置PonyDebugger"
date: 2013-01-14 01:36
comments: true
tags: iOS
---


## 前言

iOS的Debug 系统在github上还是有不少。 PonyDebugger是看上去比较牛气的一个。尝试一下

![Logo](https://github.com/square/PonyDebugger/raw/master/Documentation/Images/Logo.png)

PonyDebugger

可以监控网络

![NetworkTrafficDebugging](https://github.com/square/PonyDebugger/raw/master/Documentation/Images/NetworkDebugging.png)


还可以查看Core Data对象

![CoreDataBrowser](https://github.com/square/PonyDebugger/raw/master/Documentation/Images/CoreDataBrowser.png)

view的层级查看

![ViewHierarchyDebugging](https://github.com/Flipboard/PonyDebugger/raw/master/Documentation/Images/ViewHierarchyDebugging.png)

这种好东西。 让我们快速开始吧。


<!--more-->



## 快速开始

### 服务器端

* **1.** 安装 Xcode's Command Line Tools 
* **2.** 在shell里面执行下面命令

``` sh
curl -sk https://cloud.github.com/downloads/square/PonyDebugger/bootstrap-ponyd.py | \
  python - --ponyd-symlink=/usr/local/bin/ponyd ~/Library/PonyDebugger
```

* **3.** 安装成功以后，在shell里面执行

``` sh
ponyd serve --listen-interface=127.0.0.1
```

* **4.** 打开你的浏览器 输入地址

	http://localhost:9000


如果看见的是这样
![test1](http://ww4.sinaimg.cn/large/a74ecc4cjw1e0sfa4zcdkj.jpg)
说明服务器端已经安装好了。

### iOS端

* **1.** 把PonyDebugger作为你自己的项目的一个git submodule添加到你自己的项目里面

``` sh
cd /path/to/YourApplication
mkdir Frameworks
git submodule add git://github.com/square/PonyDebugger.git Frameworks/PonyDebugger
git submodule update --init --recursive
```

PonyDebugger依赖于[SocketRocket](https://github.com/square/SocketRocket)所以当你update的时候也会把SocketRocket一起clone下来。

* **2.** 然后把PonyDebugger/PonyDebugger.xcodeproj 增加到你的项目里面去。

![](http://ww1.sinaimg.cn/large/a74eed94jw1e0sfjqiz56j.jpg)

![](https://github.com/square/PonyDebugger/raw/master/Documentation/Images/Installing_Subproject.png)

* **3.** 在你的Project Settings里面的Build Phases标签里面把PonyDebugger作为Target Dependency的一个添加进去

![](https://github.com/square/PonyDebugger/raw/master/Documentation/Images/Installing_TargetDependencies.png)


* **4.** 链接libPonyDebugger.a和libSocketRocket.a

![](https://github.com/square/PonyDebugger/raw/master/Documentation/Images/Installing_LinkLibraries.png)

* **5.** 添加link参数-Objc

![](https://github.com/square/PonyDebugger/raw/master/Documentation/Images/Installing_OtherLinkerFlags.png)


* **6.** 最后 检查一下你项目的Framework有没有以下Framework如果没有添加一下（包括libPonyDebugger.a和libSocketRocket.a）

libicucore.dylib

CFNetwork.framework

CoreData.framework

Security.framework

Foundation.framework


到这里环境就配置好了，下面就来用把。 


## 使用  

### 基本用法
PDDebugger是一个单例 这样获得.

``` objc
PDDebugger *debugger = [PDDebugger defaultInstance];
```

自己连接网络

``` objc
[debugger autoConnect];
```

或者亲自指定服务器端 比如 ws://localhost:9000/device

``` objc
[debugger connectToURL:[NSURL URLWithString:@"ws://localhost:9000/device"]];
```

关闭连接

``` objc
[debugger disconnect];
```


更多用法 参考
[主页](https://github.com/square/PonyDebugger)

这篇blog相当于简化翻译 囧。
















