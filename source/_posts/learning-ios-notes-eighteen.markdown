title: iOS笔记(18)
date: 2013-03-29 00:04:37
tags: iOS
---

# Core Data (一)

## 序

恩，用Core Data也有一段时间了。大大小小的坑也都坑过了。重来没有认真的记录一次。这次需要好好的理一理Core Data。就当一次绝好的机会记录下来。也为了自己加深认识。

## 为什么要用Core Data

CoreData的学习是需要一定成本的。以至于我认识的人很少在用，大家要不就是用一个FMDB。或者做的App是一个已有的Web的延伸，数据直接用Web端的Api取回来就好了。

我们要用Core Data的理由有以下几点：

1. Core Data是对底层存储的一次封装。封装了以后就变成ORM的框架。这样就变成操作对象。Core Data自己去进行数据的保存。
2. 使用Core Data而不是FMDB，让整个程序架构更加的面向对象。
3. Core Data仅仅使用了Objective-C和Core Foundation，你不必去加入一些第三方的库。
4. Core Data是Apple的原生技术。每年的WWDC都能看到新特性的加入和讲授。
5. Core Data支持iCloud。而使用iCloud的App。Apple推荐的可能性增加。
6. 有了iCloud为以后Apple的全平台数据共享打下基础


所以，没有理由拒绝使用Core Data做为你App的持久化。Core Data应该是一个跟Apple混的第一选择。


<!--more-->


## 存储原理

### NSManagedObjectModel

#### 认识

我个人是把NSManagedObjectModel看做为一个core data的schema。生成这个类的来源是xCode中的Data Model

![](http://ww3.sinaimg.cn/large/bfadf3bejw1e36lv4nn89j.jpg)


点击之后会生成一个.xcdatamodeld的文件夹里面的数据具体是是用xml来存储的。
但是你在xCode里面看这个.xcdatamodeld文件夹是认为一个文件。然后可以用xCode来进行图形化的编辑。

如这样

![](http://ww1.sinaimg.cn/large/bfadf3bejw1e36m76n6k3j.jpg)

当然还有另外一种表格的方式来编辑Data Model

![](http://ww3.sinaimg.cn/large/bfadf3bejw1e36mhk7obgj.jpg)


我们编辑这个的这个文件，理论上来说只是一个资源文件。相当于数据库的schema。我们的数据库长什么样子，就是你来设计的。然后在程序中通过NSURL找到这个资源文件的位置。就可以初始化成为NSManagedObjectModel。


#### 代码加载Data Model

当我们设计好我们的Data Model以后。可以用以下代码，把Data Model加载到NSManagedObjectModel里面去。

``` 
- (void)initializeCoreDataStack
{
    NSURL *modelURL = [[NSBundle mainBundle] URLForResource:@"PPRecipes" withExtension:@"momd"];
    ZAssert(modelURL, @"Failed to find model URL");
    NSManagedObjectModel *mom = nil;
    mom = [[NSManagedObjectModel alloc] initWithContentsOfURL:modelURL]; ZAssert(mom, @"Failed to initialize model");
}
```

这样我们就从main bundle里面把data model加载到NSManagedObjectModel这个类里面去了。

### NSPersistentStoreCoordinator

NSPersistentStoreCoordinator是整个Core Data的核心。因为承担着全部数据的持久化，加载数据，缓存数据的工作。可以看做Core Data的心脏。

值得我们开心的事情是，虽然NSPersistentStoreCoordinator很重要，但是我们在写程序的时候其实只用初始化和设置一些很简单的参数就可以使用了。在我们App的整个生命周期里面其实并不会很频繁的使用到它。


#### 初始化NSPersistentStoreCoordinator

初始化NSPersistentStoreCoordinator分为两个步骤

1. 用已经初始化好的NSManagedObjectModel去初始化它
2. 选择NSPersistentStore为存储的方式


##### NSPersistentStore

理论上来说NSPersistentStoreCoordinator的数据不一定要存到硬盘上，它可以存到内存里，可以存到网络上。不过都肯定是对应着不同的应用场景。所以我们一般情况还是把它存储到磁盘上。 NSPersistentStore就是用来描述我们要把数据存到哪里的类。NSPersistentStore还可以指定存储数据的文件的文件类型。

目前知道的NSPersistentStore支持三种类型：

1. SQLite
2. 二进制
3. XML

我就只使用SQLite。理论上来说，NSPersistentStore不管选择什么样的存储文件。你后面进行Core Data的操作都是一样的。并不需要修改你的逻辑代码。这就是Core Data把底层封装了一次的好处。(书上说iOS上不建议使用XML)


有一点需要注意的是NSPersistentStoreCoordinator可以加入不止一个的NSPersistentStore。这个在某些特定场景还是很有用的，以后如果能写到在写把。

##### 代码加载NSPersistentStoreCoordinator

```
    dispatch_queue_t queue = NULL;
    queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
    dispatch_async(queue, ^{
        NSFileManager *fileManager = [NSFileManager defaultManager];
        NSArray *directoryArray = [fileManager URLsForDirectory:NSDocumentDirectory
                                                      inDomains:NSUserDomainMask];
        NSURL *storeURL = nil;
        storeURL = [directoryArray lastObject];
        storeURL = [storeURL URLByAppendingPathComponent:@"PPRecipes.sqlite"];
        NSError *error = nil;
        NSPersistentStore *store = nil;
        store = [psc addPersistentStoreWithType:NSSQLiteStoreType
                               configuration   :nil
                               URL             :storeURL
                               options         :nil
                               error           :&error];
        if (!store) {
            ALog(@"Error adding persistent store to coordinator %@\n%@",
                 [error localizedDescription], [error userInfo]);
        }
    });
```

由于这个过程是一个比较耗费资源的过程。所以我们应该把它放到后台线程里面去做。

```
NSFileManager *fileManager = [NSFileManager defaultManager];
        NSArray *directoryArray = [fileManager URLsForDirectory:NSDocumentDirectory
                                                      inDomains:NSUserDomainMask];
```

Apple中不管是OS X还是iOS能上架的App，都是沙箱机制。这个可以自己Goolge看相关的资料。这样的话每一个App都有自己的文件系统。这两句也比较常使用。就是可以获得自己沙箱中的Document文件夹路经。参数的信息直接查文档把。

接下来很重要的一句是

```
store = [psc addPersistentStoreWithType:NSSQLiteStoreType
    					configuration   :nil
    					URL             :storeURL
    					options         :nil
   						error           :&error];
```

此函数的第一个参数是指定NSPersistentStore的存储类型，我们之前说过的三种类型之一。NSSQLiteStoreType就代表这SQLite文件类型。

第二个参数比较高级。我还没有用到过。文档上写的是设置为nil的时候是把NSPersistentStoreCoordinator作为默认的配置，如果是其他参数可能是把NSPersistentStoreCoordinator设置为其他的用途。

第三个参数就是我们要存储文件的位置。刚刚我们不是获得的沙箱里面的Document文件夹路经了么。然后我们把文件名加入拼为了一个文件的完整路经传入。

第四个参数类型是一个字典，我们可以传入不同的参数来指定这个NSPersistentStore的类型。比如在本地比如在iCloud。比较高级先为nil。

最后一个参数，为error，传入一个error指针。如果NSPersistentStore初始化失败我们可以获得相应信息。


### NSManagedObjectContext

NSManagedObjectContext是我们很经常使用到的一个类。对于为什么要有这个类我个人理解是这样的，在我有限的数据库知识中记得，IO操作是很费时的，因此数据库一般情况是把一系列的操作缓存到了一个内存区域，等待合适的实际在去写入真实的磁盘中。这样大大的提高效率。如果你插入一条数据，然后修改数据，最后删除掉这条数据。如果是每次都执行Commit的话是操作三次IO，如果我们把这三条合并在一起commit的话。是任何事情都不必做。这样能有效的提高整个系统的效率。我认为NSManagedObjectContext的作用在于跟持久化直接做了这层缓存。我们使用Core Data。

还有一点需要注意的是NSManagedObjectContext并不是线程安全的。

关于NSManagedObjectContext的线程安全和高级解释，这里有两篇blog写的超好。

可以一看

[Multi-Context CoreData](http://www.cocoanetics.com/2012/07/multi-context-coredata/)

[Zarra on Locking](http://www.cocoanetics.com/2013/02/zarra-on-locking/)

初级应用的话只用记住一个就ok了NSManagedObjectContext是有一个NSManagedObjectContextConcurrencyType的属性。如果我们是UI操作（主线程操作）我们应该把它设置为NSMainQueueConcurrencyType类型的。


#### 代码加载NSManagedObjectContext

```
NSManagedObjectContext                  *moc = nil;
NSManagedObjectContextConcurrencyType   ccType = NSMainQueueConcurrencyType;
moc = [[NSManagedObjectContext alloc] initWithConcurrencyType:ccType];
[moc setPersistentStoreCoordinator:psc];
[self setManagedObjectContext:moc];

```

### 总结

![](http://ww2.sinaimg.cn/large/a7480316jw1e39dc0y8o2j.jpg)



至此，三个Core Data的核心类就已经简单说完了一次。他们的关系在上面的图片中表示的很清楚的样子。

NSPersistentStoreCoordinator从NSManagedObjectModel得到模型然后选择NSPersistentStore作为持久化目的地。然后NSManagedObjectContext在作为NSPersistentStoreCoordinator的一个缓存区给我们操作。

当然我这篇blog和接下来准备写的Core Data的可能不会是有完整代码和具体实例的文章。可能只是我对Core Data使用上的一些感悟和梳理。











