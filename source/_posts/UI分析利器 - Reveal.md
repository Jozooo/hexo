title: UI分析利器 - Reveal
categories: 必先利其器
date: 2016-01-20 12:21:17
tags: [iOS, UI, Reveal, 工具, Mac app]
---
在iOS开发中，UI是十分重要的一环，它直观的展示给用户，在日益强调用户体验的今天，UI的优劣，可以立竿见影的影响用户对其体验的判断，平常不管用真机还是模拟器，虽然没有方便但分析力度微乎其微，而Reveal这个工具，适时的弥补了这一不足，是我们学习UI或者日常工作使用UI的辅助神器。

### 一. 基础 - Reveal查看自己的app

1. 打开Reveal（[http://revealapp.com](http://revealapp.com)下载）

2. Reveal —> Help —> Show Reveal Library in Finder
<!--more-->
![](http://img.blog.csdn.net/20160120144717372)

3. 打开Xcode —> 导入Reveal.framework至当前项目中
![](http://img.blog.csdn.net/20160120144913919)
此外，若报错，还需导入CoreGraphics, QuartzCore, CFNetwork 这三个库

4. 工程设置中，在Other Linker Flags项增加-ObjC -framework Reveal    

![](http://img.blog.csdn.net/20160120145105498)

5. 运行当前项目后，打开Reveal，选择当前运行程序进行关联
![](http://img.blog.csdn.net/20160120145246265)

6. 连接成功后，应用的UI层次 元素都可以妥妥的看到了
![](http://img.blog.csdn.net/20160120145415017)

### 二. 进阶 - Reveal查看任意app的高级技巧

1. 越狱设备，iPhone/iTouch/iPad都可以，iOS6以上；
2. 安装Reveal，Trail或正式版都可以，然后越狱设备与安装Reveal的Mac在同一wifi内。
3. 仍然是点击菜单 -> Help -> Show Reveal Library in Finder，但这次选取另一个文件 libReveal.dylib

4. 将libReveal.dylib上传到设备的/Library/MobileSubstrate/DynamicLibraries
![](http://img.blog.csdn.net/20160120145558192)
5. 编辑并上传一个libReveal.plist(可指定多个BundleID以监控多个app，不上传libReveal.plist以监控所有app)
![](http://img.blog.csdn.net/20160120145630271)

6. re-spring或重启iOS设备，打开你想看的app，再从Reveal界面左上角选择要连接的机器，进入不同的页面之后还可以点击右上角的刷新钮来刷新监测的页面信息。


以上是不写一行代码就能够查看任意app的方法，各位看别人app爽的时候，也可以摸摸脖子想想自己的app。不过需要注意的是，若你选择了这种方式，那么法一中导入的`Reveal.framework`需要予以删除。

### 三. 题外话 - Reveal“破解”的方法

相信大家都对Reveal 30天的试用十分蛋疼，而89美刀的高价也让人望而却步，大家都是开发者，我还是希望有能力的同学可以去购买正版应用，支持正版，也给辛勤的开发者一点鼓励，但我还是贴出“破解”方法吧。
 
 - 比较简单粗暴的则是修改系统时间并关闭自动设置日期

 - 显然法一并不符合我们Coder的做法，正确的画风是 [ -> http://download.csdn.net/download/ikohler/9411550 <- ](http://download.csdn.net/download/ikohler/9411550)是我在CSDN上传的破解版，其实就是在官方原版的基础上替换了一个文件，十分简单，相信大家都可以很快破解。
 
 

### 四. 题外话 - iOS 9.x 的越狱

这个网上教程很多，不过不太推荐某度搜索首条的XY（无脑推广），还是用老牌的盘古，安全放心，[传送门 -> http://www.25pp.com/news/news_78659.html](http://www.25pp.com/news/news_78659.html)

不过值得注意的是，即使越狱了也会存在仍然找不到`Library`文件夹的情况，且PP助手也仍然显示设备未越狱，那么请检查以下几个补丁是否安装

 - Apple File Conduit "2" （需要添加源“apt.25pp.com”）
 - afc2add
 
 
安装完毕后重启设备，`Library`文件夹就可以轻松找到啦。








That's all, thank you.