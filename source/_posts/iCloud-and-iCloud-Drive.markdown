title: iCloud 和 iCloud Drive
date: 2014-10-20 00:25:52
tags: just-talk
---

本文仅作为个人学习记录使用,也欢迎在[许可协议][1]范围内转载或使用，请尊重版权并且保留原文链接，谢谢您的理解合作。如果您觉得本站对您能有帮助,您可以使用[RSS][2]方式订阅本站,这样您将能在第一时间获取本站信息.

## 开篇扯淡
1. 好久没有写 blog 了。 
2. 最近发现很多人对 iCloud 和 iCloud Drive 有些误解。而还没有看见中文里面有一个比较正确的说法。
3.  加上近两年来工作就是在学习 iCloud 如何使用。最近一个月做客服小弟回复了 N 个 iCloud 的问题。所以感觉还是有一些价值的。特意想记录一下。

## 是否升级到 iCloud Drive
在 iOS 8 刚刚上线的时候，用户更新了以后。第一次会跳出来，说需要重新升级的 iCloud Drive。因为没有更多的信息和提示，我想一个正常的用户应该都会去点击升级。结果就是导致很多使用 iCloud 这个功能的 App 数据出现问题。或者导致了设备之间的不同步。那会有很多文章在建议不要升级 iCloud Drive。所以可能会给后来升级到 iOS 8 的用户造成一定的心里作用说升级 iCloud Drive 是不可靠的。

其实根据我两年来 iCloud 的经验和测试结果。 iOS 8 的 iCloud Drive 是一个 Apple 云端的一次最重要的里程碑。 是 iCloud 这个技术在 Apple 产品系列上第一次做到了可用的状态。等了三年终于有个云的模样了。

当时不建议升级 iCloud Drive 的理由其实就两个：
1. 对于开发者来说，由于 Apple 为了保密 iPhone 6 和 iPhone 6 Plus。 其实在9月发布会之前。 iOS 8 的 最后两个 Beta 版本是没有提供给开发者的。在能获得的最后的 Beta 版本上。 开发者使用 iCloud 依然各种莫名其妙的问题。一直到 GM 版本才变得正常。这样导致 GM 到发布正式的版本之间的时间。大部分开发者还无法把更新 iCloud 的技术及时的完善在自己的 App 里面。
2. 另外一个是在 iOS 8 已经放出来的时候，OS X 10.10 还没有放出来。这样如果你是一个 Apple 一套的普通用户。就会导致你一些全平台使用 iCloud 技术的 App无法相互同步。所以在当时确实这样情况的普通用户应该谨慎更新。

现在11月了这两个问题随着开发者对 App 的完善和 OS X 10.10 释出。其实都不是问题了。大家可以放心大胆的升级了。

<!--more-->

##  iCloud 升级到 iCloud Drive 具体是做了什么事情
首先需要明确的是，iCloud 升级到 iCloud Drive。只跟你的 Apple ID 相关。并且这个过程不可以逆转。

### 普通用户
对于普通用户来说应当就是一次在服务端的数据迁移。把之前的存储在 iCloud 服务器上的全部数据迁移到了 iCloud Drive 的服务器。从此以后你的 iCloud 数据都是从 iCloud Drive 服务器上读取了。

基于这种逻辑支持 iCloud 的应用，理论上 100% 可以使用 iCloud Drive。

但是事实上并不是所有的应用在升级了 iCloud Drive 以后都可以使用。

Why？

先解释一个现象，就是在一开始升级了 iOS 8 然后升级了 iCloud Drive 立马就打开了某 App 发现 iCloud 的数据没了。 结果睡觉起来再打开就有了。这种的原因很简单，就是 iCloud 服务器上的数据迁移到 iCloud Drive 服务器上是需要拷贝时间的。数据还没有迁移完成的时候你打开当然没有。等到数据迁移完成了，你再打开它就有数据了。

### 开发者
PS：普通用户可以略过此小结
这次 iOS 8 的升级，一开始就是对使用 iCloud 开发者的一次噩梦。
0. iCloud 服务器数据到 iCloud Drive 服务器数据的迁移。
1.  客户端下面 iCloud 的 Container ID 发生变化。之前是 Term ID.xxxx。现在是 iCloud.xxxx。当然已经上架的应用的 Container ID 是还存在的。但是这个变化从来就没有在文档里面提过（可能现在有了，反正当时没有看见过）导致的结果就是如果容器空间没有选择正确。必然读取不到老数据。
2.  由于 iOS 8 升级需要很大的空间，有部分用户可能是选择重新安装一个新 iOS 8 或者是入了一台新的手机。 但是没有升级 iCloud Drive。这种情况下，会导致 App 无法找到 iCloud 的路径。也是坑的不行

目前能回忆起来比较坑的就这两个，这些都是开发者必须处理的情况。如果开发者花费巨大的力气处理好了，用户其实是感受不到的。这是最好的情况。如果技术不过关，处理不好。用户就上门开始骂了。（PS：这是一个悲伤的故事）

## CloudKit
CloudKit 是今年提出来的新技术。

### 普通用户
使用这个技术的 App 都是靠谱的

### 开发者
简单说来就是一个 Apple 版本的 Parse。具体去看文档不展开说明了。

## 一些存储路径的说明

### iOS
iOS 由于沙箱的原因。其实普通用户看不见什么。不过可以肯定的是。在系统路径下面。 iOS 是有 iCloud 数据的缓存目录进行数据缓存的。简单说来就是使用 iCloud 的应用在删除的时候。 iCloud 缓存里面的数据是不会立马删除的。什么时候清空，未知。

### OS X
OS X 可以看见的就比较多了。

#### Finder 里面的 iCloud Drive
这里其实由两个部分组成：
1. 是 /Library/Mobile Documents/comappleCloudDocs
2. 是使用 iCloud 技术把一些数据文件存储到了自己 iCloud 容器路径下的 Documents 目录里面。

把 Finder 里面这个 iCloud Drive 当作 Dropbox （网盘）使用。那你丢进去的文件数据都是在
```
`/Library/Mobile Documents/comappleCloudDocs
```
`这里路径下面。

而你为什么可以看见一些 App 名字的文件夹呢。就是第二个部分容器自己的 Documents 目录。你可能会问为啥不是全部使用 iCloud 技术的都有这个目录呢？

我来举例说明把在 Apple 生态里面。商店里面的程序都是沙盒的。相互之间都是独立的。不过有一个地方是可以看到沙盒内部，在开发文档里面也是同样描述那就是 Documents 的路径。举例一个 iOS App 如果你在Documents 下面有文件，那你在 iTunes App 的那页下面是可以看到这些文件。同样的概念延续到了 iCloud 上。当 App 的文件存于他自己 iCloud 路径下的 Documents 文件夹下面的时候。你就可以在 Finder 中 iCloud Drive看到。App iCloud 容器下另外的路径是不会显示到 Finder 下面的时候，所以并不是使用了 iCloud 的应用都在这里有文件夹。这些都取决于开发商对自己 App 的设计和实现。


#### /Library/Mobile Documents
这个路径下就是 iCloud 数据在 OS X 的缓存路径。理论上来说，这个路径下是跟 iCloud Drive 服务器上的数据同步的。（没升级 iCloud Drive 就是跟 iCloud 服务器上的数据同步的）

#### /Library/Caches/CloudKit
CloudKit 的缓存路径

## 总结
毫不谦虚的说，本文虽然条例不是清晰。但是是目前中文说明里面对这次 iCloud 变更目前位置最详细的解释。（PS：看在写到凌晨n点的情况下。做自己吹一下）
1. 一定要升级 iCloud Drive，这样对大家都好。用户可以得到更好的体验。开发者可以使用新的技术做出更加好玩的东西出来。
2. 能使用 iCloud 技术的厂家都不容易。请善待他们。
3. 我是一个 iCloud 黑。


[1]:	http://creativecommons.org/licenses/by-nc/3.0/deed.zh
[2]:	http://iiiyu.com/atom.xml