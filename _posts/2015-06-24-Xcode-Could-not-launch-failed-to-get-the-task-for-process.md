---
layout: post
title:  "Xcode Could not launch-failed to get the task for progress"
date:   2015-06-24 14:09:32
categories: blog
analytics : true
tags: [Xcode, launch failed]
---

#### 转载请注明出处： http://elijahdou.github.io/

### 　　在测试项目代码时，选择debug版本编译通过，测试正常；在release版本下，可以编译成功，但是APP一在真机上运行就会报如题所示的错误，程序闪退。


### 　　原来是Build Setting中Code Signing的Code Signing Identity配置问题。使用的签名是distribution certificate，即发布者证书，它不允许在真机上直接运行。


![示意图](http://ww1.sinaimg.cn/large/0069kYsZjw1etf8cztv0qj30ic05hgm9.jpg)


#### ***换用开发者证书就OK了。***

