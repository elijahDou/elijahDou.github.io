---
layout: post
title: "google protocol buffer总结"
description: ""
category: 
tags: [protobuf，protocol buffer]
---

#### 以前公司的项目使用protobuf，现在公司的项目不用这个。所以总结一下以前的经验，以备以后再用的时候能快速捡回来。

#### protocol buffer简称protobuf或者pb，出自Google之手。pb跨平台，解决了不同平台的数据字节对齐和串行化问题；使用Google新的编码方式，压缩后体积小，传输效率高。

#### Google官方的版本里面，只有Java 、C++和C版本。OC版本的在github上有一些项目，推荐[Google Protocol Buffers for Objective-C](https://github.com/alexeyxo/protobuf-objc)和[Protocol Buffers for Objective C](https://github.com/qzix/protobuf-objc)

#### 按照git上的说明安装pb