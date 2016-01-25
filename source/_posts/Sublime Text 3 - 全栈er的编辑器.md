title: Sublime Text 3 - 全栈er的编辑器
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

### 2.2 JSFormat

### 2.3 AndyJs2

下载地址[http://pan.baidu.com/s/1ntmO1QD](http://pan.baidu.com/s/1ntmO1QD)

下载后解压缩放在安装目录下Packages文件夹即可

### 2.4 ColorPicker

快捷键 Shift + Cmd + C

### 2.5 MarkDown Editing

### 2.6 Ctags

下载ctags-5.8.tar.gz源码包  [http://download.csdn.net/detail/jiuyueguang/5711867](http://download.csdn.net/detail/jiuyueguang/5711867)

配置方法 [http://jingyan.baidu.com/article/48206aeafba820216ad6b3f5.html](http://jingyan.baidu.com/article/48206aeafba820216ad6b3f5.html)

### 2.7 Git

## 3. JS控制台

源网址[http://www.tuicool.com/articles/Uf2uauv](http://www.tuicool.com/articles/Uf2uauv)
