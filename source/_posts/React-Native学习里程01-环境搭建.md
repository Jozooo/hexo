title: React Native学习里程01--环境搭建
categories: React Native(js)
date: 2016-01-13 15:41:34
tags: [React Native, JavaScript]
---

#### 1.安装node.js

- nodejs版本需要在4.0以上,官网下载安装即可[http://nodejs.cn](http://nodejs.cn)


#### 2.安装Homebrew

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
<!--more-->
#### 3.安装watchman以及flow

```
brew install watchman
brew install flow
```

#### 4.安装react-native-cli。有两种方式，
- 第一种：直接在命令行输入：`npm install -g react-native-cli`

- 第二种：在[https://github.com/facebook/react-native/](https://github.com/facebook/react-native/)上将源码下载下来,之后切换到react-native-cli文件夹，然后使用npm install -g进行安装.
如果安装失败，在命令前加上sudo，再进行尝试

#### 5.创建项目

- 使用react-native init "Proj Name" 来创建项目

因为npm的registry是在国外，访问很慢，甚至可能无法创建完成。
所以需要设置以下两条来将镜像指向国内的镜像（第二条常被遗漏）

```
npm config set registry https://registry.npm.taobao.org
npm config set disturl https://npm.taobao.org/dist
```
要看镜像地址配置是否成功，可以使用npm config list来进行查看配置是否生效。

> *PS*: 即使是指向国内的镜像也会很慢，如果想要查看进度，可以在后面加上 **--verbose** 来查看详细信息。
有的时候使用淘宝的镜像创建项目的时候会出现问题，此时可以使用VPN来下载、


#### 6.项目的运行
- iOS，使用Xcode打开项目目录下的iOS目录中的iOS的项目文件（*.xcodeproj）,点击运行即可。
- Android，在项目的根目录使用`react-native run-android`命令直接运行。



> *PS*: 对于在本机的模拟器来说，直接运行是没有问题的，但是如果要在手机上运行的话，则需要改变ip地址才行。对于IOS项目来说，可以在AppDelegate.m文件中的
jsCodeLocation = [NSURL URLWithString:@"http://localhost:8081/index.ios.bundle?platform=ios&dev=true"];
localhost 改成对应的ip地址即可。（PS：即服务器的地址。）


对于Android项目来说，则可以不用修改，直接摇一摇，然后选择dev settings。然后输入对应的ip地址加端口号即可。（PS：即服务器的地址。）
对于Android 5.0以上的设备，如果在运行的时候没有内容，则需要在命令行输入 adb reverse tcp:8081 tcp:8081 ，之后Reload JS 即可。

对于已经安装的设备，如果是因为各种缘故导致服务器关闭了，可以直接在项目的根目录，
直接运行 npm start ，然后重新进入App ，或者摇一摇选择reload js。
Android设备在摇一摇或者点击menu按钮都可以出现悬浮框，如果没有出现，可以去 设置 —> 应用设置 —> 指定应用 -> 显示悬浮框。选中即可。

***(注:安卓部分摘录自网络)***


> *PS*：值得注意的是，iOS项目需要检查AppDelegate.m 与 index.ios.js 两个文件中的以下两行代码
![](http://img.blog.csdn.net/20160113162319213)
![](http://img.blog.csdn.net/20160113162327158)

> 两个地方必须保持一致

#### 7.项目的调试
React Native 的项目可以在Chrome上进行调试。
可以在Chrome上安装**React Developer Tools**  ，调试工具可以在Chrome的应用商店上下载。

如果因为条件限制不能访问Chrome的应用商店，可以在[https://github.com/facebook/react-devtools/releases](https://github.com/facebook/react-devtools/releases)上下载到工具。

<!--安装完调试工具之后，可以在运行的设备上摇一摇，然后选择Debug in Chrome即可开始调试。调试的方法和调试网页的方法一样，可以打断点，可以看输出，都很方便。

PS：这个地方有个小坑。如果开启了Debug in Chrome 的设备在运行的时候，出现了 Runtime is not ready 的错误，此时，是因为你的Chrome没有打开调试的网页。可以选择打开调试的网页（http://localhost:8081/debugger-ui），或者关闭设备的Debug in Chrome 模式即可、如果Chrome无法打开上述的调试网页，请检查你的网络，是否开了VPN或者各种网络代理之类的。而且，localhost是可能会随着服务器的地址变化而发生改变，请注意保持一致。
![](http://img.blog.csdn.net/20160113161927806)-->

That's all, thank you.