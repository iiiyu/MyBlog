---
layout: post
title: "sizeof 深入理解"
date: 2012-02-09 16:01
comments: true
tags: just-talk
---


sizeof 深入理解
==============


sizeof你们觉得是什么呢。函数？其实是操作符。
wiki解释

>In the programming languages C and C++, the unary operator 'sizeof' is used to calculate the sizes of datatypes, in number of bytes. sizeof can be applied to all datatypes, be they primitive types such as the integer and floating-point types defined in the language, pointers to memory addresses, or the compound datatypes (unions, structs, or C++ classes) defined by the programmer. sizeof is an operator that returns the size in bytes of the type of the variable or parenthesized type-specifier that it precedes as a size_t type value.
 

sizeof is an operator：sizeof是一个操作符。擦 这货居然是操作符。
最后一句我们可以得到的信息还有sizeof操作符的结果类型是size_t，它在头文件中typedef为unsigned　int类型。该类型保证能容纳实现所建立的最大对象的字节大小。

<!--more-->
 
我们先记住什么时候可能用到sizeof,因为我们毕竟是学了用的.sizeof的应用场景一般是神马呢：

- sizeof操作符的一个主要用途是与存储分配和I/O系统那样的例程进行通信。例如：

``` c
void　*malloc(size_t　size)
size_t　fread(void　*　ptr,size_t　size,size_t　nmemb,FILE　*　stream)
```

- 用它可以看看一类型的对象在内存中所占的单元字节。

``` c
void　*　memset(void　*　s,int　c,sizeof(s))
```

- 在动态分配一对象时,可以让系统知道要分配多少内存。

``` c
int * pointer = malloc(sizeof(int) * 10)
```

- 便于一些类型的扩充,在windows中就有很多结构内型就有一个专用的字段是用来放该类型的字节大小。

- 由于操作数的字节数在实现时可能出现变化，建议在涉及到操作数字节大小时用sizeof来代替常量计算。

- 如果操作数是函数中的数组形参或函数类型的形参，sizeof给出其指针的大小。
 
 
知道怎么用以后我们来深入的蛋疼的讨论一下吧：
那我们考虑一下它既然是操作符那是一元的呢还是一元的呢还是一元的呢。。。。。。。。。。。。。
 
网上有人说sizeof是一元操作符，网上还有人说sizeof更像一个特殊的宏，它是在编译阶段求值的。
举个例子：

``` c
#include <stdio.h>
int main()
{
    int a = 0;
    printf("sizeof(a=3):%d\n",sizeof(a = 3));
    printf("a:%d\n",a);
    return 0;
}
```

输出为什么是4，0而不是期望中的4，3？？？就在于sizeof在编译阶段处理的特性。由于sizeof不能被编译成机器码，所以sizeof作用范围内，也就是()里面的内容也不能被编译，而是被替换成类型。=操作符返回左操作数的类型，所以a=3相当于int，而代码也被替换为：

``` c 
printf("sizeof(a=3):%d\n",4);
```

``` c
printf("a:%d\n",a);
```
 
所以，sizeof是不可能支持链式表达式的，这也是和一元操作符不一样的地方。
结论：好，记住：sizeof 在 计算变量所占空间大小时， 括号可以省略， 而计算类型(模子)大小时不能省略。 一般情况下， 咱也别偷这个懒， 乖乖的写上括号， 继续装作一个 “函数” 做一个 ， “披着函数皮的操作符” 。 做我的操作符，让人家认为是函数去吧。 不要把sizeof当成函数，也不要看作一元操作符，把他当成一个特殊的编译预处理。
 
其实说到这里也差不多了，最后看一下几个比较飘逸的例子：

``` c
sizeof（int）*p 表示什么意思？
int *p = NULL;
sizeof(p)的值是多少？
sizeof(*p)呢？
int a[100];
sizeof (a) 的值是多少？
sizeof(a[100])呢？//请尤其注意本例。
sizeof(&a)呢？
sizeof(&a[0])呢？
int b[100];
void fun(int b[100])
{
sizeof(b);// sizeof (b) 的值是多少？
}
```

时间不早了  1点多了 明天还要上班  睡了  结果自己看吧。

``` c
#inlcude <stdio.h> 
void fun(int b[100])
{
    printf("fun()sizeof(b):%d\n",sizeof(b));
}
int main()
{
    int *p = NULL;
    int a[100];
    int b[100]; 
    printf("sizeof(p):%d\n",sizeof(p));
    printf("sizeof(*p):%d\n",sizeof(*p));
    printf("sizeof(a):%d\n",sizeof(a));
    printf("sizeof(a[100]):%d\n",sizeof(a[100]));
    printf("sizeof(&a):%d\n",sizeof(&a));
    printf("sizeof(&a[0]):%d\n",sizeof(&a[0]));
    fun(a);
    return 0;
}
```



