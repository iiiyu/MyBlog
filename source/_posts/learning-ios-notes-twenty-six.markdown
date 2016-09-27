title: iOS学习笔记(26) iCloud(二) 准备工作
date: 2013-08-26 22:43:11
tags: iOS
---


## 开发支持iCloud的上半部分前期准备


* 需要申请一个开发者的帐号。理论上iOS和Mac OSX的都OK。考虑到我只有iOS的情况下，我的文章里面的例子默认只是iOS的。

* 你需要一台iOS设备，并且iOS版本必须大于等于5.最好大于等于6. 最最好大于等于7。

* Xcode不用说至少是MAS里面最新的

* 然后去建立App Bundle ID的页面去把iCloud支持打开。

![1](http://ww1.sinaimg.cn/large/686e6613jw1e88hq7q78aj20sa0g2tav.jpg)

![2](http://ww1.sinaimg.cn/large/686e6613jw1e88hqn9gdej20s60h2wgz.jpg)

![3](http://ww1.sinaimg.cn/large/686e6613jw1e88hqn9gdej20s60h2wgz.jpg)

![4](http://ww2.sinaimg.cn/large/686e6613jw1e88hts5dkpj20k70en75m.jpg)

* bundle id需要生成带你测试设备的证书

![5](http://ww4.sinaimg.cn/large/686e6613jw1e88htyxylrj20rw0egmzm.jpg)

(PS:截图太累了 换成文章描述)

这样才能算iCloud的前期工作做完了一半。

**以下涉及到NDA内容请自行屏蔽**
<!--more-->

* 据说Xcode5里面支持直接开启iCloud支持，推送服务支持等。不用去登录网页了。


* 据说Xcode5支持iCloud调试了不需要真机了。可是特么只是支持iCloud Key-Value。Document和Core Data还是需要你一台强力的真机。

(详细和剩下的大家自行脑补)


## 开发支持iCloud的下半部分前期准备
* 建立一个跟上部分bundle id一样的工程项目

![6](http://ww4.sinaimg.cn/large/686e6613jw1e88iciw7yaj212w0qfn0a.jpg)

* 然后在工程里面把entitlements勾上以后。

* 支持key-value stroe把勾选上

* 支持Document和Core Data要添加ubiquity containers

![7](http://ww1.sinaimg.cn/large/686e6613jw1e88id4vlv3j20jc0a63z9.jpg)


到这里，总算是把准备工作弄好了。每一个支持iCloud的App。都必须经过上面的步骤。不可跳过直接去写代码。切记切记。



## 开发iCloud基本思维

1. iCloud是网络远端的数据存储服务。最终目的是确保所有设备上的所有数据一致性。 
2. 使用iCloud的机制不是纯网络服务。简单的说就是在没有网络的情况下。你的App应该是可以畅通无阻的运行，并且没有缺失功能。iCloud存储的是数据的源头，App里面应该会有对应的缓存确保这样的机制。所以有多少台设备，就有多少份缓存的数据。
3. 缓存数据的份数多了，为了保持数据的一致性。需要同步机制来保持更新，这时候不可避免的会产生数据冲突。就需要解决冲突。

所以，只要是使用了iCloud。不论那种形式。我们都必须面对下面的几种情况并且需要对这些情况进行响应的处理：

1. 何时开启iCloud同步服务
2. 在App内部关闭了iCloud以后，App的应对措施。
3. 在App外部关闭了iCloud以后，App的应对措施。
4. 开启iCloud以后，iCloud数据和本地数据之间的处理。
5. 开启iCloud以后，正在同步数据时，UI的应对措施。
6. 使用iCloud以后，当数据完成更新的时候。UI的应对措施。
7. iCloud数据冲突时候App的处理。

以上是临时能回忆起来的一些需要处理的情况。真实情况可能要复杂的多。

