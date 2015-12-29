title: iOS笔记 (15) 
date: 2013-02-22 18:31:10
tags: iOS
---

# Mogenerator的初级使用

## 什么是Mogenerator

最近在用Core Data来进行开发。Core Data其实封装的很好了。把存储底层都屏蔽了（sqlite，xml，内存）。不管是用那种方式存储下来。用Core Data都是在操作对象了。但是Core Data一套东西下来，单是学习就学习的泪流满面。天资有限，只能找一些看上去更简单的第三方类库来使用。

github上找了很久，最后在使用的是[MagicalRecord](https://github.com/magicalpanda/MagicalRecord)。不为其他，只是用起来很爽。特别是目前升级到了2.1版本以后，保存的方法名字不那么逆天了。更是好用多了。

而[Mogenerator](http://rentzsch.github.com/mogenerator/)是什么东西呢。

它漂亮的主页上是这样写的：

mogenerator为你定义了的Core Data生成默认的数据类。与xCode不一样的是(xCode一个Entity只生成一个NSManagedObject的子类)，mogenerator会为每一个Entity生成两个类。一个为机器准备，一个为人类准备。为机器准备的类一直去匹配data model。为人类准备的类就给你轻松愉快的去修改和保存。

<!--more-->

## 为什么需要Mogenerator


## 安装Mogenerator

Mogenerator其实是一个命令行的工具，因此也就可以轻松愉快的用[homebrew](http://mxcl.github.com/homebrew)去安装。

```
$ brew install mogenerator
```

升级

```
$ brew update && brew upgrade mogenerator
```

## 在项目里面添加Mogenerator

按照[这篇](http://raptureinvenice.com/getting-started-with-mogenerator/)的教程我添加了脚本以后运行不成功

```
cd testMogenerator
mogenerator -m myDataBase.xcdatamodeld/myDataBase.xcdatamodel/
```
提示:

	/Volumes/Data/iYu/Library/Developer/Xcode/DerivedData/testMogenerator-adwdmrbsvjjxqtawmqvnhfdhubts/Build/Intermediates/testMogenerator.build/Debug-iphonesimulator/Mogenerator.build/Script-5B80B84A16D9EC8F00E8E3A3.sh: line 3: mogenerator: command not found Command /bin/sh failed with exit code 127
	
自己打命令测试Mogenerator是成功的

![测试Mogenerator](http://ww2.sinaimg.cn/large/a74ecc4cjw1e24hzzauvbj.jpg)
	
	
xCode使用的是/bin/sh,我怀疑是用homebrew安装的以后的Path跟xCode的/bin/sh/执行的

思考以后在命令之前之前导入mogenerator的路径。 由于我是用homebrew按照的自然在homebrew的路径下面

```
cd testMogenerator
export PATH="/usr/local/Cellar/mogenerator/1.27/bin:$PATH"
mogenerator -m myDataBase.xcdatamodeld/myDataBase.xcdatamodel/
```


这次mogenerator是找到了，但是xcadatamodeld又没有找到。看到

	mogenerator: error loading file at myDataBase.xcdatamodeld/myDataBase.xcdatamodel/: no such file exists Command /bin/sh failed with exit code 66
	
pwd出来路径观察以后把cd去掉

```
export PATH="/usr/local/Cellar/mogenerator/1.27/bin:$PATH"
mogenerator -m myDataBase.xcdatamodeld/myDataBase.xcdatamodel/
```

这次就对了


## 使用Mogenerator

我创建了一个叫testMogenerator的工程并且在工程的data model名字叫myDataBase 如图：

![](http://ww2.sinaimg.cn/large/a74ecc4cjw1e25ps57szwj.jpg)


mogenerator安装以后是这样的

![](http://ww4.sinaimg.cn/large/a74e55b4jw1e25pnfghgpj.jpg)

然后安command+B 运行一下这个target。

运行以后生成的文件在这里

![](http://ww4.sinaimg.cn/large/a74ecc4cjw1e25pk5ribkj.jpg)

PS: 第一次生成的文件位置不对。所以脚本又改了一下

```
cd testMogenerator
export PATH="/usr/local/Cellar/mogenerator/1.27/bin:$PATH"
mogenerator -m ../myDataBase.xcdatamodeld/myDataBase.xcdatamodel/
```

每一个Entity生成了两个类 一个Entity的名字的类， 一个下划线Entity的名字的类。_XXXX.*这个类不要去修改，修改XXXX这个类就好了。

如果是一个新的entity，需要你自己加入自己的工程里面工程，如果Entity已经加入现在只是更新了Entity的话。就不用加入工程了。

![](http://ww3.sinaimg.cn/large/a74eed94jw1e25q0ub23qj.jpg)


最后编译的时候还是不对 脚本继续改 囧


```
cd testMogenerator
export PATH="/usr/local/Cellar/mogenerator/1.27/bin:$PATH"
mogenerator -m ../myDataBase.xcdatamodeld/myDataBase.xcdatamodel/ --template-var arc=YES
```


## 总结

断断续续折腾了两天。大概明白了mogenerator怎么一个事情。但是好用好还有带研究。这篇blog先发吧。不然又难产了。









