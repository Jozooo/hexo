title: Swift_Refactor，可视化编程利器
categories: Swift
date: 2016-01-18 17:53:20
tags: [Swift, iOS, 可视化编程, Storyboard, Refactor, 团队开发]
---

Refactor是iOS9推出的新特性，如果还不会用实在是太暴殄天物了*（虽然我也是今天才开始用的）*

尽管平时开发我更倾向于纯代码编程，很享受一连串代码行云流水的敲打过程，但不得不说可视化编程在有些时候会更加方便且快速。而之所以搁浅可视化编程是因为在团队开发时，会遇到一些不太友好的操作，当每个成员使用了一个Storyboard，这样项目就存在了很多个Storyboard，最后我们需要通过代码，将多个Storyboard整合在一起的，实在是非常的麻烦。而有了这个`Storyboard Refactor`，这将变得超级简单。
<!--more-->
#### 1. 使用Storyboard搭建一个框架 
![](http://img.blog.csdn.net/20160118180641068)
仅仅一个简单框架下的Storyboard中就存在了很多的控制器，操作起来也非常的麻烦。并且在团队开发的过程中，同时处理这一个Storyboard会造成svn/git同步混乱等问题

比较好的方法是：把每一个模块都抽成一个Storyboard，然后分别在自己里面处理，团队开发的时候，也不至于修改了别人的文件。

之前我们都是通过代码进行关联，现在则可以使用新的工具Refactor。

#### 2. 选中需要抽离的控制器
![](http://img.blog.csdn.net/20160118180748319)



#### 3. 选择菜单 -> Editor -> Refactor
![](http://img.blog.csdn.net/20160118180646098)

#### 4. 输入名称
![](http://img.blog.csdn.net/20160118180650689)

保存之后原来的Main.storyboard会相应的改变如下图。

![](http://img.blog.csdn.net/20160118180655308)

图中标记圈中的玩意儿就是Storyboard Reference，它代表一个Storyboard，这样就可以从一个Storyboard关联到另一个Storyboard了，然后你可以在一个新的storyboard里找到刚才抽离的控制器，如下图所示

![](http://img.blog.csdn.net/20160118180701222)

团队里的成员只需在自己的storyboard里操作自己的模块即可，省去了一些麻烦，当然，Refactor还有其他的功能，
大家一起去挖掘吧。






That's all, thank you.