---
layout: post
title: "iOS笔记 (12)"
date: 2012-09-29 12:37
comments: true
tags: iOS
---

# Block简单使用

## 序

这篇blog主要想介绍这么用block做回调。因为跟之前的是想关联的。

![keynote](http://farm9.staticflickr.com/8448/7780946470_05ee760924.jpg)

这图中的Controller和View我之前已经讲过Delegate和Traget-Action了。Data source和Delegate差不多。有机会再TableView里面详细说说。当然，按照计划我这次要写block的回调。

<!--more-->

## 什么是block

老规矩wiki http://zh.wikipedia.org/wiki/块_(C语言扩展)

来自[apple](http://developer.apple.com/library/ios/#documentation/cocoa/Conceptual/Blocks/Articles/00_Introduction.html)的第一手资料。

所以block这种语法要在OS X 10.6 和 iOS4.0以后才支持。不过考虑到iOS的版本问题并不像Android一样屌丝。so可以默认block这种语法是都可以支持的。

block出现据说是为了[GCD](http://developer.apple.com/library/ios/#documentation/Performance/Reference/GCD_libdispatch_Ref/Reference/reference.html)的实现apple大力推进的。GCD(Grand Central Dispatch)简单说来就是进行多线程并行操作的一种机制。使得iOS和Mac的多线程程序编写很容易。如果你搞过Linux的多线程再来看GCD的话，会觉得生活在幸福中。

当然block的使用不仅仅只有GCD一个地方。block是一个标准Objective-C对象。因为block是对象，所以block可以作为参数传递、作为方法或函数的返回值、赋值给变量。使用block我们编写自己的接口时候就可以写的很现代。或者在我们需要暴露一些方法给其他人调用的时候，就不仅可用delegate，还可以用block这种现代的方法。

在没有Block之前，如果我们想在之后的某个时间回调一个方法，你一般会用代理或者NSNotificationCenter。 这样也不错， 除了一点，它会让你的代码到处都是——你在一个地方开启了一个任务， 然后在另外一个地方处理它的结果。如果使用Block的话，则不用遇到这样的问题。因为它能将和一个任务相关的所有代码都放在一个地方， 你马上就会看到。这样代码就可以开起来很清晰和具有现代感。

关于block，你可以在[绿皮](http://www.raywenderlich.com/9438/how-to-use-blocks-in-ios-5-tutorial-part-2)学习到更加详细的例子。

![block](http://developer.apple.com/library/ios/documentation/cocoa/Conceptual/Blocks/Art/blocks.jpg)


## 创建一个block

我以为在Objective-C里面,因为是C的超集。为了扩张出各式各样的面向对象的功能。就加入了各种特殊的符号。比如常见的@用来告知这是一个对象等等。所以，我觉得block也用了一个特殊的符号来开头的话，符合一贯作风。我们的block就使用「^」来表示一个block。so如果你在代码里面看见有「^」符号的话。肯定你看见了一个block。

### 声明block引用

```objc
void (^blockReturningVoidWithVoidArgument)(void);
int (^blockReturningIntWithIntAndCharArguments)(int, char);
void (^arrayOfTenBlocksReturningVoidWithIntArgument[10])(int);
```

**小技巧**

如果我自己定义了一个block并且想让他使用到多处的话。可以这样写

```objc
typedef float (^MyBlockType)(float, float);
 
MyBlockType myFirstBlock = // ... ;
MyBlockType mySecondBlock = // ... ;
```

### 实现block
刚刚知道了怎么定义block，定义好了以后。我们就可以在合适的地方来实现这个block

```objc
int (^oneFrom)(int);
 
oneFrom = ^(int anInt) {
    return anInt - 1;
};
```

## block的应用场景

### 调用block

如果是声明了一个block变量的话。可以直接调用block，如下面的两个例子。

```objc
int (^oneFrom)(int) = ^(int anInt) {
    return anInt - 1;
};

printf("1 from 10 is %d", oneFrom(10));
// Prints "1 from 10 is 9"

float (^distanceTraveled)(float, float, float) =
    ^(float startingSpeed, float acceleration, float time) {
    float distance = (startingSpeed * time) + (0.5 * acceleration * time * time);
    return distance;
};

float howFar = distanceTraveled(0.0, 9.8, 1.0);
// howFar = 4.9
```

### 在C函数中使用block作为参数
在平时使用中其实系统里面有很多函数已经是用block作为参数的了。我们不必去定义block。下面用qsort_b来作为例子(qsort_b跟qsort_r是类似的快速排序函数，只是最后一个参数用了block)

```
char *myCharacters[3] = { "TomJohn", "George", "Charles Condomine" };

qsort_b(myCharacters, 3, sizeof(char *), ^(const void *l, const void *r) {
            char *left = *(char **)l;
            char *right = *(char **)r;
            return strncmp(left, right, 1);
        });
// Block implementation ends at "}"

// myCharacters is now { "Charles Condomine", "George", "TomJohn" }
```

可以明显的看出，使用block参数的优势在于调用函数的地方可以自己处理一些事情。这样同样的函数，在细节上有不通处理的时候，使用block将变的更方便直接。

当然，其实在iOS编程中，我目前使用最多的C函数带有block肯定是GCD里面的函数。使用dispatch_apply来举例子

**函数原型**

```
void dispatch_apply(size_t iterations, dispatch_queue_t queue, void (^block)(size_t));
```

**例子**

```
#include <dispatch/dispatch.h>
size_t           count = 10;
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);

dispatch_apply(count, queue, ^(size_t i) {
                   printf("%u\n", i);
               });
```

上面这个例子可以应用到需要放到多线程里面的循环


### 在Cocoa方法中使用block作为参数

当然，我们可以在cocoa方法中使用block。

下面是我学习中的例子。第一个函数是声明一个带block的方法。后面的是Action方法。展示怎么调用一个自己声明的带block参数的方法。

```
- (NSString *)Calculator:(NSString * (^)(double one, double two))block
{
    return block([self.oneNumber.text doubleValue], [self.twoNumber.text doubleValue]);
}


- (IBAction)add:(id)sender
{
    self.lable.text = [self Calculator:^NSString * (double one, double two) {
                           return [NSString stringWithFormat:@"%lf", one + two];
                       }];
}
```

这个例子是我能想到最简单的例子了。需要注意的是(NSString * (^)(double one, double two))声明的时候需要多一对()括号。不然会报错。而调用时候则不用。而且调用和声明时候^的位置区别。

复杂一些的例子

```
NSArray *array     = [NSArray arrayWithObjects:@"A", @"B", @"C", @"A", @"B", @"Z", @"G", @"are", @"Q", nil];
NSSet   *filterSet = [NSSet setWithObjects:@"A", @"Z", @"Q", nil];

BOOL (^test)(id obj, NSUInteger idx, BOOL * stop);

test = ^ (id obj, NSUInteger idx, BOOL * stop) {
    if(idx < 5) {
        if([filterSet containsObject:obj]) {
            return YES;
        }
    }
    return NO;
};

NSIndexSet *indexes = [array indexesOfObjectsPassingTest:test];

NSLog(@"indexes: %@", indexes);

/*
 * Output:
 * indexes: <NSIndexSet: 0x10236f0>[number of indexes: 2 (in 2 ranges), indexes: (0 3)]
 */

```

NSSet里面自带的函数例子, 遍历Set

```
__block BOOL found   = NO;
NSSet        *aSet   = [NSSet setWithObjects:@"Alpha", @"Beta", @"Gamma", @"X", nil];
NSString     *string = @"gamma";

[aSet enumerateObjectsUsingBlock:^(id obj, BOOL * stop) {
     if([obj localizedCaseInsensitiveCompare:string] == NSOrderedSame) {
         *stop = YES;
         found = YES;
     }
 }];

// At this point, found == YES
```



## 补漏

貌似有一个重要的关键字忘记提了__blcok.这个让你的变量能在block修改。具体内容在第一段里面的链接里面都有。这里只是提一下。

## 优点 vs 缺点

优点：个人觉得就是上面提到过的使用block参数的优势在于调用函数的地方可以自己处理一些事情。这样同样的函数，在细节上有不通处理的时候，使用block将变的更方便直接。而且让GCD得以足够简单的方法进行多线程的编写。GCD让多线程编程从来没有想过如此之简单易用。

缺点：学习有一定曲线。闭包概念难懂。



























