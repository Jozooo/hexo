title: 'Swift_Singleton，深入浅出单例设计模式'
categories: Swift
date: 2015-11-17 00:21:13
tags: [iOS, Swift, Singleton, 单例]
---

### 单例设计模式

开发项目的时候，有时候需要很多工具类，这时候单例的使用就尤为重要。在OC中，单例的写法想必大家很清晰了，在这里给大家介绍下Swift中几种单例的写法：

#### 1. 最直接的写法

```
static var instance: DBHelper? = nil
static func sharedInstance() -> DBHelper {

    if instance == nil {
        instance = DBHelper()
    }

    return instance!
}
```
<!--more-->
> 这种写法的缺陷在于，没有处理线程安全的问题

#### 2. 使用GCD

```
static var sharedInstance: DBHelper {

    struct Static {
        static var onceToken : dispatch_once_t = 0
        static var instance : DBHelper? = nil
    }

    dispatch_once(&Static.onceToken) {
        Static.instance = DBHelper()
    }

    return Static.instance!
}
```
> 使用GCD可以帮助我们处理线程安全的问题，但缺陷在于写法太麻烦

#### 3. 使用let变量简单的写法

```
class var sharedInstance: DBHelper {

    struct Static {
        static let instance: DBHelper = DBHelper()
    }

    return Static.instance
}
```

> 使用常量来实现单例对象，虽然没有问题，但是写法还不够简洁


#### 4. 最方便的写法

```
private static let instance = DBHelper()
class var sharedInstance: DBHelper {

    return instance
}
```

> 本人用的最多的也就是最下面这一种了，十分简单明了，也希望大家能喜欢。