title: iOS笔记(32) UbiquityStoreManager 学习笔记1  
date: 2013-12-25 23:22:33
tags: iOS
---

本文仅作为个人学习记录使用,也欢迎在[许可协议](http://creativecommons.org/licenses/by-nc/3.0/deed.zh)范围内转载或使用，请尊重版权并且保留原文链接，谢谢您的理解合作。如果您觉得本站对您能有帮助,您可以使用[RSS](http://iiiyu.com/atom.xml)方式订阅本站,这样您将能在第一时间获取本站信息.

## 开篇扯淡

现在都是进入互联网时代，一个互联网的App数据肯定是存在互联网上的。到处都是云，到处都是服务器。如果数据是存储在云端or服务器端。每次数据的读取和修改直接作用于服务器。这样不管你用多少设备，多少平台。数据都能保证是唯一的。但是还有些App需要一些更好的性能和效果的时候往往等不起网络的数据传来传去。这时候需要一些折中的办法来解决这些问题。iCloud就是Apple给出的解决方案。就普通用户来看，iCloud应该是在Apple系中的最优选择。但是从开发者的角度来看iCloud就是个无穷无尽的深渊。

全球有很多开发者致力开发第三方库以便让iCloud能被使用。 

UbiquityStoreManager就是其中之一。

<!--more-->

## UbiquityStoreManager

[UbiquityStoreManager](https://github.com/lhunath/UbiquityStoreManager)是一个core data持久化层的控制器.

UbiquityStoreManager在iOS 6时代是使用GPLv3协议。所以好像使用者不是很多。iOS 7 Core Data 关于iCloud引入了新的机制和新API以后UbiquityStoreManager迅速跟进。并且更换了协议使用了Apache v2。所以如果你有iOS 7的App想使用iCloud + Core Data这种hard模式。UbiquityStoreManager是第一个应该推荐的库.

UbiquityStoreManager不同于其他解决方案。大部分都是在iCloud上面继续构建一层来保证数据同步的完整一致性。UbiquityStoreManager仅仅是解决了就只单纯使用iCloud就会遇到的问题。比如数据出错了怎么办。从本地数据迁移到iCloud数据。从修改日记来重建数据库等。

## UbiquityStoreManager解决的问题

* 提供在iCloud和本地Store直接切换
* 在用户没有iCloud store 的时候自动的把local data 合并到iCloud(使用本地数据重建iCloud)
* 所有与iCloud有联系的事件处理 iCloud帐号删除、iCloud数据删除、本地数据导入、其他设备变换ubiquitous、恢复有错误的transaction logs
* 其他的一些实用功能：从transaction logs重建cloud store、 从cloud store 重建transaction logs、 删除cloud store、 Ability to nuke the entire cloud container、 把一个store里面全部的entities合并到另外一个store

在iCloud + CoreData的官方解决方案中，对于开发者来说完全的黑箱子操作。难于开发和调试bug。而且经验不足完全会被很多坑拉进泥潭里面。使得开发更本无法进行下去。所以仔细研究UbiquityStoreManager。是我建议踏入iCloud + Core Data开发的必修课。

## 总结

只是我学习中的一点笔记和注释翻译而已。留给自己看看。如果有不对的地方，欢迎指出。
