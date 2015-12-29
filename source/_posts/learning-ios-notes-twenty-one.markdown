title: iOS笔记(21) CoreData (三) NSFetchedResultsController
date: 2013-04-17 15:06:24
tags: iOS
---


# CoreData (三) 

## NSFetchedResultsController

### 什么是NSFetchedResultsController

NSFetchedResultsController是一个让人爱恨交加的一个类。如果使用得当，NSFetchedResultsController能帮组减少很多代码。如果使用不当，整个App就随时崩溃。

NSFetchedResultsController我觉得最初的设计应该是为了配合UITableView来使用的。因为UITableView在iOS的应用App中出场次数实在是太高了.而且UITableView是重要的数据展示View,所以需要频繁的向Model去请求数据,但是根据MVC来说,V不应该直接跟M联系的.这样就在Core Data下面出现了一个C--NSFetchedResultsController来把V和M协调起来. NSFetchedResultsController就是这个C. 



NSFetchedResultsController是有两个重要的功能。

第一:NSFetchedResultsController是作用在Core Data上的,通过NSFetchRequest来查询Core Data里面的数据.可以返回按照组分好的数据.这样便于UITableView来显示.

第二:但Modle改变的时候NSFetchedResultsController能及时的发出通知.准确的说,应该是当NSManagedObjectContext发生改变的时候,NSFetchedResultsController能知道这些变化,然后发出通知出来.以便UITableview能及时的更新.

<!--more-->


## 实现一个NSFetchedResultsController作为Data source的UITableView

### 创建一个最小带Core Data的工程

![](http://ww3.sinaimg.cn/large/a74ecc4cjw1e3tlc32lbhj20k80dndhr.jpg)

选择Master-Detail Application

![](http://ww4.sinaimg.cn/large/a74eed94jw1e3tle5fxqnj20k80dn0uf.jpg)


整理一下显示层级和结构使其看起来顺眼一些

![](http://ww2.sinaimg.cn/large/a74e55b4jw1e3tlnesq81j20750dut9e.jpg)


### 确立目标
打开看以后 发现建立的工程是已经使用了NSFetchedResultsController
我们的目标是改写这个项目支持UITableView分组显示

#### 首先修改Data Model

增加一个字段用来分组. 我们增加一个同样的Date用来记录此时的分钟数量.

![](http://ww4.sinaimg.cn/large/bfadf3bejw1e3tn8pouqgj.jpg)



### 初始化一个NSFetchedResultsController

这里假设你看过我的Core Data笔记1,2.默认你已经做好了Core Data stack的全部工作.然后再开始NSFetchedResultsController的初始化.

```
- (NSFetchedResultsController *)fetchedResultsController
{
    if (_fetchedResultsController != nil) {
        return _fetchedResultsController;
    }

    NSFetchRequest *fetchRequest = [[NSFetchRequest alloc] init];
    // Edit the entity name as appropriate.
    NSEntityDescription *entity = [NSEntityDescription entityForName:@"Event" inManagedObjectContext:self.managedObjectContext];
    [fetchRequest setEntity:entity];

    // Set the batch size to a suitable number.
    [fetchRequest setFetchBatchSize:20];

    // Edit the sort key as appropriate.
    NSSortDescriptor    *sortDescriptor = [[NSSortDescriptor alloc] initWithKey:@"timeStamp" ascending:NO];
    NSArray             *sortDescriptors = @[sortDescriptor];

    [fetchRequest setSortDescriptors:sortDescriptors];

    // Edit the section name key path and cache name if appropriate.
    // nil for section name key path means "no sections".
    NSFetchedResultsController *aFetchedResultsController = [[NSFetchedResultsController alloc] initWithFetchRequest:fetchRequest managedObjectContext:self.managedObjectContext sectionNameKeyPath:@"sectionMinute" cacheName:@"Master"];
    aFetchedResultsController.delegate = self;
    self.fetchedResultsController = aFetchedResultsController;

    NSError *error = nil;

    if (![self.fetchedResultsController performFetch:&error]) {
        // Replace this implementation with code to handle the error appropriately.
        // abort() causes the application to generate a crash log and terminate. You should not use this function in a shipping application, although it may be useful during development.
        NSLog(@"Unresolved error %@, %@", error, [error userInfo]);
        abort();
    }

    return _fetchedResultsController;
}
```

这里是工程里面的NSFetchedResultsController的set方法.可以看出,第一我们创建一个NSFetchRequest查询.然后在用这个NSFetchRequest去创建一个NSFetchedResultsController.

```
NSFetchedResultsController *aFetchedResultsController = [[NSFetchedResultsController alloc] initWithFetchRequest:fetchRequest managedObjectContext:self.managedObjectContext sectionNameKeyPath:@"sectionMinute" cacheName:@"Master"];
```

第一个参数就是NSFetchRequest.

第二个参数是要指定在哪个context里面进行查询

第三个参数是根据什么key来分组.sectionNameKeyPath本来是nil是不分组,我改为我们需要分组的key值"sectionMinute".

第四个参数  [官方解释是这里](http://developer.apple.com/library/ios/#documentation/CoreData/Reference/NSFetchedResultsController_Class/Reference/Reference.html)点到The Cache的地方. 我的理解是cache只保留很少的一部分数据在磁盘上面,如果使用了Cache,在重建UITableView的时候, 就优先查询cache里面的数据.然后要在-performFetch:执行的时候才会去刷新新的数据.这样有助于UITableView的流畅性.

然后我加入Sections的方法

```
- (NSString *)tableView:(UITableView *)tableView titleForHeaderInSection:(NSInteger)section
{
    NSArray *sections = [[self fetchedResultsController] sections]; id <NSFetchedResultsSectionInfo> sectionInfo = nil;
    sectionInfo = [sections objectAtIndex:section];
    
    return [sectionInfo name];
}
```

接着我添加

```
https://github.com/erica/NSDate-Extensions.git
```

这个NSDate库进来.自己按照他的写法,写一个能获得当前时间秒数为0的方法.用来分组.

```
- (NSDate *) dateAtStartOfMinutes
{
    NSDateComponents *components = [CURRENT_CALENDAR components:DATE_COMPONENTS fromDate:self];
	[components setSecond:0];
	return [CURRENT_CALENDAR dateFromComponents:components];
}
```


然后改写insert方法


```
- (void)insertNewObject:(id)sender
{
    NSManagedObjectContext *context = [self.fetchedResultsController managedObjectContext];
    NSEntityDescription *entity = [[self.fetchedResultsController fetchRequest] entity];
    NSManagedObject *newManagedObject = [NSEntityDescription insertNewObjectForEntityForName:[entity name] inManagedObjectContext:context];
    
    // If appropriate, configure the new managed object.
    // Normally you should use accessor methods, but using KVC here avoids the need to add a custom class to the template.
    [newManagedObject setValue:[NSDate date] forKey:@"timeStamp"];
    [newManagedObject setValue:[[NSDate date] dateAtStartOfMinutes]  forKey:@"sectionMinute"];
    
    // Save the context.
    NSError *error = nil;
    if (![context save:&error]) {
         // Replace this implementation with code to handle the error appropriately.
         // abort() causes the application to generate a crash log and terminate. You should not use this function in a shipping application, although it may be useful during development. 
        NSLog(@"Unresolved error %@, %@", error, [error userInfo]);
        abort();
    }
}
```

其实就是加入了

```
[newManagedObject setValue:[[NSDate date] dateAtStartOfMinutes]  forKey:@"sectionMinute"];
```
这句.

这样,简单的使用NSFetchedResultsController来显示分组的UITableView就搞定了.
当然因为建立的工程模板原因.很大一部分都是xCode搞定的.


### 被遗忘的地方

Sections数量,决定了有多少组

```
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView
{
    return [[self.fetchedResultsController sections] count];
}
```

如果在初始化NSFetchedResultsController的时候sectionNameKeyPath为nil.这里应该会返回1.(就算没有数据也会返回1)


Row数量,决定每一组分别有多少行数据.

```
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
    id <NSFetchedResultsSectionInfo> sectionInfo = [self.fetchedResultsController sections][section];
    return [sectionInfo numberOfObjects];
}
```

我前面说过NSFetchedResultsController就是为了配合UITableView而设计的.所以自然有根据indexPath来取对应的NSManagedObject的方法.


```
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"Cell" forIndexPath:indexPath];
    [self configureCell:cell atIndexPath:indexPath];
    return cell;
}

- (void)configureCell:(UITableViewCell *)cell atIndexPath:(NSIndexPath *)indexPath
{
    NSManagedObject *object = [self.fetchedResultsController objectAtIndexPath:indexPath];
    cell.textLabel.text = [[object valueForKey:@"timeStamp"] description];
}
```


### show
![](http://ww3.sinaimg.cn/large/a74ecc4cjw1e3tydp5hsqj208w0geabq.jpg)



## 总结

写Blog实在是太累了. NSFetchedResultsController努力一天也才一点点.回去继续写. 下次要写NSFetchedResultsController通知方法.


















