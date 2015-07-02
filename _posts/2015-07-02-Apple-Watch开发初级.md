---
layout: post
title:  "Apple Watch 开发初级"
date:   2015-07-202 14:27:55
---

 *** 本文转自 [猛戳这里](http://chiefky.github.io/ios/2015/06/29/watch开发-初级.html) ***
 
#Apple Watch 开发初级

##运行机制 （此图说明了一切）
![运行机制图](http://ww2.sinaimg.cn/large/6d49d34djw1etkp5vl682j20tx0h9410.jpg)

在iOS App和WatchKit Extension中，可以看到除了Resources之外，还有Code，但是在WatchKit App中却没有！这说明什么？这说明Watch App没有运算能力， 也就是说它本身并不能处理任何事物，做任何决定，这些事情都是Extension代之完成的。 他是不过是Extension的傀儡而已。
简言之：
Watch应用 只包含与应用程序的用户界面有关的storyboards和资源文件。
WatchKit应用扩展 则包含了用于管理、监听应用程序的用户界面以及响应用户交互的代码。


##UI组成
##### Apple watch只有这三种界面：watch主界面、Glance界面、自定义通知界面

**watch主界面**：Watch应用拥有iOS应用的完整用户界面。用户从主界面启动手表应用，来查看或处理数据。

**Glance界面**：使用glance界面以便在Watch应用上显示即时、相关的信息，该界面是可选的只读界面。并不是所有的Watch应用都需要使用glance界面，但是如果使用了它的话就可以让用户方便地访问iOS应用的数据。

**自定义通知界面**：自定义通知界面可以让您修改默认的本地或远程通知界面，并可以添加自定义图形，内容以及设置格式。自定义通知界面是可选的。

 <!--启动工程，搭建界面-->

##简单说一下项目搭建
首先，上面部分已经了解到appWatch 是捆绑到iOS App中的，APP watch必须依附于iOS app 才能启动。写完这个demo，大家就可以深刻体会到这一点。

**first**. 创建一个iOS app project.

**second**. 选择Xcode-->File-->new-->target(选择Apple watch) next（如图）
![步骤图](http://ww1.sinaimg.cn/large/6d49d34djw1etkp6kfjd8j20ka0byta6.jpg)

此处，选项框部分要根据项目需要添加相关界面。

顾名思义，图中 notification Scene 就是三种界面中的通知界面，Glance Scene 就是三种界面中的Glance界面

OK，到此，Apple watch 工程已初步创建。
##project架构组成
![project架构图](http://ww2.sinaimg.cn/large/6d49d34djw1etkp7b9xqnj20gw0dlgoe.jpg)

**前面讲解运行机制有提到 Apple watch的实际运行 包含两个部件，一个是 WatchKit应用扩展（*WKDemo WatchKit Extension*），一个是Watch应用（*图中WKDemo WatchKit App*），
从图中 可以清晰的看出，WatchKit App 实际只包含一个storyboard 文件和一个image set集
而，WatchKit Extension 却囊括了 所有的watch 视图类。再次印证了前面的运行机制原理图所说的所有的数据 都是由捆绑到iOS APP中的WatchKit Extension完成处理并分别与iPhone、watch App 进行通信的。**

<!--*分别说一下这两部分*-->
### WatchKit app：
WatchKit App只包含一个storyboard 文件和一个image set集 ，之前似乎是喵神的某一篇文章中说：苹果的这一举动暗示了一种趋势----用storyboard 代替手写代码布局的一种趋势，忽略这一信息的可信度，单就目前如今关于Apple watch的界面布局 确实只有storyboard这一种方式，如果你去确认，发现帮助文档上没有任何关于size，point，frame等一些列字段的痕迹。实际上，仅仅story board 去布局，苹果也设置了条条框框去限制，举个栗子，watch 的布局是不存在像iPhone 那样view与view的叠加，简单说就是类似于网页那样平面布局的风格，先看一下效果图吧。
![](http://ww1.sinaimg.cn/large/6d49d34djw1etkp7s4yawj20dr0lyjsh.jpg)

下面以对主界面的布局为例，其他两种界面都大同小异。具体区别，实际应用体会更深刻。
![](http://ww1.sinaimg.cn/large/6d49d34djw1etkp8d824aj211407c0vc.jpg)

所有的WKcontroller 都是用Group来布局的，相当于iOS的view层，区别在于，Group 只是个容器，不像view一样可以对自身的layer等属性做设置，使用Group可以对其内部控件按水平或者竖直方向放置控件（一个Group 只能按一个方向布局），Group内部控件是按由左向右或者由上至下,用一张图来说明一下group的所有属性吧（如下）。

![](http://ww3.sinaimg.cn/large/6d49d34djw1etkp8rm12hj20et0ikq6t.jpg)

关于watch app storyboard的基本使用，了解这些就可以做基本开发了。
其他，诸如动画的使用等情况，就项目要求具体情况具体对待吧。

### WatchKit Extension ：
先看一下WatchKit Extension 的文件组成：（如图）

![Extension的文件组成](http://ww2.sinaimg.cn/large/6d49d34djw1etkp92vtslj207m07175k.jpg)

前面提到watch的UI组成只包括三种界面，所以从字面上可以清楚的看出①代表了主界面的一系列视图控制器，②代表了自定义通知界面视图控制器，③则是代表了glance界面视图控制器。在这里简单提一下，关于glance界面，苹果官方规定，一个watch APP 只能有一个glance界面，用来显示一些重要信息，方便用户预览，另外glance 界面的布局格式，苹果做了严格规范，开发者只能套用苹果给出的几套模板，不可自定义样式。关于通知界面，需要在
![通知界面tip](http://ww1.sinaimg.cn/large/6d49d34djw1etkp9ey2zuj209701z3ym.jpg) 这个文件中做一些相关设置，大家课余研究吧，本人对此界面研究不多。

下面着重来说一下 主界面视图控制器：

![主界面视图控制器](http://ww1.sinaimg.cn/large/6d49d34djw1etkpa05rgyj20ck07qmyg.jpg)


如图，常用的就这三个方法

① - (void)awakeWithContext:(id)context;

② - (void)willActivate;

③ - (void)didDeactivate;

①方法类似于UIViewController 中的viewdidload 方法
②方法类似于UIViewController 中的viewDidAppear 方法
③方法类似于UIViewController 中的viewDidDisappear 方法

①方法 只在watch APP启动时调用一次，app 启动后不会再次调起，当视图控制器之间相互切换时会调起新视图控制器②方法和老视图控制器③方法。

##iPhone 与watch 之间的消息传递
因为watch的数据处理都是捆绑在iPhone 端，如果不唤醒watch 捆绑的ios app ，那么空有wantch app在运行就如同是图形预览，完全失去了app 的意义。所以，watch app启动的时候 都是由watch Extension强制启动了捆绑的ios APP程序，只有当iPhone 端与watch Extension 产生了数据交互，在watch app界面上有所体现，才称的上是一个app。

###watch是如何唤醒iPhone app的
*很简单，苹果提供了这么几个方法*

**一个是watchkit 提供的唤醒父应用程序方法：**

+ (BOOL)openParentApplication:(NSDictionary *)userInfo reply:(void(^)(NSDictionary *replyInfo, NSError *error)) reply;    // launches containing iOS application on the phone. userInfo must be non-nil

**一个是ios8.2之后UIKit新加的响应watch请求的一个方法：**

- (void)application:(UIApplication *)application handleWatchKitExtensionRequest:(NSDictionary *)userInfo reply:(void(^)(NSDictionary *replyInfo))reply NS_AVAILABLE_IOS(8_2);
你只需要在iPhone 工程的AppDelegate 里重写该方法，外加一些相关配置，即可。

上几张图作下说明：

首先在watch Extension 的主视图控制器的启动方法（awakeWithContext方法）中，调用openParentApplication:(NSDictionary *)userInfo reply:(void(^)(NSDictionary *replyInfo, NSError *error)) reply 方法,此处发起打开父程序请求，附代码如下：

![代码1](http://ww3.sinaimg.cn/large/6d49d34djw1etkpajfix6j20kk08dq54.jpg)

其次，在appDelegate里添加application:(UIApplication *)application handleWatchKitExtensionRequest:(NSDictionary *)userInfo reply:(void(^)(NSDictionary *replyInfo))reply 方法，此处接收watch 发送过来的请求。附代码如下：
![代码2](http://ww4.sinaimg.cn/large/6d49d34djw1etkpaxhf2mj20mu06k41x.jpg)

*完成这些代码之后，需要在工程做一点小小配置：*

**在plist文件中，注册对外接口**
找到appname-info.plist文件，点击打开它，在列表中找到URL types（如果没有，可添加一个URL types ）里面的内容设置看下图：
![注册对外接口](http://ww2.sinaimg.cn/large/6d49d34djw1etkpbbsaz7j20h50bwdiz.jpg)


关键Key和Value是 URL identifier => com.future.wkDemo；URL Schemes => wkDemo

<!--到此对外接口已注册好,此时选择watchkit App scheme运行模拟器，已经可以唤醒iPhone 绑定程序了-->

###watch与iPhone app之间是怎么通知彼此的

####watch向iPhone 端发送数据更新的通知方法
![代码1](http://ww1.sinaimg.cn/large/6d49d34djw1etkpbmmxh6j20hq02wq3h.jpg)
![代码2](http://ww1.sinaimg.cn/large/6d49d34djw1etkpbz888sj20na07d0wc.jpg)

####iPhone向watch 端发送数据更新的通知方法（此处用的是达尔文通知，具体原理可搜索关键字--达尔文通知或者CFNotificationCenterGetDarwinNotifyCenter）

![代码1](http://ww4.sinaimg.cn/large/6d49d34djw1etkpcgjlqmj20ii02rt9c.jpg)
![代码2](http://ww4.sinaimg.cn/large/6d49d34djw1etkpctedcoj20if0buq6g.jpg)

##数据共享相关配置 
首先：在Target中，先选择iOS App，在General中的Team选项中选择现有的组（非None选项）
对Extension和WatchKit App也重复上面的步骤

其次：在Capabilities中，打开App Groups选项，增加一个Group并选中
对于Extension也做同样的操作并选中刚才创建的那个Group。如果在此过程中出现红色感叹号就点击“Fix issue”，直到全部正确为止。

完成了设置之后我们就可以设计WatchKit App的界面了， 找到Interface.storyborad，加入需要的元素。

*关于iPhone 与 watch之间的数据共享，Apple 建议还是在 iOS app 中完成，并通过 App Groups 进行数据共享，从而在 Watch Extension 中拿到这些数据。*









