---
layout: post
title: "Using Swift with Cocoa and Objective C 总结"
description: ""
category:swift
tags: [ios, swift]
---
### 本文是阅读Apple官方文档***Using Swift with Cocoa and Objective-C (Swift 2.2)***的总结，方便以后翻阅



#### Closures

##### OC中的`block`等价与swift中的`closures`。混编时 `block`会自动转为swift中的`closures` 并标记为`@convention(block)`属性。swift中`closures`长这个样子：

{% highlight swift %}
let completionBlock: (NSData, NSError) -> Void = { data, error in
    // ...
}
{% endhighlight %}

##### `closures`跟`block`兼容，可以将swift中的`closures`传给OC中的`block`，如果一个`closures`和一个swift函数有相同的参数类型，神之可以把函数名传给OC中的`block`。  `closures`和`block`最大的区别是 后者捕获到的变量是`copy`, 而前者是`mutable`，简言之 `closures`的默认行为相当于 `block`中的捕获到用`__block`修饰的变量




#### 避免循环引用

##### OC中循环引用出现的条件:`self strong/copy block`,并且这个存在于*堆*上的`block`内部出现了`self`，使用的标准姿势如下（这可是apple的demo奥）：

{% highlight objective-c %}
__weak typeof(self) weakSelf = self;
self.block = ^{
   __strong typeof(self) strongSelf = weakSelf;
   [strongSelf doSomething];
};
{% endhighlight %}

##### swift中处理`closures`的循环引用 要讲`self`指定为`unowned`,姿势如下：

{% highlight swift %}
self.closure = { [unowned self] in
    self.doSomething()
}
{% endhighlight %}




#### 对象比较

##### swift中有两种比较操作符 `==`和`===`： 前者比较对象内容是否相同，默认调用`isEqual`方法； 后者比较对象是否指向同一个引用，检查指针是否指向同一地址。  在自定义子类中 要自己实现`isEqual`方法，这涉及到对象等同性的判断，同OC

##### note：swift自动提供equality（==）和 identity(===)操作符的逻辑实现：!= 和 !==， 这些不要重写

#### hashing

##### 这部分同OC，`NSObject`的子类在等同性判断时 也要实现`hash`属性



#### swift的类型兼容性

##### OC转swift的类中，属性 方法 下标 初始化方法等是兼容可用的，但是一些swift特有的feature是不支持的，如下：
- 泛型 generics
- 元组 tuples
- swift中的 非`Int`原始数据类型 的枚举
- swift中定义的结构体
- swift中的顶级函数
- swift中的全局变量
- swift中的类型别名 Typealiases
- swift风格的可变参数 variadics
- 嵌套类型 Nested types
- [柯里化函数](http://www.cocoachina.com/ios/20141110/10166.html)

##### swift的API转OC与OC转swift相同：
- swift中的 optional 类型标记为`__nullable`
- swift中的 non-optional 类型标记为 `__nonnull`
- swift的常量存储属性和计算属性转到OC中为read-only属性
- swift的变量存储属性转为OC中的read-write属性
- swift中的type method转为OC的类方法
- swift的初始化方法和实例方法转为OC的实例方法
- swift的抛出`errors`的方法转为OC的方法时 会在方法名后后缀一个`NSError **`类型的参数。当改swift方法没有返回值时，相应的OC方法会有一个`BOOL`类型的返回值
- ***在OC代码中，不能子类化一个swift类***

{% highlight swift %}
class Jukebox: NSObject {
    var library: Set<String>
    
    var nowPlaying: String?
    
    var isCurrentlyPlaying: Bool {
        return nowPlaying != nil
    }
    
    init(songs: String...) {
        self.library = Set<String>(songs)
    }
    
    func playSong(named name: String) throws {
        // play song or throw an error if unavailable
    }
}

Here’s how it’s imported by Objective-C:

@interface Jukebox : NSObject
@property (nonatomic, copy) NSSet<NSString *> * __nonnull library;
@property (nonatomic, copy) NSString * __nullable nowPlaying;
@property (nonatomic, readonly) BOOL isCurrentlyPlaying;
- (nonnull instancetype)initWithSongs:(NSArray<NSString *> * __nonnull)songs OBJC_DESIGNATED_INITIALIZER;
- (BOOL)playSong:(NSString * __nonnull)name error:(NSError * __nullable * __null_unspecified)error;
@end
{% endhighlight %}



#### 配置swift在OC中的接口

##### 

















