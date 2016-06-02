title: Sublime Text 3 - Full Stack developer的编辑器
categories: 必先利其器
date: 2015-12-28 11:58:57
tags: [JavaScript, Sublime Text]
---
## 1. 安装Sublime Text 3

百度网盘链接: [http://pan.baidu.com/s/1qWOE5L6](http://pan.baidu.com/s/1qWOE5L6) 密码: vuch

## 2. 插件 

### 2.1 Package Control

- 按Ctrl+`调出console（注：安装有QQ输入法的这个快捷键会有冲突的，输入法属性设置-输入法管理-取消热键切换至QQ拼音）
- 粘贴以下代码到底部命令行并回车：
<!--more-->

```
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```

如果是 Sublime Text 2，代码为

```
import urllib2,os; pf='Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler( ))); open( os.path.join( ipp, pf), 'wb' ).write( urllib2.urlopen( 'http://sublime.wbond.net/' +pf.replace( ' ','%20' )).read()); print( 'Please restart Sublime Text to finish installation')
```

### 2.2 Emmet

神器，可以自动生成诸多Html格式，快捷键 ctrl + E 或者 Tab
输入 ! 按下Tab，可以自动生成Html格式，如下图所示

<img src="http://7xusmm.com1.z0.glb.clouddn.com/mdJozo/1464696019071.png" width="122"/>

再者，输入一些常用标签，如 div, link等，也会有生成相应格式

<img src="http://7xusmm.com1.z0.glb.clouddn.com/mdJozo/1464696113139.png" width="125"/>

进阶版的话可以去这个网站看一下

>  [前端开发必备！Emmet使用手册](http://www.w3cplus.com/tools/emmet-cheat-sheet.html)




### 2.3 AndyJs2

下载地址[http://pan.baidu.com/s/1ntmO1QD](http://pan.baidu.com/s/1ntmO1QD)

下载后解压缩放在安装目录下Packages文件夹即可

### 2.4 ColorPicker

一款颜色选择器，默认快捷键 Shift + Cmd + C

### 2.5 Insert Callback

按下快捷键 alt + C 后，可以快速插入回调函数。

### 2.6 GotoDocumentation
快速查看手册，快捷键super+shift+h

### 2.7 Git
不解释

### 2.8 DocBlackr

快速注释神器

### 2.9 advanceNewFile

可以直接在当前目录下创建新文件，从WebStorm过来的同学可以找到一点熟悉感

### 2.10 autofilename

当你要引入文件的时候，自动提示引入的文件名

### 2.11 SyncedSideBar

打开文件时自动打开文件所在目录

### 2.12 Inc Dec Value

alt+鼠标滚轮或者alt+上下，使得数值变化

### 2.13 alignment

对齐格式，快捷键ctrl + A

## 3. JS控制台

源网址[http://www.tuicool.com/articles/Uf2uauv](http://www.tuicool.com/articles/Uf2uauv)
