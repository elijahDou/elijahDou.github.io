---
layout: post
title:  "Package Control:There are no packages available for installation的解决方法"
date:   2015-06-24 15:09:32
categories: jekyll update
---

# 原谅我不想学vim，我感觉它变态。现在好的编辑器也挺多，我主要用sublime。


# 在安装完Package Control后，安装其他插件包的时候，遇到了错误：Package Control:There are no packages available for installation。 


# Holy shit，这是什么鬼，Google之，[StackOverFlow](http://stackoverflow.com/questions/25105139/sublime-text-2-there-are-no-packages-available-for-installation)上面解答的是IPv6的问题，我们配置一下hosts文件可以解决之。


# 具体方法：
## 在终端中输入命令 sudo sublime /etc/hosts  （鄙视我的用Vimba）
## 在hosts文件最后添加：   
- '# to fix sublime Package Control IPV6 issue'
- '50.116.34.243 sublime.wbond.net'
- '#end'
## 重启sublime，再try一下试试。


# [这里](http://blog.csdn.net/freshlover/article/details/44261229)有一篇中文文章，还介绍了其他两种sublime常见问题的解决方法。


# mark一下。