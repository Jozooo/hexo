title: iOS9与XCode7中不能使用http连接的解决办法
categories: iOS
date: 2015-11-18 10:58:00
tags: [iOS, XCode7, http, info.plist]
---

在Xcode7.0及以上版本中编译iOS APP时，默认会使用iOS9的一项新特性，使得所有http连接被禁用，项目里使用的API没有https支持，就悲剧了。差了官方文档，有这么一段话
<!--more-->
> App Transport Security

> App Transport Security (ATS) enforces best practices in the secure connections between an app and its back end. ATS prevents accidental disclosure, provides secure default behavior, and is easy to adopt; it is also on by default in iOS 9 and OS X v10.11. You should adopt ATS as soon as possible, regardless of whether you’re creating a new app or updating an existing one.

> If you’re developing a new app, you should use HTTPS exclusively. If you have an existing app, you should use HTTPS as much as you can right now, and create a plan for migrating the rest of your app as soon as possible. In addition, your communication through higher-level APIs needs to be encrypted using TLS version 1.2 with forward secrecy. If you try to make a connection that doesn't follow this requirement, an error is thrown. If your app needs to make a request to an insecure domain, you have to specify this domain in your app's Info.plist file.

在这里面可以看到，通过修改Info.plist文件可以继续使用http连接，具体的方法如下：

##### 1. 在项目左侧找到Info.plist文件，可以通过Filter来搜索
![](http://img.blog.csdn.net/20151218110401016)
##### 2. 在右侧点击Add Row添加NSAppTransportSecurity，类型为Dictionary
然后再添加子项目NSAllowsArbitraryLoads类行为Boolean值为YES
![](http://img.blog.csdn.net/20151218110406265)

这样就可以重新使用普通的http连接了。不过有条件的话，还是去搞一个https吧