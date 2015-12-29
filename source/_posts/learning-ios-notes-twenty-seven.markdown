title: iOS学习笔记(27) iCloud(三) key-Value Stroe
date: 2013-09-02 22:21:13
tags: iOS
---


## 什么是NSUserDefaults

NSUserDefaults的持久化本体是一个[plist](http://zh.wikipedia.org/zh/Plist).内存单例是一个操作类似NSDictionary的类。 而NSUserDefaults的出现我想是因为在每一个程序当中。我们都会设定一些选项。比如桌面壁纸、声音大小、提醒日期等，我们希望就算App关闭以后。我们再次打开的时候还在的东西。但是它并不适合存储App中关键的内容和用户自己产生的大量数据。 大量数据应该用更加合理的方式去做持久化(Document or Core Data).


<!--more-->

### NSUserDefaults简单代码讲解

使用NSUserDefaults其实巨简单。 

大概步骤如下:

1.  首先通过一个单例获得持久化plist的内存映射。
2.  然后就可以用这个单例的实例类进行读写操作。
3.  读当然没啥问题，但是写了的话这时候只是操作了在内存里面的这个单例实例。需要做持久化的动作。为什么这个动作要自己来做呢。我想是持久化都是进行IO操作。而IO操作其实很多时候就是性能的瓶颈所在。我们可能很短的时间内一次性操作很多次NSUserDefaults。这样就只用在结束的时候保存一次。IO操作就很少。反之如果每次写NSUserDefaults的时候都去做持久化。那刚刚的情况就会在很短的时间内操作多次IO。这是应该避免的。

```
// 首先把数据从plist里面读到内存里面的单例来
NSUserDefaults *standardDefaults = [NSUserDefaults standardUserDefaults];

// 进行增加or修改操作
[standardDefaults setObject:@"a123456789" forKey:@"userID"];
[standardDefaults setInteger:24 forKey:@"age"];
[standardDefaults setBool:YES forKey:@"isLogin"];

// 删除操作
[standardDefaults removeObjectForKey:@"debts"];

// 从内存里面写入plist进行持久化
[standardDefaults synchronize];

// 读取操作
NSString *userID = [standardDefaults stringForKey:@"userID"];
BOOL isLogin = [standardDefaults boolForKey:@"isLogin"];
```

当然还有现代一点的写法。下面两种都是一个效果

```
// 常见方法
NSUserDefaults *standardDefaults = [NSUserDefaults standardUserDefaults];
if ([standardDefaults stringForKey:@"favoriteColor"] == nil) {
[standardDefaults setObject:@"Green" forKey:@"favoriteColor"];
[standardDefaults synchronize];
}

// 现代高端上档次方法
NSUserDefaults *standardDefaults = [NSUserDefaults standardUserDefaults];
[standardDefaults registerDefaults:@{@"favoriteColor": @"Green"}];
[standardDefaults synchronize];
```

## iCloud的Key-Value

简单的，可以把iCloud的key-value当作一个在云端的NSUserDefaults。

我的用法是，App的Setting最终设置决定的还是NSUserDefaults。iCloud的Key-Value作为数据源来对NSUserDefaults进行修改。这样的优点在于，就算iCloud关闭或者iCloud的数据没有同步回来。你的App依然可以正常的工作和运行。

```
NSUserDefaults *standardDefaults = [NSUserDefaults standardUserDefaults];
```
对应

```
NSUbiquitousKeyValueStore* store = [NSUbiquitousKeyValueStore defaultStore];
```

其他操作跟NSUserDefaults一样一样的。

只有一个值得注意的是NSUbiquitousKeyValueStore需要去监听
NSUbiquitousKeyValueStoreDidChangeExternallyNotification事件。就可以知道NSUbiquitousKeyValueStore是否已经同步更新完成。

```
NSUbiquitousKeyValueStore* store = [NSUbiquitousKeyValueStore defaultStore];
[[NSNotificationCenter defaultCenter] addObserver:self
          selector:@selector(updateKVStoreItems:)
          name:NSUbiquitousKeyValueStoreDidChangeExternallyNotification
          object:store];
[store synchronize];
```

然后实现updateKVStoreItems:方法

```
- (void)updateKVStoreItems:(NSNotification*)notification {
   // Get the list of keys that changed.
   NSDictionary* userInfo = [notification userInfo];
   NSNumber* reasonForChange = [userInfo objectForKey:NSUbiquitousKeyValueStoreChangeReasonKey];
   NSInteger reason = -1;
 
   // If a reason could not be determined, do not update anything.
   if (!reasonForChange)
      return;
 
   // Update only for changes from the server.
   reason = [reasonForChange integerValue];
   if ((reason == NSUbiquitousKeyValueStoreServerChange) ||
         (reason == NSUbiquitousKeyValueStoreInitialSyncChange)) {
      // If something is changing externally, get the changes
      // and update the corresponding keys locally.
      NSArray* changedKeys = [userInfo objectForKey:NSUbiquitousKeyValueStoreChangedKeysKey];
      NSUbiquitousKeyValueStore* store = [NSUbiquitousKeyValueStore defaultStore];
      NSUserDefaults* userDefaults = [NSUserDefaults standardUserDefaults];
 
      // This loop assumes you are using the same key names in both
      // the user defaults database and the iCloud key-value store
      for (NSString* key in changedKeys) {
         id value = [store objectForKey:key];
         [userDefaults setObject:value forKey:key];
      }
   }
}
```

超级简单吧。

## 第三方库推荐

当然有大神写的第三方库。并且实现了NSUserDefaults白名单功能。因为可能你存在NSUserDefaults里面的东西不想要也不需要全部同步到NSUbiquitousKeyValueStore上去把。

对了忘记说一点NSUbiquitousKeyValueStore可是有大小和条目限制的。你不要把他当作无穷无尽的东西来用。具体限制是最大空间1 MB。 最多1024个key。记住不要拿来当作主要数据存储哦。

MK大神应该还是如雷贯耳的把。 不知道么。 MKNetworkKit是他写的。还不知道么。
[iOS 6 Programming Pushing the Limits: Advanced Application Development for Apple iPhone, iPad and iPod Touch](http://www.amazon.com/gp/product/1118449959/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1118449959&linkCode=as2&tag=blogmugunthku-20) 可是他写的哦。我和我的小伙伴都从MK大神的书里面学到很多不错的东西

[MKiCloudSync](https://github.com/MugunthKumar/MKiCloudSync)


[DDiCloudSync](https://github.com/Daij-Djan/DDiCloudSync)


[FTiCloudSync](https://github.com/futuretap/FTiCloudSync)

## 总结


参考资料：

[Preferences and Settings Programming Guide](https://developer.apple.com/library/ios/documentation/cocoa/Conceptual/UserDefaults/Introduction/Introduction.html)

[NSUserDefaults (plist) 筆記](http://gibuloto.com/blog/nsuserdefaults/)

[Back to Basics: Forgotten NSUserDefaults](http://www.doubleencore.com/2013/03/back-to-basics-forgotten-nsuserdefaults/)

[Synchronizing iPhone iOS 5 Key-Value Data using iCloud](http://www.techotopia.com/index.php/Synchronizing_iPhone_iOS_5_Key-Value_Data_using_iCloud)

[Sync Preference Data With iCloud](http://useyourloaf.com/blog/2011/10/24/sync-preference-data-with-icloud.html)

[Beginning iCloud in iOS 5 Tutorial Part 1](http://www.raywenderlich.com/6015/beginning-icloud-in-ios-5-tutorial-part-1)
