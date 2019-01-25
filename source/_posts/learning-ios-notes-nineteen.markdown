title: iOS笔记(19)
date: 2013-04-01 11:28:20
tags: iOS
---

# Core Data (二)


# 序

[上次](https://iiiyu.com/2013/03/29/learning-ios-notes-eighteen/)只是说了三个Core Data栈基本类。这次准备介绍一下常用的类。


## NSManagedObject

![](http://ww1.sinaimg.cn/large/bfadf3bejw1e39yrt56ifj.jpg)

Core Data是一次底层数据封装成面向对象的技术。最直接的表现就是在SQLite里面的一条记录在Core Data里面的表现是一个NSManagedObject对象。因此我们的增删改查都是基于操作对象的。恩这里多说一句，NSManagedObject是相对NSManagedObjectContext里面是唯一的。而真实的应用情况可能是NSManagedObjectContext会有多个。而NSManagedObjectContext线程不是安全的，所以可能有你多个NSManagedObjectContext里面各自有指向同一条数据的不同的NSManagedObject。这个情况需要你的程序设计和逻辑上去解决。暂时不讨论。

<!--more-->

### 使用NSManagedObject所经历的三种方法

对于NSManagedObject来说一般有以下几种使用方法：

第一种就是我直接使用NSManagedObject来访问我的数据，因为Cocoa特有的KVC机制，使得NSManagedObject可以用KVC的方法去访问属性内容。这样的好处是你不用在NSManagedObject上去写特别的代码，就可以使用。一般入门例子都是这样写的。

第二种是老头视频里面教过的。在点击Data Model选择Entity以后，在xCode的菜单里面有这样一个选项：

![](http://ww4.sinaimg.cn/large/bfadf3bejw1e39z74y7j8j.jpg)

next都按完了以后，会生成选择了的Entity对应的一个NSManagedObject子类。这样的话我们就可以用生成的类来进行增删改查。这样的话看起来代码里面也会变得清晰一些。我们也不必去写KVC的方法去访问修改属性。可以直接就访问修改。在我看来还是好处多多的。为了OO的原则，我们可能会对不同的Entity类去写一些只是公开接口的方法。但是这样生成的NSManagedObject子类是不好修改的。因此我们会去建立一个category类。来进行扩写自己的方法。这样项目里面就会出现一票category感觉不是很好的样子。

第三种方法是我现在在使用的方法。
就是使用Mogenerator.

Mogenerator已经写过一篇blog了。[详细查看Mogenerator的初级使用](https://iiiyu.com/2013/02/22/learning-ios-notes-fifteen/)
好处是生成一次以后，再次生成并会在工程里面重复引用。而且机制上可以直接在生成的类里面进行方法的扩写。不用使用category类。这样整个工程就看起来高级素雅很多。

### NSManagedObject其他注意事项

NSManagedObject就是我们拿来操作数据的基本单位。因此对应，增加一条数据是在NSManagedObjectContext新建一个NSManagedObject。查找数据是查找NSManagedObject。修改是修改NSManagedObject类的属性。删除是从NSManagedObjectContext里面删除NSManagedObject类。最后我们保存NSManagedObjectContext，然后一直向上传递到磁盘上面去。才是持久化的修改。


## NSFetchRequest

NSFetchRequest是一个查询的动作类。我们使用它来在NSManagedObjectContext里面查询相应的NSManagedObject。

一般使用的顺序是先生成NSFetchRequest。指定要查询的NSManagedObjectContext和Entity的名字。然后设置NSPredicate进行过滤。NSSortDescriptor来进行排序。最后用想要查询的NSManagedObjectContext执行NSFetchRequest。就可以得到返回结果了。

## NSPredicate

NSPredicate的作用不仅仅局限于Core Data里面。其他时候也有用到的地方。而在Core Data里面使用简单的说就是作为一个过滤条件。过滤掉我们不想查找的数据。就相当于SQL语句WHERE后面的那些条件。

## NSSortDescriptor

查找到的数据是杂乱无章的。而我们往往希望是具有一定顺序的返回结果。NSSortDescriptor就是用来指定排序的属性和方式的。把NSSortDescriptor生成设置好然后给NSFetchRequest设置。这样我们的结果就可以是按照我们希望的顺序返回回来。以便我们操作。







## 实例代码

新建NSManagedObject

```
NSManagedObject *newManagedObject = [NSEntityDescription insertNewObjectForEntityForName:[entity name] inManagedObjectContext:context];
[newManagedObject setValue:[NSDate date] forKey:@"timeStamp"];
```

删除NSManagedObject

```
[context deleteObject:managedObject];
```


初始化NSPredicate

```
NSUInteger  numberOfServings = 10;
NSPredicate *predicate = nil;
predicate = [NSPredicate predicateWithFormat:@"serves > %i", numberOfServings];
```

初始化NSSortDescriptor

```
NSSortDescriptor *sort = [[NSSortDescriptor alloc] initWithKey:@"name" ascending:YES];
```

初始化NSFetchRequest

```
NSManagedObjectContext *moc = [self managedObjectContext]; NSFetchRequest *request = [[NSFetchRequest alloc] init]; [request setEntity:[NSEntityDescription entityForName:@"Recipe"
                                                                                                                                                            inManagedObjectContext:moc]];
```


带有NSPredicate的初始化NSFetchRequest

```
NSUInteger              numberOfServings = 10;
NSManagedObjectContext  *moc = [self managedObjectContext]; NSFetchRequest *request = [[NSFetchRequest alloc] init]; [request setEntity:[NSEntityDescription    entityForName:@"Recipe"
                                                                                                                                                                inManagedObjectContext:moc]];
NSPredicate *predicate = nil;
predicate = [NSPredicate predicateWithFormat:@"serves > %i", numberOfServings]; [request setPredicate:predicate];
```


带有NSSortDescriptor的初始化NSFetchRequest

```
NSFetchRequest *fetchRequest = nil;
fetchRequest = [NSFetchRequest fetchRequestWithEntityName:@"Recipe"];
NSSortDescriptor *sort = [[NSSortDescriptor alloc] initWithKey:@"name" ascending:YES];
[fetchRequest setSortDescriptors:[NSArray arrayWithObject:sort]];
```
