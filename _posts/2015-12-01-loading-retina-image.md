---
layout: post
title: "加载网络图片到UIImageView中并适配retina屏"
description: ""
category: ios
tags: [retina image,ios,UIImageView]
---

### [本文出自这里](https://mobiarch.wordpress.com/2014/01/27/loading-a-retina-image-into-uiimageview/)

#### 在`UIImageView`中加载一个网络图片有一些需要注意的地方。因为retina屏使iPhone的屏幕的基础坐标系系统有一个像素到点的转换。以 iPhone 5为例，像素是640×1136 而点是320×568。这中间有一个2倍的缩放系数，即`[UIScreen mainScreen].scale`。像素和点的关系是`pixel = point × scale`。

#### 还是以iPhone 5为例，假如有一张图片占据整个屏幕，并且图片命名有`@2x`后缀，那么这张图正好适应这个全屏的`UIImageView`并且不需要按照系数缩放。但是，如果这个图片的命名没有`@2x`后缀，`iOS`会放大这张图片两倍。而在加载网络图片的时候正是这个问题，因为图片名木有`@2x`后缀。

#### 下面是解决方法的代码，为了使用的方便，写成了一个`category`。

{% highlight objective-c %}
@interface UIImageView (URLImage)
- (void)loadRetinaImage: (NSString*) url;
@end

@implementation UIImageView (URLImage)
- (void)loadRetinaImageWithURL:(NSString*)urlStr {
    NSURL *url = [NSURL URLWithString:urlStr];
    NSData *data = [NSData dataWithContentsOfURL:url];

    if (data == nil) {
        NSLog(@"Failed to load data from URL: %@", urlStr);

        return;
    }

    UIImage *image = [UIImage imageWithData:data];

    //Resample the image to be for retina
    image = [UIImage imageWithCGImage:[image CGImage] 
                                scale:[UIScreen mainScreen].scale 
                          orientation:UIImageOrientationUp];

    self.image = image;
}
@end
{% endhighlight %}

#### 这个方案的重点是` [UIImage imageWithCGImage]`。 这个方法会设置图片为retina，等价于图片命名有`@2x`后缀。