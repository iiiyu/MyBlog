title: iOS学习笔记(30) Core Data是如何保存的？
date: 2013-12-02 18:33:18
tags: iOS
---

本文仅作为个人记录使用，也欢迎在[许可协议](http://creativecommons.org/licenses/by-nc/3.0/deed.zh)范围内转载或使用，请尊重版权并且保留原文链接，谢谢您的理解合作。如果您觉得本站对您能有帮助，您可以使用[RSS](http://iiiyu.com/atom.xml)方式订阅本站，这样您将能在第一时间获取本站信息。


## 开场扯淡
恩 一个月没有写一篇blog了。恩。就这样把。

<!--more-->

学习使用Core Data也一年多了。之前以为3个月就可以登堂入室。事实是我图样图森破。到现在我也才勉强hello world而已。

比如这个问题 **Core Data是如何保存的**。

一直使用[MagicalRecord](http://github.com/magicalpanda/MagicalRecord) 都被宠习惯了。 直接就调用MR_saveToPersistentStoreAndWait。反正能存进去，也没有太仔细的思考。

恩 最近对Core Data的技能点在进行增加中。所以还是多记录一些。

恰好看见了 DM大神的 blog [How Does Core Data Save?](http://mentalfaculty.tumblr.com/post/65682908577/how-does-core-data-save)

也不算翻译，就是自己写个自己的精简版本看看。

## Core Data是如何保存的

1. 当然是NSManagedObjectContext调用save方法的时候。
2. 这时候context里面持有的那些NSManagedObject将会自己调用自己的willSave方法。
3. NSManagedObjectContextWillSaveNotification发出。
4. 开始验证。这个验证可能是你在data model里面写的。也可以是在NSManagedObject里面代码写的。
5. 验证结束以后数据就合并到NSPersistentStoreCoordinator和NSPersistentStore里面。
6. 如果你在合并的时候更改了数据。这时候会重新验证数据。
7. 这个时候已经把需要验证过的合并数据存到持久化介质当中。
8. 最后NSManagedObjectContextDidSaveNotification这个通知发出。



## 参考资料

[How Does Core Data Save?](http://mentalfaculty.tumblr.com/post/65682908577/how-does-core-data-save)