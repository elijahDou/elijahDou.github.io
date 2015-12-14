---
layout: post
title: "转 iOS高效开源库集合"
description: ""
category: ios
tags: [open source library,tool]
---

### 偶然看到的收藏之

### 网络篇：

#### 为啥要用第三方开源库？因为系统API废话太多，用起来太啰嗦。

#### HTTP请求看这些：
- ***AFNetWorking： 个人认为是首选，轻量、易用，开发者也在很积极的维护，拥护者也多，新的项目，就直接用它吧！ https://github.com/AFNetworking/AFNetworking*** 
- MKNetworkKit： 用的人也不少，也是非常轻量级，使用起来比较简单。https://github.com/MugunthKumar/MKNetworkKit 
- ASIHttpRequest:  一句话，过时了。开发者已经很久没有维护了，而且用起来有点重。

#### Socket通信看这个：
- ***CocoaAsyncSocket： 如果要使用socket进行通讯，那么肯定就用他了，支持TCP和UDP，支持GCD，支持OC接口，封装的非常不错。 https://github.com/robbiehanson/CocoaAsyncSocket***

#### 网络相关工具看这个：
- ***Reachability： 用来判断和监听网络状态和变化，非常好的辅助工具库，必用。 https://github.com/tonymillion/Reachability***


#### JSON解析篇：
- 如果是支持iOS 5以上的系统，那么就直接使用系统提供的NSJSONSerialization吧，性能好，官方提供，可靠！
如果要用第三方的，那就选择JSONKit吧，性能好，源码就2文件，看着都舒服。
SBJson就千万别再用了，性能差，维护的也不好。

### UI框架篇：

#### 提升到框架层面，一般都比较重了，选择的时候需要慎重了，因为这个对于你的APP未必是必须的。
- ***AsyncDisplayKit：强烈推荐一试。 Facebook刚刚开源发布的UI框架，被用于Facebook自家的应用Paper中。这个框架能给你带来的核心收益，是能够提高UI的流畅性，降低UI的响应时间。这是通过对UIKIt的进一步的异步封装来实现的，AsyncDisplayKit的UI元素都是线程安全的，可以在并行异步的进行操作，这样就可以让图像解码等耗时操作移出UI线程，为UI主线程减负。值得一提的是，AsyncDisplayKit的组件封装，与UIKit很类似，可以直接进行替换，接入门槛相对比较低。https://github.com/facebook/AsyncDisplayKit***
- ***ReactiveCocoa：如果你对响应式编程模式有兴趣，可以尝试一下这个框架，毕竟一般APP，大部分时间是在等待事件发生，等待用户操作或者网络请求返回，所以这种事件驱动模式，还是很适合的。https://github.com/ReactiveCocoa/ReactiveCocoa***
- bee Framework：国产mvc框架，将许多核心逻辑注入到了NSObject基类里，同样也是采用了响应式的设计。 https://github.com/gavinkwoe/BeeFramework
- nimBus：Three20的作者之一开发的一套轻型框架，很多思路和Three20相似，但是轻量很多，支持命名空间，内部耦合性没那么高，很多人把他作为Three20的替代品。
- Three20： 曾经的辉煌，Facebook自己也早已不维护，不要再使用了。

### UI控件篇：
- ***SDWebImage ： 网络图片加载库，可以自动的管理图片的下载、缓存、处理、展示，就像使用本地图片一样简单的使用网络图片，非常好用，神器！ https://github.com/rs/SDWebImage***
- ***MBProgressHUD： 优雅的进度显示效果，提供了很多动画效果，常备利器。 https://github.com/jdg/MBProgressHUD*** 
- SVProgressHUD： 和MBProgressHUD的功能效果类似，不过使用他不需要使用协议，也不需要声明实例，直接类方法调用即可。 https://github.com/samvermette/SVProgressHUD
- MWPhotoBrowser： 一个完整的图片浏览器组件，效果和iOS自带的相片app 差不多，能滑动切换图片，点击隐藏导航条，缩放，分享，复制等。既可以管理本地图片，也可以查看网络图片，并且自带缓存策略。  https://github.com/mwaterfall/MWPhotoBrowser
- HPGrowingTextview：增强的文本输入框，实现了增加多行文本和减少文本时输入框的伸缩和滚动效果。
- AKSegmentedControl：可自定义外观的 Segmented Control， 包括背景颜色、segment上显示图片或者文字、按键模式等。 https://github.com/alikaragoz/AKSegmentedControl

- EGOTableViewPullRefresh： 解决下拉刷新和上提加载更多的交互效果。 https://github.com/enormego/EGOTableViewPullRefresh


### 存储篇：
- ***喜欢使用 sqlite的，可以试试fmdb，封装的不错，简洁易用。 https://github.com/ccgus/fmdb***
- 如果要用CoreData， 那么用一下MagicalRecord，它降低了使用coredata的门槛。https://github.com/magicalpanda/MagicalRecord
- 如果要使用keyChain存储敏感数据，那么可以用SFHFKeychainUtils，使用非常简单。 https://github.com/kamiro/SFHFKeychainUtils


### 动画篇：
- ***pop： Facebook发布的大名鼎鼎的动画库（动画引擎），支持多种常见动画效果，包括弹性和衰减，也可以自定义效果，最关键的是，其接口语法上和Core Animation非常相似，就像是重新实现了系统的Core Animation，所以上手容易。https://github.com/facebook/pop***
- Canvas ：可以认为就是个动画集合，提供了对Core Animation进一步的封装。 https://github.com/CanvasPod/Canvas


### 工具篇：
- 日志框架： 用用 CocoaLumberjack吧，支持log分级，支持日志记录方式（控制台、文件、服务器），而且性能不错，可以简单的替换NSLog。 https://github.com/CocoaLumberjack/CocoaLumberjack
- 数据加密和摘要： CocoaSecurity 提供了常见的AES加解密，sha1、md5系列的摘要算法。 https://github.com/kelp404/CocoaSecurity
- zip压缩和解压： ZipArchive，非常好用，打包上传和下载的时候经常需要用到。
- 文字效果： DTCoreText 是一个功能强大的文字效果库，可以在UITextView上实现包括文字大小、颜色、字体、下划线、链接、间距等效果。 https://github.com/Cocoanetics/DTCoreText
- DTFoundation： 这是个强大的工具集合，百宝箱啊，里面有异步文件删除、目录监控、base64编码、日志、版本管理，以及对NSArray、NSDictionary、NSString、NSURL等系统类的一些方便使用扩展，还有对gif动画的支持和HTML解析，以及UIWebView、UIImage、UIView等UIKit的扩展和一些UI增强组件。 https://github.com/Cocoanetics/DTFoundation
- MAZeroingWeakRef：非常棒的智能指针工具，可以自动销毁对象分配的内存，防止内存泄漏，方便内存管理。https://github.com/mikeash/MAZeroingWeakRef
- TMCache： 一个不错的基于key/value的数据缓存库，支持内存和持久缓存，基于GCD支持多线程操作，挺好用。 https://github.com/tumblr/TMCache

> 扩展

> [94个iOS开发资源推荐，帮你加速应用开发](http://www.cocoachina.com/ios/20151210/14635.html)
