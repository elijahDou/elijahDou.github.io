---
layout: post
title: "iOS开发中涉及的所有warning"
description: ""
category: 
tags: [iOS、warning、Clang、-W*]
---

#### 在这里[Clang Warning](http://fuckingclangwarnings.com/)找到了一篇Clang中所有warning的总结，内有解释。

#### 这样在处理一下警告就有了指导文件了。还可以使用#pragma屏蔽某种类型的警告，如：
{% highlight objective-c %}

   #pragma clang diagnostic push
#pragma clang diagnostic ignored"-Wdeprecated-declarations"
    //写在这个中间的代码,都不会被编译器提示-Wdeprecated-declarations类型的警告
dispatch_queue_tcurrentQueue =dispatch_get_current_queue();
#pragma clang diagnostic pop
{% endhighlight %}

#### 具体的使用方法，可以参考[怎么去掉Xcode工程中的某种类型的警告](http://www.cocoachina.com/ios/20141218/10678.html) 、[谈谈Objective-C的警告](http://onevcat.com/2013/05/talk-about-warning/)和[复用 Xcode 警告设置](http://weizhe.me/warning-setting/)。
