---
layout: post
title: "怎样减小iOS APP 或者 静态库的体积（精）"
description: ""
category: ios开发
tags: [iOS，APP，size]
--- 
#### 转载请注明粗处：http://elijahdou.github.io/

### 闲话不多收，直接上干货。

#### 把打包好的`.ipa`文件的后缀改为`.zip`并解压。右键`.app`bundle 选择显示包内容。有些情况下，大一点的文件压缩后反而比小一点的文件压缩后的体积小，而我们真正关心的时候解压后的真是体积，所以一定要解压里面的资源文件。从APP Store下载的`.ipa`文件要比自己本地打包的要大，以为APP Store 又做了加密处理。`Xcode`的`Organizer window`的`Estimate Size`功能能估计本地打包文件从APP Store下载时的大小。根据优化的28定律和常识，首先当然是多媒体资源的体积啦。

#### 图片

* 压缩图片 不重要的图片可适当采用 8bit PNG图片

	1.什么是矢量图
	矢量图是由计算机的算法产生的，可以无限放大或缩小，不会有任何损失，通常由矢量软件制作。

	2.什么是位图
	位图是由一个一个的小色块组成，放大后会看到那些小色块，同一面积内小色块越多，分辨率就越高。

	3.矢量图的优缺点
	可以无限放大或缩小，不会影响图像素质，文件体积较小，编辑灵活。缺点是表达的色彩层次不清，整体观感效果不如位图

	4.位图的优缺点
	不能放太大，减少文件分辨率后会影响图片质量，图片战胜空间较大，优点是能很细腻地表达图片的效果，图片表达效果非常好

	5.什么情况下用位图，什么情况下用矢量图
	一些对图片要求高的用位图，例如照片。其他的尽量用矢量图。例如文字、表格、卡通图片等

* 去掉无用的图片

* 用代码绘制简单的纯色图片 [用Sketch和PaintCode快速得到绘制代码](http://www.jianshu.com/p/d01110c80495)

* 如果不需要使用透明，可以用jpeg代替PNG。jpeg减少了些效率但更加小。需权衡性能，大小。
* 使用PS的“Save For Web”功能，可以有效的减小JPEG和PNG图片的尺寸。 默认情况下，在build时，PNG图像就被[pngcrush](https://developer.apple.com/library/ios/qa/qa1681/_index.html)压缩。


#### 音频

* 压缩音频，[尽可能使用AAC或者MP3格式，并且使用一个较低的码率。通常44.1khz的码率有点浪费，降低一定的码率也不会丢失多少音质](https://developer.apple.com/videos/wwdc/2011/?id=404)

#### 视频

* 视频也可以使用类似于音频的处理方法，音视频的压缩可以很大程度的压缩，但是要注意压缩的格式，是不是会增加编解码的负担，这要权衡考虑。
    
#### Assets

* 检查bundle中的无用文件，不要打包到app或者静态库中。可以点击文件，在右侧的file inspector里面的target membership中取消勾选；或者在build phase里面的Copy Bundle Resources中去掉。
* 确定 dead code（代码被定义但从未被调用）被剥离，build setting 里 DEAD_CODE_STRIPPING = YES。 去掉冗余的代码，即使一点冗余代码，编译后体积也是很可观的。

#### 编译设置

* `Optimization Level` 设置为`Fastest, Smallest [-Os]`，`Strip Debug Symbols During Copy`设置为`YES (COPY_PHASE_STRIP = YES)`，这样会减小接近一半的体积，但是在release版本，这些貌似是默认的配置，但是不妨也检查一下。 此外在debug版本最好在完成开发测试后，设置成这种模式，重新测试一遍，因为不同的编译设置可能会掩盖一些bug。


* 设置IOS_DEPLOYMENT_TARGET 为想要运行系统的最低版本

* 设置需要的arm 架构，设置 ARCHS = arm64可以消除armv6架构，潜在的减少近一半的容量。

  [iOS Devices: ARM，尺寸，像素一览表 ](https://www.innerfence.com/howto/apple-ios-devices-dates-versions-instruction-sets)
  
    1，如果想自己的app在各个机器都能够最高效率的运行，则需要将Build Active Architecture Only改为NO,Valid architectures选择对应的指令集：armv7 armv7s arm64。这个会为各个指令集编译对应的代码，因此最后的 ipa体积基本翻了3倍,Release版本必须NO。

    2，如果想让app体积保持最小，则现阶段应该选择Valid architectures为armv7,这样Build Active Architecture Only选YES或NO就无所谓了
	   
> 参考资料
>
> [Reducing the size of my App](https://developer.apple.com/library/ios/qa/qa1795/_index.html)





