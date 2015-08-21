---
layout: post
title: "iOS和Android语音格式"
description: ""
category: 
tags: [iOS, android, voice, format]
---

#### 转载请注明粗处：http://elijahdou.github.io/

#### 见到有人讨论这个，就自己记录一下，方便以后使用。

****

### iOS和Android支持的 ***即可以回放也可以录制的*** 音频类型

#### **iOS**：[AAC(MPEG-4 Advanced Audio Coding), Apple Lossless(ALAC), iLBC(internet Low Bitrate Codec, another format for speech),  IMA4 (IMA/ADPCM), Linear PCM (uncompressed, linear pulse-code modulation), µ-law and a-law](http://developer.apple.com/library/ios/#documentation/AudioVideo/Conceptual/MultimediaPG/UsingAudio/UsingAudio.html) 

#### **Android**：[mp3, wma, ogg, AAC(enhanced AAC+, enhanced low delay AAC, AAC+, AAC-LC), AMR-NB, AMR-WB, WAV, MIDI](http://developer.android.com/guide/appendix/media-formats.html) 

#### 其中两个平台都支持的是AAC并且文件格式为MPEG-4。选择该格式是成本最低的方式，不需要再使用三方库转换。相对来说AAC对数据进行了压缩，文件较小，而且iOS平台上会有硬件加速，效率很高。

#### 如果使用其他格式，那么需要使用三方库转换：[libOpenCore](https://github.com/hehe19900512/RecordingAudio-libopencore-amr) 和 [lame](https://github.com/kewlbear/lame-ios-build)


> 扩展延伸阅读
>
> [ios与android设备即时语音互通的录音格式](http://blog.csdn.net/kingkong1024/article/details/14004885)
>
>