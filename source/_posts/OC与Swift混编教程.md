title: OC与Swift混编教程
categories: Swift
date: 2016-01-04 16:43:14
tags: [Objective-C, Swift, 混编, Xcode]
---

本文会介绍一下OC与Swift的混编教程，按步骤来，简单易懂

### 1. 创建一个swift或者oc的工程

我这里是创建的Swift语言的工程

### 2. 在工程中代码目录下创建一个oc的类，选择oc语言

之后会出一个对话框，选择Create Bridging Header

<!--more-->
![](http://img.blog.csdn.net/20160104165138216)

> ##### 如果没有提示这一步，可以自己手动设置
![](http://img.blog.csdn.net/20160106102111878)

### 3. 这时会在工程里看到下图这样一个头文件  

![](http://img.blog.csdn.net/20160104165151903)

### 4. 在这个头文件里添加你的OC文件的.h文件

就和OC中的引入差不多，这样做后就可以在任意swift文件中自行调用所包含的oc文件了。 
![](http://img.blog.csdn.net/20160104165209436)
### 5. 接下来在工程的target-》build Setting->package下个性如下两项 
![](http://img.blog.csdn.net/20160104165215673)

### 6. 然后在OC代码的.m文件中引入  + “-swift.h” 这样一个头文件

如下图所示，这步之后你的.m文件就可以随便调用swift文件了。
![](http://img.blog.csdn.net/20160104165221793)

其实如果你设置的`Defines Module = YES`了，xcode就会默认生成`"Product Module Name"-swift.h`这样一个头文件，这个头文件下会有你所有.swift文件的.h信息。 