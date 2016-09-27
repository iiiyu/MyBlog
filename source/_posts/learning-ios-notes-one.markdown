---
layout: post
title: "iOS笔记 (1)"
date: 2012-02-28 22:46
comments: true
tags: iOS
---

## iOS 系统架构


#### iOS
	
	Cocoa Touch
    Media
    Core Services
    Core OS
        
#### Core OS
	OSX kernel
	Power Management
	Mach 3.0
	Keychain Access
	BSD
	Certificates
	Sockets
	File System
	Security 
	Bonjour  


#### Core Services
	Collections 
	Core Location
	Address Book
	Net Services
	networking 
	Threading
	File Access
	Preferences
	SQLite
	URL Utilities


#### Media
	Core Audio
	JPEG PNG TIFF
	OpenAL 
	PDF
	Audio Mixing 
	Quartz(2D)
	Audio Recording
	Core Animation
	Video Playback
	OpenGL ES


#### Cocoa Touch
	Multi-Touch
	Alerts
	Core Motion
	Web View
	View Hierarchy
	Map Kit
	Localization
	Image Picker
	Controls
	Camera


<!--more-->

## MVC

>Model Controller View

斯坦福老头的课上是这么定义的：

	model = What your application is (but not how it is displayed)
	controller = How your model is presented to the user (UI logic)
	view = Your Controller's minions
这个定义太经典了。

以本人乡村洋文的水平理解如下：

	model里面有你应用需要的数据啥的。（只有东西但是不知道怎么显示）

	controller帮你把model里面的东西用UI logic给呈现给用户

	view是什么呢，view是controller呈现数据时候负责跑腿的狗腿子。

在MVC里面有下面的规矩	
	
	controllers一直是直接跟他们的model进行交流。也跟他们的view直接交流。
	
	model跟view是永远不知道对方的。（MVC精髓之一）

下面我来说一个故事：

   controller一直叫唤狗腿子view，叫view干啥view就干啥。某天，view出事情了，这时候view想在出事情的时候通知一下controller主子。controller主子也不是没有人性，在controller设置一个target。这时候要是出现了什么事情view这个狗腿子要发一个action给主子controller就好了。有时候view狗腿子要和主子controller步调保持一致（synchronize 同步）这个时候要怎么做呢。在controller里面设置view狗腿的delegate（代表）这个delegate就通过一个protocol来设置（协议。哈哈 objective-c的语法出现了。）

   view狗腿是不会拥有显示的数据，如果狗腿需要数据的时候数据会通过协议来取得。controllers几乎都是data source（但不是model），因为controller是把model里面的信息格式化以后给view的。
那model能直接跟controller交流么？答案是不，model必须独立于UI之外。（好一个不为五斗米折腰）。
那model有一些忠义之言（信息更新）要进谏给controller怎么办。
model就自己想办法弄了一个广播电台（类似broadcast mechanism广播机制。PS：学过设计模式的童鞋还hold住么）controllers 或者其他model就可以“收听”到感兴趣的内容。view可能也有“收听”这个功能，但是很可能收到的不是model这个台。

MVC就是这样一个模式。

至此，乡村版的MVC介绍完毕。如果发现写的太烂直接导致看不懂。那就听斯坦福老头的第一课。

## Objective-C简介

本来自己整理了一些，然后看到有一篇写的超好，就搞过来了。原帖也找不到了，如果侵犯了原作者，请联系我。

#### 一句话

	首先Objective-C是C的一个超集。

	其次Objective-C是一个面向对象的语言。

	#import = #include

	在头文件定义的都是公共的（方法 or 变量）
	
	在m文件里面定义的都是私有的（方法 or 变量）
	
	@property 这个后面的变量在声明的时候就一起声明了两个方法（getter setter）



#### 第一节 总括
  这一节是对Objective-C(以后简称ObjC)的简要介绍,目的是使读者对ObjC有一个概括的认识。
  
* 面象的读者：在阅读本文之前,应具备使用与C类似的编程语言(如C,C++,JAVA)的一些经验,同时熟悉面向对象编程。  
        
* ObjC简介：ObjC是以SmallTalk为基础，建立在C语言之上，是C语言的超集。20世纪80年代早期由 Brad J.Cox设计,2007年苹果公司发布了ObjC 2.0,并在iPhone上使用ObjC进行开发。
* ObjC学习内容：习的内容主要包括语法和Cocoa框架两部分。本文主要对语法进行介绍。
* IDE：编写ObjC程序最主要的编译环境是Xcode,它是苹果官方提供的IDE,官网中的SDK包括Xcode,可以通过下载SDK来获得它。但是Xcode只支持MacOS X,所以如果要在其它环境下编写ObjC程序,要使用其它IDE。Linux/FreeBSD用GNUStep,Windows NT5.x(2000,XP)要先安装cywin或mingw,然后安装GNUStep。同时仅仅通过文本编辑器,GCC的make工具也可以用于开发。
注:如果要使用到Cocoa的话，只能在Apple公司的Xcode上。  
* 框架： ObjC编程中主要用到的框架是Cocoa,它是MacOS X中五大API之一,它由两个不同的框架组成FoundationKit 和ApplicationKit。 Foundation框架拥有100多个类,其中有很多有用的、面向数据的低级类和数据类型,如NSString,NSArray, NSEnumerator和NSNumber。ApplicationKit包含了所有的用户接口对象和高级类。这些框架本文不做重点介绍,如果要深入了解可以去看Xcode自带的文档。
* 特别之处：初次接触ObjC时,会发现许多和其它语言不同的地方,会看到很多的+,-,[ ,] ,@, NS等符号,这些符号在以后的编程中将经常看到,这部分内容在第二节中介绍。先熟悉一下ObjC的代码

``` objc
#import "ClassA.h"
#import <stdio.h>
            
int main( int argc, const char *argv[] ) {
	ClassA *c1 = [[ClassA alloc] init];
    ClassA            *c2 = [[ClassA alloc] init];
            
            
    //            print count
    printf(            "ClassA count: %i\n", [ClassA initCount] );
             
    ClassA            *c3 = [[ClassA alloc] init];
            
            
    //            print count again
    printf(            "ClassA count: %i\n", [ClassA initCount] );
            
            
    [c1            release];
    [c2            release];
    [c3            release];
             
    return            0;
}
```

  除了这些语言要素上的不同,ObjC也提供了一些很好的特性,如类别,扮演(Posing)等,这些在运行时的特性使得编程更加灵活。
  
  * 优缺点: 每一个语言都有其优缺点,ObjC也不例外,这就要求在选择语言时权衡利弊。对于ObjC,只要善于利用它的优点,你会发现它是一个简单,灵活,高效的语言。以下列举了它的一些特点:
  
    优点: 类别、扮演(Posing)、动态类型、指针计算、弹性信息传递、不是一个过度复杂的c衍生语言、可通过Objective-c++与c++结合
    
    缺点: 没有命名空间、没有操作符重载、不像c++那样复杂








#### 第二节对C的扩展




1.扩展名

  ObjC是ANSI版本C的一个超集,它支持相同的C语言基本语法。与C一样,文件分为头文件和源文件,扩展名分别为.h和.m。如果要加入c++的语法,需要用到.mm,这里不做介绍。




    .h 头文件。头文件包涵类的定义、类型、方法以及常量的声明
         
    .m 源文件。这个典型的扩展名用来定义源文件，可以同时包含C和Objective-C的代码。
          


2.#import   

在ObjC里,包含头文件有比#include更好的方法#import。它的使用和#include相同,并且可以保证你的程序只包含相同的头文件一次。相当于#include+ #pragma once的组合。
例如要包含Foundation框架中的Foundation.h文件,可以像下面这样。
   
   ``` c                 
   #import<Foundation/Foundation.h>
   ```      
   
   注:每个框架有一个主的头文件,只要包含了这个文件,框架中的所有特性都可以被使用。




3.@符号
        
   @符号是ObjC在C基础上新加的特性之一。常见到的形式有@”字符串”,%@ , @interface,@implement等。@”字符串”表示引用的字符串应该作为Cocoa的NSString元素来处理。@interface等则是对于C的扩展,是ObjC面向对象特性的体现。
注:这里提一个小技巧,只要看到@符号,就可以认为它是对于C的一个扩展。




4.NSLog()

   在ObjC中用的打印函数是NSLog(),因为ObjC是加了一点”特殊语料”的C语言,所以也可以用printf()但是NSLog()提供了一些特性,如时间戳,日期戳和自动加换行符等,用起来更方便,所以推荐使用NSLog()。下面是两种输出的对比。
   
   使用NSLog()输出任意对象的值时,都会使用%@格式说明。在使用这个说明符时,对象通过一个名为description的方法提供自己的NSLog()格式。
   
  下面分别是使用NSLog()和使用printf()的相应输出:

     2010-10-15 14:54:21。42610_15[1973:207] Hello World!
     
     Hello World!
         

注:NS前缀告诉你函数来自Cocoa而不是其他工具包。




5.BOOL
   
   BOOL是ObjC中的布尔类型,它和C中的bool有如下区别

                                                        
    BOOL YES(1),NO(0)
         
    bool true(!0),false(0)
         

6.id
    
   这是ObjC新加的一个数据类型,它是一般的对象类型,能够存储任何类型的方法。




7.nil

   在ObjC中,相对于C中的NULL,用的是nil。这两者是等价的。下面是nil的定义。

	#define nil NULL
         



#### 第三节创建对象




1.接口和实现

在ObjC中定义一个类需要有两个部分:接口和实现。接口文件包含了类的声明,定义了实例变量和方法。实现文件包含了具体的函数的实现代码。下图显示了一个叫MyClass的类,它继承自NSObject基类。类的定义总是从@interface开始到@end结束。在类名后面的是父类的名称。实例变量被定义在两个花括号之间。在实例变量下面的是方法的定义。一个分号用来结束一个变量或者方法。

下面的代码显示了MyClass这个类的实现代码。就像类的定义规则一样,类实现文件也被两个标识框起来,一个是@implementation,还有一个是@end。这两个指令标识符告诉编译器程序从哪里开始编译到哪里结束。类中的方法名称的定义和它接口文件中的定义是一样的,除了实现文件中有具体的代码以外。

``` c
@implementation MyClass
-(id)initWithString:(NSString *) aName
{
    if (self = [super init]) {
        count count = 0;
        data = nil;
        name = [aName copy];
        return self;
    }
}
+(MyClass *)createMyClassWithString: (NSString *) aName
{
    return [[[self alloc] initWithString:aName] autorelease];
}
@end
```


               
            
         
   当你要把一个对象保存进变量，要使用指针类型。ObjC同时支持强和弱变量对象。强类型对象在变量类型定义的时候包含了类名。弱对象使用id类型作为实例变量。下面的例子同时显示了定义MyClass中的强弱两种类型的变量

``` c
MyClass* myObject1;    // Strong typing
id myObject2;    // Weak typing
```


2.方法
        
   一个方法定义包含了方法类型，返回类型，一个或者多个关键词，参数类型和参数名。在ObjC中一个类中的方法有两种类型：实例方法，类方法。实例方法前用(-)号表明,类方法用(+)表明,通过下图可以看到,前面有一个(-)号,说明这是一个实例方法。

   在ObjC中，调用一个方法相当于传递一个消息，这里的消息指的是方法名和参数。所有的消息的分派都是动态的，这个体现了ObjC的多态性。消息调用的方式是使用方括号。如下面的例子中，向myArray对象发送insertObject:atIndex:这个消息。



``` c
[myArray insertObject:anObj atIndex:0];
```        
这种消息传递允许嵌套

``` c
[[myAppObject getArray] insertObject:[myAppObject getObjectToInsert] atIndex:0];
```

前面的例子都是把消息传递给实例变量，你也可以把消息传递给类本身。这时要用类方法来替代实例方法 。你可以 把他想象成静态C++类（当然不完全相同）。
类方法的定义只有一个不一样那就是用加号（+）代替减号（-）。下面就是使用一个类方法。

``` c
NSMutableArray* myArray = nil;    // nil is essentially the same as NULL
//            Create a new array and assign it to the myArray variable.
myArray = [NSMutableArray arrayWithCapacity:0];
```

3.属性
      
   属性提供了比方法更方便的访问方式。通过property标识符来替代getter和setter方法。使用方法就是在类接口文件中用@property标识符，后面跟着变量的属性，包括 copy, tetain, assign ,readonly , readwrite,nonatomic，然后是变量名。同时在实现文件中用@synthesize标识符来取代getter和setter方法。

``` c
@property BOOL flag;
@property (copy) NSString* nameObject;              
//            Copy the object during assignment.
@property (readonly) UIView* rootView;  // Create only a getter method
```

接口文件中使用@property

``` objc
@synthesize            flag,nameObject,rootView;
```         

实现文件中使用@synthesize

属性的另一个好处就是，可以使用点（.）语法来访问，如下所示：
   
```              
  myObject.flag = YES;
  CGRect viewFrame = myObject.rootView.frame;
```        






#### 第四节继承

 继承的语法如下，冒号后的标识符是需要继承的类。

```
 @interface            Circle : NSObject
```         



1.不支持多继承

  要注意的是ObjC只支持单继承，如果要实现多继承的话，可以通过类别和协议的方式来实现，这两种方法将在后面进行介绍。
  
2.Super关键字

ObjC提供某种方式来重写方法，并且仍然调用超类的实现方式。当需要超类实现自身的功能，同时在前面或后面执行某些额外的工作时，这种机制非常有用。为了调用继承方法的实现，需要使用super作为方法调用的目标。下面是代码示例：

```
@implementation Circle
-(void)setFillColor: (ShapeColor) c
{
    if(c== kRedColor){
        c = kGreenColor;
    }
    [super setFillColor: c];
}
@end
```
Super来自哪里呢？它既不是参数也不是实例变量，而是由ObjC编译器提供的某种神奇功能。向super发送消息时，实际上是在请求ObjC向该类的超类发送消息。如果超类中没在定义该消息，ObjC将按照通常的方式在继承链中继续查找对应的消息。


#### 第五节 对象初始化

1.分配与初始化

   对象的初始化有两种方法：一种是[类名new], 第二种是[[类名 alloc]init]。这两种方法是等价的，不过，通常的Cocoa惯例是使用alloc和init,而不使用new.一般情况下，Cocoa程序员只是在他们不具备足够的水平来熟练使用alloc和init方法时，才将new作为辅助方法使用。
         
   [[类名alloc]init]有两个动作。alloc是分配动作，是从操作系统获得一块内存并将其指定为存放对象的实例变量的位置。同时，alloc方法还将这块内存区域全部初始化为0。与分配动作对应的是初始化。有如下两种初始化写法。

```
Car *car = [[Class alloc] init];
//写法1                  
Car *car = [Car alloc];
[car init];
//写法2
```
应该使用第一种写法，因为init返回的对象可能不是以前的那个。

2.编写初始化方法

下面是一段初始化的代码

```
-(id)init
{
    if(self = [super init]){
        engine = [Engine new];
        …
            }
}
```
使用self= [super init]的作用是使超类完成它们自己的初始化工作。同时因为init可能返回的是不同的对象，实例变量所在的内存位置到隐藏的self参数之间的跳离又是固定的，所以要这样使用。
注：这部分可以参考书[1]144页。




#### 第六节协议

   这里的协议是正式协议，相对的还有非正式协议，这在类别一节中有介绍。正式协议是一个命名的方法列表。它要求显式地采用协议。采用协议意味着要实现协议的所有方法。否则，编译器会通过生成警告来提醒你。
1.声明协议

```
@protocol NSCopying
-(id) copyWithZone：（NSZone *)zone;
@end       
```
        

2.采用协议

```                  
@interface Car : NSObject <NSCopying , NSCoding>
{
    // instance            variables
}
@end
```
协议可以采用多个，并且可以按任意顺序列出这些协议，没有什么影响。

3.ObjC 2.0的新特性

  ObjC2.0增加了两个新的协议修饰符：@optional和@required,因此你可以像下面这样编写代码：


```     
@protocol BaseballPlayer
-(void)drawHugeSalary;
@optional
-(void)slideHome;
-(void)catchBall;
@required
-(void)swingBat;
@end
```
因此，一个采用BaseballPlayer协议的类有两个要求实现的方法：-drawHugeSalary和-swingBat,还有3个不可选择实现的方法：slideHome,catchBall和throwBall。




#### 第七节委托

Cocoa中的类经常使用一种名为委托（delegate）的技术，委托是一种对象，另一个类的对象会要求委托对象执行它的某些操作。常用的是，编写委托对象并将其提供给其他一些对象，通常是提供给Cocoa生成的对象。通过实现特定的方法，你可以控制Cocoa中的对象的行为。

 通过下面的例子，可以更清楚地理解委托的实现原理。其中A对象需要把一些方法委托给其它对象来实现，例子中就是对象B，B实现了含A对象特定方法的协议ADelegate，从而可以在B中实现A委托的方法。

```
@protocol ADelegate <NSObject>
- (void)aDelegateMethod;
……             

@end

@interface A : NSObject {
    ……

    id <ADelegate> delegate;
}
            

            

@property (readwrite, assign)             

id<ADelegate> delegate;

……

@end
            

            

@implementation A
@synthesize delegate;
- (void)aMethod
{
    [delegate aDelegateMethod];
    ......
}
@end
         
//A类
                  
@interface B : NSObject <ADelegate>             

@end

@implementation B
- (id)init {
    [[A sharedA] setDelegate:self];
}    

- (void)aDelegateMethod{   //B中实现A委托的方法
}

@end
         
//B类
```



           




注：实现委托还可以使用类别，在第八节中将做介绍




#### 第八节 类别

 类别允许你在现有的类中加入新功能，这些类可以是框架中的类，并且不需要扩充它。
 
1.声明类别

```    
@interface NSString (NumberConvenience)
-(NSNumber *) lengthAsNumber;
@end
```         

该声明表示，类别的名称是NumberConvenience，而且该类别将向NSString类中添加方法。




2.实现类别

```
@implementation NSString (NumberConvenience)
-(NSNumber *) lengthAsNumber
{
    unsigned int length = [self length];
    return ([NSNumber numberWithUnsignedInt: length]);
}
@end
```


3.局限性

   类别有两方面的局限性。第一，无法向类中添加新的实例变量。类别没有位置容纳实例变量。第二，名称冲突，即类别中的方法与现有的方法重名。当发生名称冲突时，类别具有更高的优先级。这点可以通过增加一个前缀的方法解决。
   
4.非正式协议和委托类别

实现委托除了第七节中应用协议的方式，还可以使用类别。具体做法就是把委托对象要实现的方法声明为一个NSObject的类别。如下面的代码所示:

```
@interface NSObject(NSSomeDelegateMethods)
-(void)someMethod;
…
@end
```

  通过将这些方法声明为NSObject的类别，使得只要对象实现了委托方法，任何类的对象都可以成为委托对象。创建一个NSObject的类别称为“创建一个非正式协议”。非正式协议只是一种表达方式，它表示“这里有一些你可能想实现的方法”,第六节介绍的协议可以叫做正式协议。
  
   非正式协议的作用类似于使用许多@optional的正式协议，并且前者正逐渐被后者所代替。
   
5.选择器

   选择器只是一个方法名称，它以ObjC运行时使用的特殊方式编码，以快速执行查询。你可以使用@selector()预编译指令指定选择器，其中方法名位于圆括号中。如一个类中setEngine:方法的选择器是：
   
```
@selector(setEngine:)
```   

   因为选择器可以被传递，可以作为方法的参数使用，甚至可以作为实例变量存储。这样可以生成一些非常强大和灵活的构造。




#### 第九节Posing

Posing有点像类别，但不太一样。它允许你扩充一个类，并且全面性地扮演(pose)这个超类。例如：你有一个扩充NSArry的NSArrayChild对象。如果你让NSArrayChild扮演NSArry,则在你的代码中所有的NSArray都会自动被替代为NSArrayChild.

```
@interface FractionB: Fraction
-(void) print;
@end

@implementation FractionB
-(void) print {
    printf("(%i/%i)", numerator, denominator );
}
@end
         
//Fraction.m
```
```                    
int main( int argc, const char *argv[] ) {
    Fraction *frac = [[Fraction alloc] initWithNumerator: 3 denominator: 10];
    //print it
    printf("The fraction is: " );
    [frac print];
    printf("\n" );
            

            

    //make FractionB pose as Fraction
    [FractionB poseAsClass: [Fraction class]];
    Fraction *frac2 = [[Fraction alloc] initWithNumerator: 3 denominator: 10];
            

            

    // print it
    printf("The fraction is: " );
    [frac2  print];
    printf( "\n" );
    //free memory
    [frac release];
    [frac2 release];
    
    return 0;
}
         
// Main.m
```

	输出
	The fraction is: 3/10
    The fraction is: (3/10)
         
                          
这个程序的输出中，第一个fraction会输出3/10,而第二个会输出（3/10)，这是FractionB中实现的方式。poseAsClass这个方法是NSObject的一部分，它允许子类扮演超类。




#### 第十节动态识别 （Dynamictypes)




下面是应用动态识别时所用到的方法:

```
-(BOOL)isKindOfClass: classObj
    //是否是其子孙或一员
         
-(BOOL)isMemberOfClass: classObj
    // 是否是其一员
         
-(BOOL)respondsToSelector: selector
    // 是否有这种方法
         
+(BOOL)instancesRespondToSelector: selector
    // 类的对象是否有这种方法

-(id)performSelector: selector
    // 执行对象的方法
```



通过下面的代码可以更清楚地理解动态类型的使用：

```
#import "Square.h"
#import "Rectangle.h"
#import <stdio.h>  

int main( int argc, const char *argv[] ) {
    Rectangle *rec = [[Rectangle alloc] initWithWidth: 10 height: 20];
    Square *sq = [[Square alloc] initWithSize: 15];
    //            isMemberOfClass
    //            true             

    if( [sq isMemberOfClass: [Square class]] == YES ) {
        printf( "square is a member of square class\n" );
    }
    //            false
    if ( [sq isMemberOfClass: [Rectangle class]] == YES ) {
        printf( "square is a member of rectangle class\n" );
    }
    //            false
    if( [sq isMemberOfClass: [NSObject class]] == YES ) {
        printf("square is a member of object class\n" );
    }
    //            isKindOfClass
    //            true             

    if ( [sq isKindOfClass: [Square class]] == YES ) {
        printf("square is a kind of square class\n" );
    }
    //            true
    if( [sq isKindOfClass: [Rectangle class]] == YES ) {
        printf("square is a kind of rectangle class\n" );
    }
    //            true
    if( [sq isKindOfClass: [NSObject class]] == YES ) {
        printf("square is a kind of object class\n" );
    }
    //            respondsToSelector
    //            true
    if( [sq respondsToSelector: @selector( setSize: )] == YES ) {
        printf("square responds to setSize: method\n" );
    }
    //            false
    if( [sq respondsToSelector: @selector( nonExistant )] == YES ) {
        printf("square responds to nonExistant method\n" );
    }
    //            true
    if( [Square respondsToSelector: @selector( alloc )] == YES ) {
        printf("square class responds to alloc method\n" );
    }
    //            instancesRespondToSelector
    //            false
    if( [Rectangle instancesRespondToSelector: @selector( setSize: )] == YES ) {
        printf("rectangle instance responds to setSize: method\n" );
    }
    //            true
    if( [Square instancesRespondToSelector: @selector( setSize: )] == YES ) {
        printf("square instance responds to setSize: method\n" );
    }
//            free memory
    [rec release];
    [sq  release];
             

    return 0;
}
```

	输出：
	square is a member of square class
	square is a kind of square class
	square is a kind of rectangle class
	square is a kind of object class
	square responds to setSize: method
	square class responds to alloc method
	square instance responds to setSize: method
