title: 'Swift，截取字符串'
categories: Swift
date: 2015-11-27 01:11:52
tags: [iOS, Swift, String, 截取字符串]
---

下面介绍2种swift的字符串截取方法,实际上用到了

- substringFromIndex
- substringToIndex
- substringWithRange

这三个方法，相信大家在OC中肯定对此不会陌生，因此我们可以

##### 1. 将String转化为NSString再截取
具体做法如下

```
let str = (self as NSString).substringFromIndex(1)
```
<!--more-->
##### 2. 直接调用String的对应方法(推荐)

由于String对应的方法参数是String.Index类型而非Int类型,所以我们首先要创建String.Index类型参数值,代码如下

```
// var str="1234567890"

// let index = advance(str.startIndex, 5)  swift 1.x
// let index2 = advance(str.endIndex, -6)  swift 1.x


var str ="1234567890"

let index = str.startIndex.advancedBy(5) 		//swift 2.0+
let index2 = str.endIndex.advancedBy(-6) 		//swift 2.0+
var range = Range<String.Index>(start: index2,end: index)

var str1:String=str.substringFromIndex(index)
var str2:String=str.substringToIndex(index2)
var str3=str.substringWithRange(range)

print(str1)//67890
print(str2)//1234
print(str3)//5

```

当然，上诉设计到`Range`时会比较繁琐，因此我们可以扩展String

```

var str="1234567890"

// 扩展String
extension String {
    subscript (range: Range<Int>) -> String {
        get {
            let startIndex = self.startIndex.advancedBy(range.startIndex)
            let endIndex = self.startIndex.advancedBy(range.endIndex)

            return self[Range(start: startIndex, end: endIndex)]
        }
    }
}

var str4 = str[2...4]
print(str4);//345

```
扩展后我们需要截取时直接使用`aString[1...2]`这样的语句就好了，是不是十分方便呢，赶快用起来吧。

That's all, thank you.