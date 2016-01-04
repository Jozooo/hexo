title: '[iOS开发]NSPredicate谓词的用法详解'
categories: iOS
date: 2015-12-18 10:49:11
tags: [iOS, NSPredicate, NSArray]
---

### 1. 引
在开发中也许会需要用到过滤数组的情况，如**本机通讯录中新增或者删除了联系人后，App端读取通讯录时的处理**，因为我们往往会把通讯录先保存到本地数据库，这样读取时就会更加迅捷，而一旦源数据改变了，怎样快速更新村粗在本地数据库中的数据呢？这就是本文要探讨的内容

> 举个例子

<!--more-->

```
NSArray *arr1 = @[@"1", @"2", @"3", @"6"];

NSArray *arr2 = @[@"1", @"2", @"3", @"4", @"5"];

NSPredicate * filterPredicate = [NSPredicate predicateWithFormat:@"NOT (SELF IN %@)",arr1];

//过滤数组

NSArray * reslutFilteredArray1 = [arr2 filteredArrayUsingPredicate:filterPredicate];

NSLog(@"Reslut Filtered Array = %@",reslutFilteredArray1);
    
    
    
filterPredicate = [NSPredicate predicateWithFormat:@"NOT (SELF IN %@)",arr2];
NSArray * reslutFilteredArray2 = [arr1 filteredArrayUsingPredicate:filterPredicate];

NSLog(@"Reslut Filtered Array = %@",reslutFilteredArray2);
    
```

以上两数组打印的结果分别是`{4,5}`和`{6}`,即arr2相较arr1多余的元素，以及arr1相较arr2多余的元素，之后的操作就要看具体需求了。

### 2. 文
当然，上面赘述了那么多，无非是想引出一个关键字`NSPredicate`，下面我将继续挖掘。