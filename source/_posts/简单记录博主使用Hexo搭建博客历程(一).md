title: 简单记录博主使用Hexo搭建博客历程(一)
date: 2011-05-16 13:58:38
tags: [Hexo, Next, Github, Gitcafe, godaddy, dnspod]
categories: Hexo
---

网上的各种教程鱼龙混杂，自己也一路摸爬滚打，无论怎样，总算将博客搭建成现在这个样子也算小成了，那就这里抛砖引玉，尽量去繁就简，也会将自己所遇到过的问题贴出来供大家参考。

## hexo介绍
这么官方的内容我就贴个链接就好，就不增加阅读量，影响搭建效率了。[Hexo](http://www.oschina.net/p/hexo)
<!--more-->

## hexo安装
### 1. 安装Node.js
直接下载安装即可，[点击下载Node.js](https://nodejs.org/en/)

或者我们可以先安装brewhome，一句话搞定

```
ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)”
```
然后用它来安装Node.js

```
#安装命令
brew install node  #如果我没记错的话,最新版的node.js的包中已经集成了npm包管理工具

使用以下命令验证是否安装成功
node -v
npm -v

如果npm未安装成功，那么执行以下语句
npm install 
```

### 2. 安装Git


用brew来安装git，还是一句话

```
sudo brew install git 
```

### 3. 安装hexo
Node, npm和Git都安装成功, 开始安装hexo

```
// 此前最好先自定义一个文件夹，然后在此文件夹内进行hexo的安装
mkdir hexo  // hexo是我在根目录下新建的文件夹
cd ~/hexo	// 打开hexo文件夹
npm install hexo -g  #-g表示全局安装, npm默认为当前项目安装
```
hexo常用命令

```
hexo init <folder>  #执行init命令初始化hexo到你指定的目录 
如：hexo init ~/hexo
hexo generate       #自动根据当前目录下文件,生成静态网页
hexo server         #运行本地服务,
```
浏览器输入 [http://localhost:4000](http://localhost:4000) 就可以看到效果。

---
### 4. 部署到Github or GitCafe
在本地看到效果后，就要把你的hexo部署到服务器上了，Github和GitCafe都可以选择，因为我们的博客主要还是供国内用户使用，因此这里选择的是GitCafe，如果仍然想使用Github的朋友可以搜索其他教程进行参考。

GitCafe, 感觉就是GitHub的国内版，访问速度比GitHub快

注册之类的东西就不说啦，注册以后，创建一个项目：
##### *项目名一定要与GitCafe的ID一致*
![pic1](http://img.blog.csdn.net/20151119113120653)

接下来设置SSH
#### 1).启动Git，创建一个存放SSH的目录：
```
mkdir ~/.ssh
```

#### 2).生成新的SSH公钥：

```
ssh-keygen -t rsa -C "你的邮箱@xxx.com" -f ~/.ssh/gitcafe

```
#### 3).在SSH的文件夹下，可以看到gitcafe私钥和公钥文件：
```
gitcafe
gitcafe.pub
```
#### 4).生成配置文件
```
touch ~/.ssh/config
```
添加以下内容：

```
Host gitcafe.com www.gitcafe.com
IdentityFile ~/.ssh/gitcafe
```


#### 5).进入 GitCafe —>账户设置—>SSH 公钥管理设置项

![pic2](http://img.blog.csdn.net/20151119113101672)
![pic3](http://img.blog.csdn.net/20151119113111291)
![pic4](http://img.blog.csdn.net/20151119113131288)

将SSH文件夹下的GitCafe.pub，中的内容复制到公钥框中即可。

#### 6).测试是否可以连接GitCafe服务器

```
ssh -T git@gitcafe.com -i ~/.ssh/gitcafe
```

![pic5](http://img.blog.csdn.net/20151119113526038)
ok,到这里就只差一步啦，加油继续！
#### 7).修改Hexo，并部署到GitCafe！

修改hexo目录下的_config.yml文件（以后会简称hexo配置文件）
附上自己的配置文件代码

部署的部分是 `deploy:`下的内容

```
# Hexo Configuration
## Docs: http://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site
title: 科勒的小刀
subtitle: be a quiet man
description: iOS | life
author: Kohler Chen
language: zh-Hans
email: ikohler7c@gmail.com
timezone:

# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://yoursite.com
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:

# Directory
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:

# Writing
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link: true # Open external links in new tab
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
future: true
highlight:
  enable: true
  line_number: true
  auto_detect: true
  tab_replace:

# Category & Tag
default_category: uncategorized
category_map:
tag_map:

# Archives 默认值为2，这里都修改为1，相应页面就只会列出标题，而非全文
## 2: Enable pagination
## 1: Disable pagination
## 0: Fully Disable
archive: 1
category: 1
tag: 1

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss

# Pagination
## Set per_page to 0 to disable pagination
per_page: 10
pagination_dir: page

duoshuo_shortname: kohmax
swiftype_key: wyXNFUWN_N8FU9Bcysd3

# Extensions
## Plugins: http://hexo.io/plugins/
## Themes: http://hexo.io/themes/
theme: next
exclude_generator:
plugins:
- hexo-generator-feed
- hexo-generator-sitemap

# Deployment  站点部署到github要配置这里, 非常重要
## Docs: http://hexo.io/docs/deployment.html

****************重点部分****************
deploy:
  type: git    #部署类型
  repository: git@gitcafe.com:ikohler/ikohler.git  #部署的仓库的SSH，把ikohler替换成自己的gitcafe名称
  branch: gitcafe-pages  #部署分支,一般使用master主分支
plugins: 
- hexo-generator-feed   #此插件用于RSS订阅, 以后有时间再介绍
```

保存之后就可以上传了

```
hexo deploy
```
---
> ### *注意！注意！这一步很有可能遇到部署失败的问题，会提示找不到git，博主也是因此卡壳了好久，那是因为在Hexo 3.0版本后deploy git 被分开的，所以需要安装，安装命令如下:`npm install hexo-deployer-git --save`,安装好后在尝试一下就ok。*
  
     
       
         
           

如果还有其他问题，可以[点击此链接](http://blog.csdn.net/wx_962464/article/details/44786929)，这是一个问题汇总。

---

如果你已经做好了这一步，那就是大功告成啦，可以通过[ikohler.gitcafe.io](ikohler.gitcafe.io)访问博客啦!

如果你有域名的话，也可以绑定自己的域名，很简单，官方的wiki写的十分清楚了，[传送门](https://gitcafe.com/GitCafe/Help/wiki/Pages-%E7%9B%B8%E5%85%B3%E5%B8%AE%E5%8A%A9)




如果你也想拥有自己的个性域名，下一遍我会继续分享。


---
#### tips

hexo现在支持更加简单的命令格式了，比如：

- hexo g == hexo generate

- hexo d == hexo deploy

- hexo s == hexo server

- hexo n == hexo new

- hexo g -d == hexo g  +  hexo d   // 每次更新配置或者文章都需要使用一次

