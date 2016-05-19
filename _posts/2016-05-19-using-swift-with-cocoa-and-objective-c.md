---
layout: post
title: "Using Swift with Cocoa and Objective C 总结"
description: ""
category:swift
tags: [ios, swift]
---
### 本文是阅读Apple官方文档***Using Swift with Cocoa and Objective-C (Swift 2.2)***的总结，方便以后翻阅


#### 类方法和便捷初始化方法
##### OC的类方法转到swift中为convenience initializers



#### 可失败的初始化
##### OC中失败的初始化多返回 nil，可失败的初始化方法 可以用`nullable`标记。OC中可失败和不可失败的初始化方法转到swift表示为`init(...)` `init?(...)` 否则 为`init!(...)`



#### 存取属性
##### 


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

{% highlight Objective-C %}
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

// Here’s how it’s imported by Objective-C:

@interface Jukebox : NSObject
@property (nonatomic, copy) NSSet<NSString *> * __nonnull library;
@property (nonatomic, copy) NSString * __nullable nowPlaying;
@property (nonatomic, readonly) BOOL isCurrentlyPlaying;
- (nonnull instancetype)initWithSongs:(NSArray<NSString *> * __nonnull)songs OBJC_DESIGNATED_INITIALIZER;
- (BOOL)playSong:(NSString * __nonnull)name error:(NSError * __nullable * __null_unspecified)error;
@end
{% endhighlight %}



#### 配置swift在OC中的接口

##### 在需要控制swift API向OC暴露时，可以使用`@objc(name)`属性去改变类 属性 方法 枚举类型 枚举成员暴露到OC中的名字
##### 向swift函数提供一个OC的name时，如果用的selector语法，要在selector的参数后面加`:`
##### 在swift中使用`@objc(name)`属性时，转到OC时 没有命名空间。  当把一个可归档的OC类迁移到swift时，可以使用`@objc(name)`属性指定与OC类相同的类名（因为已归档对象会存储归档的类名），这样就可以把原先已经归档的数据恢复到swift类中。
##### swift中`@nonobjc`属性 会使swift的声明在OC中无法使用。这个可以用来解决 桥接方法 的环，或者 重新加载OC导入的类中的方法。如果OC的方法被swift的方法重写 并且该方法不能暴露给OC，比如指定一个参数为变量，那么这个方法必须标记为`@nonobjc`



#### Requiring Dynamic Dispatch

##### 当在OC的runtime时 引入swift的API时，不能保证属性 方法 下标和初始化方法的动态派发。swift的编译器可能会绕过runtime 反虚拟化或者inline成员存取 来优化代码。 使用`dynamic`可以要求成员的存取方法在OC的runtime动态派发。在使用`KVO`或者OC runtime 的`method_exchangeImplementations`函数时，需要使用该修饰符。这样swift编译器inline的方法实现或者反虚拟化的存取方法不会被使用。
##### note：用`dynamic`修饰的 不能用 `@nonobjc`属性



#### OC selectors
##### OC selector在swift中用`Selector`结构体表示。构造一个selector用`#selector`表达式。如 `let mySelector = #selector(MyViewController.tappedButton)`（注意OC方法是其中的子表达式）。OC方法的引用可以括号化，可以用`as`运算符消除重载函数之间的歧义，如`let anotherSelector = #selector(((UIView.insertSubview(_:at:)) as (UIView) -> (UIView, Int) -> Void))`

##### 用performSelector发送消息
##### performSelector的方法 同步执行 并隐式返回一个`unwrapped optional unmanaged instance (Unmanaged<AnyObject>!)`，因为该方法返回值的类型和拥有关系在编译器不能确定。[ Unmanaged Objects](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/BuildingCocoaApps/WorkingWithCocoaDataTypes.html#//apple_ref/doc/uid/TP40014216-CH6-ID85)



















