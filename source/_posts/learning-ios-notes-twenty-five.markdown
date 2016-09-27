title: iOS学习笔记(25) iCloud(一) 概览
date: 2013-08-19 22:29:37
tags: iOS
---


## 什么是iCloud

iCloud是苹果公司提供的云服务的总称。每一个拥有Apple ID的用户都拥有5G大小的空间使用权。用户可以使用iCloud的空间来进行数据的备份，联系人备份，照片备份，应用备份等。好处就是当你有多台设备或者是换新的设备的时候。可以有无差别的体验。

对于开发者来说，iCloud相当于提供了一种官方形式的云端存储形式。帮助你的App实现所谓理想的状态。不论用户在什么设备打开App。 里面的数据，设置，形状，大小。都跟他上次打开的一模一样。这样你的App会给你的用户非一般的体验。

## 开发慎用iCloud

上一段说了iCloud的优点，其实是接近意淫中的理想状态。我以近一年的血泪史告诫，初级开发者，应该避免使用iCloud。中高级开发者视项目规划内容酌情使用。如果能有其他方案替代iCloud，建议优先使用其他方案。

初级开发者: iCloud的三种存储方式 Key-Value， Documents， Core Data都是在之前已有的框架上进行扩展的高级接口。换言之，你应该先具备了这三种技术的基本知识再开始考虑学习iCloud。 iCloud + Key-Value对应的基础是NSUserDefaults。 iCloud + Documents对应的基础是使用文档进行数据存储(NSFileManager,NSFileCoordinator,NSFileWrapper,NSCoding等一系列持久化数据存储到文件的问题). iCloud + Core Data的对应基础就是Core Data。本来是强烈建议如果没有这些基础的人不要直接来学习iCloud的。但是谁都是一步一步走过来的，如果要用到iCloud的某一种方式。建议先把基础的持久化方式原理弄懂了，在看iCloud的部分。不然学习门槛的过高，会使得进度无法按时推进。

中高级开发者：如果是已经用过Key-Value，Documents，Core Data的。应该会很快能明白iCloud的原理。 而进行开发。但是值得注意的是，iCloud的使用和调试会非常的消耗时间和精力。并且和你的当前网络状态息息相关。然后会出现各种诡异的情况。这个时候你都需要淡定很超级的耐心。去找到这些坑，然后慢慢的积累经验去绕过这些坑。（在这里先挖坑，后面在慢慢写我的一些使用经验）

<!--more-->

# 为什么要用iCloud
既然iCloud这么多坑。为什么还要使用iCloud进行开发呢？这个问题最近半年的每周的某些时候我都会这样的问自己。 

个人觉得最重要的是:

**在需要数据统一的服务里面，几乎所有的第三方服务和自己搭建服务器为了识别唯一的用户。都需要进行一次注册流程。这是不可避免的。但是把注册用户的流程接入App里面。对我来说不能第一次打开就畅快的使用App是难以忍受的。iCloud由于是基于Apple ID的，相当于一个正常的用户来说是必备的。所以，使用iCloud技术可以避免注册用户流程。和App登入功能。即开即用的体验会大大提高。**

(注意，在这里我说的是几乎所有的第三方服务不包括Dropbox。Dropbox[新的Datastore API](https://www.dropbox.com/developers/datastore)和本身Dropbox的普及率感觉会比注册要好一些。继续挖坑以后填)

其他的原因我觉得不是很重要但是还是列出来：

1. iCloud是Apple的服务。好歹是市值4000亿刀的公司。理论上来说比小公司靠谱一些把。
2. iCloud是Apple自家的服务。App里面如果使用到了iCloud服务。理论上来说会被官方推荐的机率大了一些
3. 如果以后做Apple全平台(iOS,Mac)自然对接起来应该方便一些。(感觉这个是YY)


## 除了iCloud还能用啥

### Dropbox
第一个顺位的自然是[Dropbox](https://www.dropbox.com/developers).

Dropbox不仅仅是一个无处不在的U盘。它还可以变成你App的数据存储端。而且新的Datastore API也显示着Dropbox会大力的发展成为App的数据端的决心。

### Parse

[Parse](https://www.parse.com)的顺位也相当高。 原因在于，Parse的服务不仅仅有Data。还具备推送、社交分享等一系列高端大气上档次的功能。而且它的SDK覆盖了所有主流平台(iOS OSX Android JS WP8 W8 .NET)。更加重要的是，它在一定数据和请求量上是免费的。如果你的用户做到超过它的免费额度。我觉得这个时候你也就能轻松负担起它的付费版了。顺便说一句Parse应该是被Facebook收了。所以，应该对他们充满信心。

### avoscloud
推荐[avoscloud](https://cn.avoscloud.com)的理由很简单。这是好基友Summer参与开发的。 作为好基友肯定要鼎力推荐一下。 AVOS看起来是国际高端大气上档次的大厂。但是这个cloud，应该是完全由在天朝的团队开发的(个人猜想). 为什么要推荐？起码人家的文档是中文的，阅读起来没障碍。 有问题了，还可以直接去weibo吐槽也不当心人家看不懂好吧。

### helios
当然如果你喜欢自己搞定一切。那你还是可以自己搭建自己的服务器的。如果你需要一个类似的参考[Mattt大神的helios](http://helios.io)你不应该错过。如果你写iOS or Mac OS X开发超过三月，还不知道Mattt大神。那你应该恶补一下圈子里面的知识了。Mattt大神就是主导AFNetworking的那个啊。 不说围绕着AFNetworking衍生出来的庞大类库。 当一个[NSHipster](http://nshipster.com)就造福一方啊。


## 总结 && 瞎扯蛋

好久好久没有更新Blog。鄙视一下自己懒的不成样子了。其实好几次想写点扯淡的东西。但是总觉得技术Blog就应该有个技术Blog的样子。不要总是在扯淡。扯着扯着就变成IT评论家的感觉。今天黑这个，明天黑那个的。不好，应该自己踏踏实实的静心学习。最近就是因为心太乱了，导致各种问题。不过期待能把这个iCloud的系列坑给填好。也能算给自己学习了一年iCloud的使用做出一个总结不至于白弄了一年。

最后嫌弃我写的太慢的同学。那就给一个入口地址：从[这里](http://developer.apple.com/library/Mac/documentation/General/Conceptual/iCloudDesignGuide/Chapters/Introduction.html#//apple_ref/doc/uid/TP40012094-CH1-SW1)加油把