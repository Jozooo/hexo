title: 将Hexo博客从GitCafe转移到Github时所遇到的问题详解
categories: Hexo
date: 2016-05-25 15:14:46
tags: [Hexo, Github, GitCafe, coding]
---


近日博客频频登陆不上，遂登入GitCafe看看是啥状况，谁知看到这一惊天“噩耗”，虽然合并本是一件好事，但由于之前访问仓库频繁的不稳定，以及实在懒得再Coding上再花时间部署，于是就将博客仓库转到了比较熟悉的Github上来，不过坑依然很多，下面择取几个讲讲看

![](http://img.blog.csdn.net/20160528155002415)

### 1. 每次 hexo d 都要输入github的账号密码
这一操作实在是令人抓狂，不过解决办法也相当简单。
<!--more-->
![](http://img.blog.csdn.net/20160528155009504)

如果**repository**之前是使用**https://github.com/xxx/xxx.github.io.git**，改为**git@github.com:xxx/xxx.github.io.git**即可。

> 本文所有“xxx”均指github用户名

### 2. Github自定义域名
Github的设置里面，并没有直接可以设置自定义域名的选项，这就导致，我通过 xxx.github.io能访问到了博客，但是通过www.kohmax.com却访问不了，一直显示Github Pages Error..这需要我们创建一个**CNAME**文件，记住要全大写，且没有后缀名，用文本编辑器打开，填入自己的自定义域名，不要加“https://”的前缀，如下图

![](http://img.blog.csdn.net/20160528155014493)

那么这个文件上传到哪里呢？网上的好多攻略没有交代太清楚，导致我饶了很久，后来看了下Github的Help，确定把这个文件上传到博客仓库的根目录下，即“xxx.github.io”这个repositorie，放好之后刷新，点击改仓库的Setting。

![](http://img.blog.csdn.net/20160528155510901)

![](http://img.blog.csdn.net/20160528155024942)

出现上图图所示的情况时，则代表已经成功，然后通过域名访问博客即可。


### 3. 域名的购买与管理
博主域名是在[goDaddy](https://sg.godaddy.com)上买的，默认英文，可以选择地区到新加坡切换到中文页面。

域名管理是在[DNSPod](https://www.dnspod.cn)，当然在DNSPod也可以购买域名，有时反而更便宜，可以自行比较。

![](http://img.blog.csdn.net/20160528155020066)

在DNSPod进行上述设置，其中马赛克的部分是通过控制台输入“Ping xxx.github.io”得来的IP地址，分别填入即可，同时，在goDaddy的域名管理中进行下面一系列操作。目的是将DNSPod里的默认记录值“f1g1ns1.dnspod.net”，“f1g1ns2.dnspod.net”写入域名管理中。

![](http://img.blog.csdn.net/20160528155030540)

#### 然后选择需要管理的域名

![](http://img.blog.csdn.net/20160528155034473)

![](http://img.blog.csdn.net/20160528155040118)


暂时只有这么多了，后续有问题继续补充。








That's all, thank you.