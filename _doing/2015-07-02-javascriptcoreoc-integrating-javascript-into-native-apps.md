<<<<<<< HEAD
---
layout: post
title: "JavaScriptCore与OC的互相调用 Integrating JavaScript into Native Apps"
description: ""
category: blog
tags: [iOS, JavaScript, Objective-c, integer, app]
---

本文来自苹果的wwdc2013的一个主题 [***Integrating JavaScript into Native Apps***](https://developer.apple.com/videos/wwdc/2013/)的读书笔记。

#### 直接进入正题，使用JavaScriptCore.framework的框架有：
- Xamarin：由微软支持，缺陷挺多。
- React Native：由Facebook开发维护。
- JSPatch：可以给程序打补丁。

#### 而JSPatch应用在iOS APP的原理如下：


#### 直接上内容：

#### OC中调用JS代码
##### 在头文件中导入 `#import <JavaScriptCore/JavaScriptCore.h>` 示例代码如下
{% highlight objective-c %}
int main () {
    JSContext *context = [[JSContext alloc] init];
    JSValue *result = [context evaluateScript:@"2 + 2"];
    NSLog(@"2 + 2 = %d", [result toInt32]);
    return 0;
}
{% endhighlight %}

##### 
=======
---
layout: post
title: "JavaScriptCore与OC的互相调用 Integrating JavaScript into Native Apps"
description: ""
category: blog
tags: [iOS, JavaScript, Objective-c, integer, app]
---

本文来自苹果的wwdc2013的一个主题 [***Integrating JavaScript into Native Apps***](https://developer.apple.com/videos/wwdc/2013/)的读书笔记。

#### 直接进入正题，使用JavaScriptCore.framework的框架有：
- Xamarin：由微软支持，缺陷挺多。
- React Native：由Facebook开发维护。
- JSPatch：可以给程序打补丁。

#### 而JSPatch应用在iOS APP的原理如下：


#### 直接上内容：

#### OC中调用JS代码
##### 在头文件中导入 `#import <JavaScriptCore/JavaScriptCore.h>` 示例代码如下
{% highlight objective-c %}
int main () {
    JSContext *context = [[JSContext alloc] init];
    JSValue *result = [context evaluateScript:@"2 + 2"];
    NSLog(@"2 + 2 = %d", [result toInt32]);
    return 0;
}
{% endhighlight %}

##### 
>>>>>>> try a new theme:simple grey with modify
