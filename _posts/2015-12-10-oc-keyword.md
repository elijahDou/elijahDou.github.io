---
layout: post
title: "OC的关键字"
description: ""
category: iOS, OC
tags: [OC, keyword]
---

### 开发中遇到了一些不常见的关键字，原来只是一知半解，所以做了一些功课，总结了以下。
*****

### @encode()

#### 在使用`NSNumber`时，很可能会遇到这个关键字。在用`masonry`时，也发现了这个关键字的使用场景。我们可以把`int`、`BOOL`等封装成`NSNumber`对象，但是翻过来，我们怎么知道`NSNumber`内封装的到底是什么原型的数据呢。就会用到`@encode()`。

#### Objective-C 的数据类型，甚至自定义类型、函数或方法的元类型，都可以使用`ASCII`编码。`@encode(aType)` 可以返回该类型的`C`字符串`（char *）`的表示。使用举例如下：
{% highlight objective-c %}
id value = [dic valueForKey:key];  
if([value isKindOfClass:[NSNumber class]]){  
    const char * pObjCType = [((NSNumber*)value) objCType];  
    if (strcmp(pObjCType, @encode(int))  == 0) {  
        NSLog(@"字典中key=%@的值是int类型,值为%d",key,[value intValue]);  
    } else if (strcmp(pObjCType, @encode(float)) == 0) {  
        NSLog(@"字典中key=%@的值是float类型,值为%f",key,[value floatValue]);  
    } else if (strcmp(pObjCType, @encode(double))  == 0) {  
        NSLog(@"字典中key=%@的值是double类型,值为%f",key,[value doubleValue]);  
    } else if if (strcmp(pObjCType, @encode(BOOL)) == 0) {  
        NSLog(@"字典中key=%@的值是bool类型,值为%i",key,[value boolValue]);  
    }
}
{% endhighlight %}

*********

### @class

#### 这个关键字我用的很多，总而言之一句话，`.h`文件内，能用`@class`就别导入头文件。

*******

### @dynamic

#### 这个关键字是相对于`@synthesize`的，用于动态合成`property`。编译器不会合成`accessor`，`runtime`时动态解析。

********

### @defs

#### 这个关键字把OC对象转化成对应的`struct`结构体。这个`struct`与原`OC`类具有相同的内存布局。`OC`类底层也是`C struct`和一些方法的封装。涉及非常底层的操作或优化的时候才会用到。示例代码如下：

`struct { @defs( NSObject) }` 

******

### @autoreleasepool

#### 用于`ARC`下代替`NSAutoreleasePool`的关键字。一般用于降低app的内存峰值，包裹敏感代码。

*******

### @compatibility_alis

#### 用于给一个类设置一个别名。这样就不用重构以前的类文件就可以用新的名字来替代原有名字。示例代码如下

`@compatibility_alias AliasClassName ExistingClassName`

*****

### 附：OC保留的关键字列表
{% highlight objective-c %}
@class
@defs
@protocol @required @optional @end
@interface @public @package @protected @private @property @end
@implementation @synthesize @dynamic @end
@throw @try @catch @finally
@synchronized @autoreleasepool
@selector @encode
@compatibility_alias
@”string”
{% endhighlight %}

*******

> 扩展阅读
 
> [关于Objective-C warning的笔记](https://gist.github.com/croath/5027669)
 
> [那些被遗漏的Objective-C保留字](http://blog.devtang.com/blog/2013/04/29/the-missing-objc-keywords/)

> [The Complete List of Objective-C 2.0 @ Compiler Directives](http://www.learn-cocos2d.com/2011/10/complete-list-objectivec-20-compiler-directives)
