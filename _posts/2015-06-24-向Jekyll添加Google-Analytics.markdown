---
layout: post
title:  "向Jekyll添加Google Analytics"
date:   2015-06-24 17:25:32
categories: blog
analytics : true
tags: Jekyll google analytics
---

# 由于要用Jekyll + github page搭建blog，想要添加analytics的功能，便于管理。便从网上找了方法，记录总结下来，与大家分享。


# 背景知识不多说，直接上干货：                  
### 修改`_config.yml`文件，可以直接把统计跟踪的信息写在这里，也可以使用自定义的方式。使用自定义的方式可以方便以后升级，否则文件可能会被覆盖，推荐！      

     analytics:       
     provider: custom      


### 然后再`_include`目录下，建立一个`custom`目录，再在这个目录下新建一个文件`analytics`，如下:
     cd _includes       
     mkdir custom     
     cd custom     
     touch analytics          


### 在新建的`analytics`文件内填写Google Analytics的跟踪代码：

{% highlight javascript %}
<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','//www.google-analytics.com/analytics.js','ga');

  ga('create', '这是跟踪ID', 'auto');
  ga('send', 'pageview');
</script>
{% endhighlight %}
  

***

## 如果需要关闭对某个单独页面的跟踪统计的话，关闭Analytics的方法，上图先：

![提示图](http://ww2.sinaimg.cn/large/0069kYsZjw1etfbviyllpj30d705et99.jpg)

## markdown文件头添加 `analytics: false`即可关闭对该blog文件的跟踪统计，图中为打开状态。

PS:可以在sublime里建议blog的markdown模板，以后直接填写内容就可以了，很简单吧。

***

> 参考资料：
  [Blog Configuration](http://jekyllbootstrap.com/usage/blog-configuration.html)


