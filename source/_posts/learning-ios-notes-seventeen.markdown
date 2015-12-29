title: iOS笔记(17)
date: 2013-03-11 23:18:49
tags: iOS
---

#iOS读写文件

## 序
由于iOS App的机制和限定，我们在App里面的权限就仅限于App内部。这个打包好的内部称为沙箱。沙箱有利有弊。我觉得这个世界上没有绝对的好坏。虽然沙箱的作用限制了一些功能的实现。但是也确保了iPhone的安全机制。对于普通用户来说我觉得的利大于弊的。(MAS上架的软件也接受这一约束)


## 第一步 路径
不管是读文件还是写文件我们都要需要知道文件的位置。这个位置在iOS里面就是沙箱的Document文件夹的位置。关于沙箱里面各个文件夹的功能和作用。Apple的某文档里面写的很清楚建议Google以后详细查看（懒得去找来贴了）。获取代码如下：

``` objc
NSArray  *paths              = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
NSString *documentsDirectory = [paths objectAtIndex:0];
```

NSDocumentDirectoryz这个参数你点过去可以找到一堆类似的定义比如常用的NSLibraryDirectory，NSApplicationDirectory。这样就可以直接获取到对应的文件夹路径。其他的你照抄就好。想知道意思就点过去看呗。其实看变量名也可以猜测一二。

``` objc
NSString *filePath = [documentsDirectory stringByAppendingString:@"/hello.txt"];
```

然后我们加上我们文件名字构成一个完整的路径。注意文件名字前有一个/。

这样我们就获得一个文件路径了。

<!--more-->

## 第二步 检查文件是否存在

我们有了文件的路径以后，想读取。是可以。但是要是文件不存在，我们硬来就读取不到任何东西。所以，一般的做法是在读取之前去检查一下这个路径的文件是否存在。如果不存在我们就可以作出相应的处理动作。

``` objc
NSFileManager *fileManager = [NSFileManager defaultManager];
//检查文件是否存在
if([fileManager fileExistsAtPath:filePath]) {
	// 做存在的事情
}else{
	// 做不存在的事情
}
```

## 第三步 读取文件

如果文件存在我们就可以读出文件内容。常用以下几种

``` objc
//读取为二进制
NSData *myData = [NSData dataWithContentsOfFile:filePath];
if(myData) {
    // do something useful
}
//读取为String
NSString *myString = [[NSString alloc] initWithContentsOfFile:filePath encoding:NSStringEncodingConversionAllowLossy error:nil];
if(myString) {
	// do something useful
}
//如果是一个Plist，可以读取为字典
NSDictionary *myDictionary = [[NSMutableDictionary alloc] initWithContentsOfFile:filePath];
if(myDictionary) {
	// do something useful
}
```

## 第四步 写入文件

如果我们有内容要存入文件。常用一下几种

``` objc
//写入二进制内容
[myData writeToFile:filePath atomically:YES];
//写入String
[myString writeToFile:filePath atomically:YES encoding:NSStringEncodingConversionAllowLossy error:nil];
//写入Plist
[myDictionary writeToFile:filePath atomically:YES];
```

值得注意的是参数中YES的意思为如果为YES则保证文件的写入原子性,就是说会先创建一个临时文件,成功了就改名为正式的文件。如果为NO,直接写入目标文件目录里.writeToFile:atomically:的这个方法都为覆盖。如果想要是追加的效果。建议先读出原文件内容然后在后面加入新内容。最后一起写入。这样就覆盖了老的文件。



## 总结

之前一直想写的blog想深入一些。以至于不敢动手开始写。总觉得还没有准备好。自己也懂的不够透彻。这样不知不觉欠了几十篇blog了。在todo list里面都要变成谍中谍了。最近关注了几个微信公众帐号。见到人家天天画时间去更新。比我优秀的人比我更加努力。惭愧的很。所以现在就拿着简单的内容写起来。

	故不积 跬步，无以至千里；不积小流，无以成江海。






