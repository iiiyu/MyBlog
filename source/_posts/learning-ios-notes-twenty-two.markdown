title: iOS笔记(22) CoreData (四) 监听NSFetchedResultsController
date: 2013-04-29 12:03:11
tags: iOS
---

# CoreData (四)

## 监听NSFetchedResultsController

之前说过, NSFetchedResultsController是有两个重要的功能。

第一:NSFetchedResultsController是作用在Core Data上的,通过NSFetchRequest来查询Core Data里面的数据.可以返回按照组分好的数据.这样便于UITableView来显示.

第二:但Model改变的时候NSFetchedResultsController能及时的发出通知.准确的说,应该是当NSManagedObjectContext发生改变的时候,NSFetchedResultsController能知道这些变化,然后发出通知出来.以便UITableview能及时的更新.


上一篇写了第一点. 现在写第二点.


<!--more-->

## 背景
如果在数据改变了的时候,我们用UITableView reload. 整个UITableView的数据确实能保持最新的情况. 但是问题是这样做的效率很低. 更希望的情况是,我哪一条数据增加,修改,删除. 就对应着UITableView里面的那一条数据在UI上增加,修改,删除.这样效率会有很大的提升. 


## 直接操作
首先是用Delegate来进行UITableView的改变.

第一个方法,告诉UITableView数据要开始更新了,你UITableView赶紧准备好更新.

```
- (void)controllerWillChangeContent:(NSFetchedResultsController *)controller
{
    [[self tableView] beginUpdates];
}
```

当然有开始就有结束,下面的方法就是告诉UITableView结束更新.

```
- (void)controllerDidChangeContent:(NSFetchedResultsController *)controller
{
    [[self tableView] endUpdates];
}
```

这两个方法一看命名规则就能看出来是一个Delegate.在NSFetchedResultsController将要变换的时候,我们开启UITableView的编辑,然后在NSFetchedResultsController已经改变结束的时候结束UITableView的编辑.思维上自然而然,有始有终.

接下来我们要在begin和end之间对tableview做出改变.

改变section的方法.

```
- (void)controller:(NSFetchedResultsController *)controller
  didChangeSection:(id <NSFetchedResultsSectionInfo>)sectionInfo
           atIndex:(NSUInteger)sectionIndex
     forChangeType:(NSFetchedResultsChangeType)type
{
    NSIndexSet *indexSet = [NSIndexSet indexSetWithIndex:sectionIndex];
    switch(type) {
        case NSFetchedResultsChangeInsert:
        {
            [[self tableView] insertSections:indexSet
                            withRowAnimation:UITableViewRowAnimationFade];
            break;
        }
        case NSFetchedResultsChangeDelete:
        {
            [[self tableView] deleteSections:indexSet
                            withRowAnimation:UITableViewRowAnimationFade];
            break;
            
        }
    }
}
```

这个方法是在section改变的时候调用.改变类型支持两种NSFetchedResultsChangeInsert和NSFetchedResultsChangeDelete..这样我们能操作UITableView里面section变化了.

接下来的方法是改变cell内容的.

```
- (void)controller:(NSFetchedResultsController *)controller didChangeObject:(id)anObject
       atIndexPath:(NSIndexPath *)indexPath
     forChangeType:(NSFetchedResultsChangeType)type
      newIndexPath:(NSIndexPath *)newIndexPath
{
    NSArray *newArray = [NSArray arrayWithObject:newIndexPath]; NSArray *oldArray = [NSArray arrayWithObject:indexPath]; switch(type) {
        case NSFetchedResultsChangeInsert:
            [[self tableView] insertRowsAtIndexPaths:newArray
                                    withRowAnimation:UITableViewRowAnimationFade];
            break;
        case NSFetchedResultsChangeDelete:
            [[self tableView] deleteRowsAtIndexPaths:oldArray
                                    withRowAnimation:UITableViewRowAnimationFade];
            break;
        case NSFetchedResultsChangeUpdate: {
            UITableViewCell *cell = nil;
            NSManagedObject *object = nil;
            cell = [[self tableView] cellForRowAtIndexPath:indexPath];
            object = [[self fetchedResultsController] objectAtIndexPath:indexPath]; [[cell textLabel] setText:[object valueForKey:@"name"]];
            break;
        }
        case NSFetchedResultsChangeMove:
            
            [[self tableView] deleteRowsAtIndexPaths:oldArray
                                    withRowAnimation:UITableViewRowAnimationFade];
            [[self tableView] insertRowsAtIndexPaths:newArray
                                    withRowAnimation:UITableViewRowAnimationFade];
        break; }
}
```

在这个方法里面我们可以增删改Cell的.并且可以有动画.



## NSFetchedResultsController的原理

上面我们已经可以无痛的使用NSFetchedResultsController了。而且各种数据都可以自动更新，但是它是一个什么原理呢？

NSFetchedResultsController的核心其实是作为一个观察者去监听NSManagedObjectContext的通知。当NSManagedObjectContext发生改变的时候NSFetchedResultsController就知道了变化。所以，我们初始化一个NSFetchedResultsController的时候，也就监听了对应的NSManagedObjectContext的通知。具体的是三个通知。

*NSManagedObjectContextObjectsDidChangeNotification*

*NSManagedObjectContextWillSaveNotification*

*NSManagedObjectContextDidSaveNotification*

其实看名字都可以猜测一些他们的具体发出通知的时机。

#### NSManagedObjectContextObjectsDidChangeNotification

当任何一个Object中的任何属性有改变的时候，会发出此通知。然后NSFetchedResultsController会去用设置好的NSFetchRequest查处结果进行参数传递。当这些改变发送的时候，我们就只用在 -controller: didChangeObject: atIndexPath: forChangeType: newIndexPath:判断改变类型是 NSFetchedResultsChangeUpdate或者 NSFetchedResultsChangeMove就可以做相应的数据到UI的变更操作了。


####NSManagedObjectContextWillSaveNotification

这个通知是在删除Object的情况下。 这时候可能删除的是section。用-controller: didChangeSection: atIndex: forChangeType:。 如果只是一个Object的删除。就用-controller: didChangeObject: atIndexPath: forChangeType: newIndexPath:。 类型都是NSFetchedResultsChangeDelete.

#### NSManagedObjectContextDidSaveNotification
这个通知对应的delegate方法就是-controller: didChangeSection: atIndex: forChangeType: 和 -controller: didChangeObject: atIndexPath: forChangeType: newIndexPath:。 



了解了NSFetchedResultsController的原理。事实上自己就可以写NSFetchedResultsController了。



事实上，这篇blog写的确实很糟糕。 而且看日期已经写了20多天了。这样的拖沓让我很不开心。 所以我决定快速的结束这篇blog。 以后就算写的糟糕也不应该拖沓的。




















































































































































































































































































