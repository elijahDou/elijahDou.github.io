---
layout: post
title:  "Xcode Could not launch-failed to get the task for progress"
date:   2015-06-24 14:09:32
categories: blog
---

# 在测试项目代码时，选择debug版本编译通过，测试正常；在release版本下，可以编译成功，但是APP一在真机上运行就会报如题所示的错误，程序闪退。


# 原来是Build Setting中Code Signing的Code Signing Identity配置问题。使用的签名是distribution certificate，即发布者证书，它不允许在真机上直接运行。


![示意图](http://ww1.sinaimg.cn/large/0069kYsZjw1etf8cztv0qj30ic05hgm9.jpg)


#### **换用开发者证书就OK了。**




<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','//www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-64460072-1', 'auto');
  ga('send', 'pageview');

</script>

