---
layout: post
title: "一个关于frame和Autolayout（Masonry）的问题"
description: ""
category: UI
tags: [frame，autolayout，masonry]
---

#### 在做开发的时候，UI适配方面老代码都是使用`frame`做UI布局，新的代码却是用的`Autolayout`，我们主要是用的三方库`masonry`写约束完成布局。在这个过程中。我一直有个错误的认识，`Autolayout`和`frame`相互独立互不影响。网上看到很多blog也这么认为。让我在这个坑里一直徘徊。这么认为多是由于设置完约束之后，打印`frame`的值为`CGRectZero`或者一个错误值造成的。但是在我调试一段代码时，情况却超出了我原来（错误）的认识。为什么打印的`frame`完全正确？

#### 我请教了一些同事和牛人并经过验证之后，刷新了我对`Autolayout`的认识。`Autolayout`的本质是通过一套约束规则去确定一个`view`在`superview`中的位置和尺寸。既是位置和尺寸那不就是`frame`嘛。所以我通过实验得出了一以几个结论：
- `Autolayout`是一套关于`view`位置和尺寸的约束规则
- `Autolayout`改变可以改变`view`的`frame`，但是`frame`的改变不能改变`Autolayout`的约束规则，约束只能通过约束更新的方式改变。想要在`Autolayout`改变后理解看到`frame`的变化，请调用`- (void)layoutIfNeeded`来立即刷新UI的布局
- `Autolayout`和`frame`能否混用，答案是一定程度上可以，牢记上面一条两者之间的相互影响关系。在`frame`布局之前一定要先调用`- (void)layoutIfNeeded`方法，这样就能得到`superView`和相关`view`准确的`frame`。但不是修改`frame`不会修改约束使得混用的情况的变得复杂很容易出错，所以还是大众化的建议，最好不要混用。
