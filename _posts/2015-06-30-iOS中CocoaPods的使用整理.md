---
layout: post
title: "iOS中CocoaPods的使用整理"
description: "CocoaPods的使用总结"
category: blog
tags: [iOS，CocoaPods]
---

#### CocoaPods的使用总结与整理，来自至于巧叔[这一篇blog](http://www.devtang.com/blog/2014/05/25/use-cocoapod-to-manage-ios-lib-dependency/)和自己的使用经验总结。

#### CocoaPods使用来做iOS项目工程所使用的三方库的依赖管理和维护。它的项目在git上的源码地址[请点击这里](https://github.com/CocoaPods/CocoaPods)。而我们做实际项目时，不可避免的要使用三方库，这时CocoaPods绝对是个利器，CocoaPods会自动对三方库做一些submodule的处理、配置（如禁用ARC（-fno-objc-arc），关闭三方库的警告等） 以及更新。

#### CocoaPods的安装与使用

#### 安装方法

##### CocoaPods的安装需要Ruby，而Mac都默认自带有Ruby环境，直接使用安装CocoaPods即可。默认的下载源是 `https://rubygems.org/`，天朝上国的墙内是用不了的。所以我们要先重置ruby源：

{% highlight ruby %}
// 建议先升级一下
sudo gem update --system

// 替换源
gem source -l    // 查看源，如果已经是taobao的源，跳过以下2步
gem sources --remove https://rubygems.org/
gem sources -a http://ruby.taobao.org/   // 替换源

// 开始安装
sudo gem install cocoapods
pod setup
{% endhighlight %}

#### 使用    

##### 如果是自己新建工程，那么使用的第一步是新建一个`Podfile`文件。 样例如下：
`platform :ios, '7.0'
 pod 'SDiPhoneVersion', '~>1.1.2'
 pod 'AFNetworking', '~>2.0'
 pod 'JSONKit-NoWarning', '~> 1.2'`

Podfile文件的语法见[这里](https://guides.cocoapods.org/using/the-podfile.html)，简单易学。 

##### Podfile最好放在工程目录下面，当然也可以是其他位置（不过使用不方便）。以放在工程目录下为例：
`cd "your project home"
 pod install`

 以后如果Podfile有更新，在工程目录下执行`pod update`。

 ***注意：在install或者update过程中，要退出workspace或者Xcode，若不退出，这个过程中不要做其他的操作 等待结束，不然可能会出现：工程打不开的情况 [Can't open project from workspace](https://github.com/CocoaPods/CocoaPods/issues/763) 这是血泪的教训啊***


##### 添加新的三方库时，使用pod search，能够搜索到的就是能够使用Cocoapods维护的，上样例：

`$ pod search AFNetworking


-> AFNetworking (2.5.4)
   A delightful iOS and OS X networking framework.        
   ***pod 'AFNetworking', '~> 2.5.4'（）***     
   - Homepage: https://github.com/AFNetworking/AFNetworking     
   - Source:   https://github.com/AFNetworking/AFNetworking.git       
   - Versions: 2.5.4, 2.5.3, 2.5.2, 2.5.1, 2.5.0, 2.4.1, 2.4.0, 2.3.1, 2.3.0,
   2.2.4, 2.2.3, 2.2.2, 2.2.1, 2.2.0, 2.1.0, 2.0.3, 2.0.2, 2.0.1, 2.0.0,
   2.0.0-RC3, 2.0.0-RC2, 2.0.0-RC1, 1.3.4, 1.3.3, 1.3.2, 1.3.1, 1.3.0, 1.2.1,
   1.2.0, 1.1.0, 1.0.1, 1.0, 1.0RC3, 1.0RC2, 1.0RC1, 0.10.1, 0.10.0, 0.9.2,
   0.9.1, 0.9.0, 0.7.0, 0.5.1 [master repo]       
   - Subspecs:     
     - AFNetworking/Serialization (2.5.4)     
     - AFNetworking/Security (2.5.4)     
     - AFNetworking/Reachability (2.5.4)     
     - AFNetworking/NSURLConnection (2.5.4)    
     - AFNetworking/NSURLSession (2.5.4)     
     - AFNetworking/UIKit (2.5.4)    


-> AFNetworking+AutoRetry (0.0.5)     
   Auto Retries for AFNetworking requests     
   ***pod 'AFNetworking+AutoRetry', '~> 0.0.5'***     
   - Homepage: https://github.com/shaioz/AFNetworking-AutoRetry    
   - Source:   https://github.com/shaioz/AFNetworking-AutoRetry.git    
   - Versions: 0.0.5, 0.0.4, 0.0.3, 0.0.2, 0.0.1 [master repo]   

// 此处省略若干行`

最简单的写更新Podfile的方法，就是复制其中的***加粗斜体***部分到Podfile中即可。

***

#### 关于.gitignore

##### 当你执行`pod install`之后，除了Podfile外，CocoaPods还会生成一个名为`Podfile.lock`的文件，你不应该把这个文件加入到`.gitignore`中。因为`Podfile.lock`会锁定当前各依赖库的版本，之后如果多次执行`pod install` 不会更改版本，要`pod update`才会改`Podfile.lock`了。这样多人协作的时候，可以防止第三方库升级时造成大家各自的第三方库版本不一致。

##### CocoaPods的这篇[官方文档](http://guides.cocoapods.org/using/using-cocoapods.html#should-i-ignore-the-pods-directory-in-source-control)也在`What is a Podfile.lock`一节中介绍了`Podfile.lock`的作用，并且指出：
> This file should always be kept under version control.

***

#### 使用私有的pods

##### 我们可以直接指定某一个依赖的podspec，这样就可以使用公司内部的私有库。该方案有利于使企业内部的公共项目支持CocoaPods。如下是一个示例：

`pod 'MyCommon', :podspec => 'https://yuantiku.com/common/myCommon.podspec'`

***

#### 不更新podspec

##### CocoaPods在执行pod install和pod update时，会默认先更新一次podspec索引。使用--no-repo-update参数可以禁止其做索引更新操作。如下所示：

`pod install --no-repo-update
pod update --no-repo-update`

***

#### 生成第三方库的帮助文档

##### 如果你想让CococaPods帮你生成第三方库的帮助文档，并集成到Xcode中，那么用brew安装appledoc即可：

`brew install appledoc`

appledoc的介绍和使用，google之。

***

#### 给自己的开源项目创建podspec

##### 我们可以给自己的开源项目创建podspec文件，命令如下：
`pod spec create custom_pod_spec_name`

##### 之后会得到一个`custom_pod_spec_name.podspec`文件。下面的工作就是修改`.podspec`文件中相关内容。
{% highlight ruby %}

Pod::Spec.new do |s|

  s.name         = "test"
  s.version      = "0.0.1"
  s.summary      = "A short description of test."

  s.description  = <<-DESC
                   A longer description of test in Markdown format.

  s.homepage     = "http://EXAMPLE/test"
  # s.screenshots  = "www.example.com/screenshots_1.gif", "www.example.com/screenshots_2.gif"


  s.license      = "MIT"


  s.author       = { "***" => "***@**" }
  
  s.source       = { :git => "http://EXAMPLE/test.git", :tag => "0.0.1" }


  s.source_files  = "Classes", "Classes/**/*.{h,m}"
  s.exclude_files = "Classes/Exclude"

end
{% endhighlight %}

##### 好多内容是自解释的注释，按照注释修改完成后，删掉注释，大概就是上面的代码的样子。

#### 以下是文件修改的方法 

***

#### s.source s.source_files

##### 这里的 source 我们看出是一个git 的地址，这里我们调试的时候，可以先暂时设置成本地git，调试完毕之后就可以发布 增加tag。想要最新的代码只需要这样设置就好

`{ :git => "https://github.com/studentdeng/ShareCenterExample.git"}`

##### s.source_files表示源文件的路径，注意这个路径是 ***相对podspec文件而言*** 的。

##### 我们的git项目中，并不是所有的代码都需要被引用到我们的代码中，通常project还会包括一些example，test cases等，这里的 source_files 就是用来指定一些文件夹或文件。

***

#### s.frameworks s.library

##### 这里配置的就是我们的framework 和 library，这里注意一下library的名字规则就好,需要用到的frameworks，***不需要加.frameworks后缀***。

***

#### vendored_libraries

##### 用来指定外部的静态库。

***

#### s.prefix_header_contents

##### 用来指定预编译的配置。

***

#### 部署我们的配置到cocoapods中

##### cocoapods的代码配置文件是在这里[Specs](https://github.com/CocoaPods/Specs)

##### 这里最好是去fork一个自己的project，然后保存一个自己或是团队的配置，这样不会在更新cocoapods的时候，丢掉自己的配置。当然，如果觉得自己搞的还不错，也可以去pull requests。

##### 在之前提到的目录~/.cocoapods/repo/master/ 下面，我们可以看到已经有超级多的项目了，我们可以也可以通过 `$ pod search XXX` 来查找项目，或是直接在这个文件夹下面找。

#### LICENSE文件

CocoaPods强制要求所有的Pods依赖库都必须有license文件，否则验证不会通过。license的类型有很多种，详情可以参考[网站](https://tldrlegal.com)。

#### 注册Truck
如果已经注册过了，可以跳过这一步。 CocoPods从0.33加入了Trunk，这样方便了发布自己的Pod。注册很简单，命令如下：
`pod trunk register your-email "your nickname"`

然后就可以使用如下命令来发布和升级自己的Pod：
`pod trunk push NAME.podspec`

##### 在使用Trunk发布前，要讲自己的代码提交到github上，并打上tag。 `Podfile`内书写这种自定义的Pod的格式如下：

`pod 'name', :git => 'custom source url', :tag => 'tag version'`

##### 发布成功后就可以用 `pod search`命令搜索到自定义的 pod 就说明成功了。

##### 如果希望更多的了解cocoapods，还是需要去[Github](https://github.com/CocoaPods/CocoaPods)上面。 


> 参考资料：
> [cocopod使用篇](http://blog.csdn.net/wzzvictory/article/details/20067595)
> 
> [CocoaPods](http://nshipster.cn/cocoapods/)
> 
> [发布自己的pods到CocoaPods trunk](http://www.tuicool.com/articles/6FF7fi)
> 
> [CocoaPods详解之----制作篇](http://blog.csdn.net/wzzvictory/article/details/20067595)
> [CocoaPods 手把手五分钟教你制作自己的podspec文件](http://iiiyu.com/2013/12/19/learning-ios-notes-thirty-one/)
