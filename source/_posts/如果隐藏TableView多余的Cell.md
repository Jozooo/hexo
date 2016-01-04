title: 如何隐藏UITableView多余的Cell
categories: iOS
date: 2014-12-11 11:06:54
tags: [iOS, UITableView, Cell]
---

在我们的日常开发中，UITableView想必是一个十分熟悉的控件，几乎每个页面都要用到它，本文就讲一讲一些关于UITableView的实用小操作。

## 1. 如何隐藏UITableView多余的Cell

所谓多余的cell便是指那些没有赋值的cell。如果没有隐藏分割线，或者tableView与父view颜色不一致，而恰巧有值cell的行数不足以填充整个屏幕，由于cell的复用性，导致空的cell会填充屏幕，如下图所示。
<!--more-->
![](http://img.blog.csdn.net/20151218110810943)

我们只需调用几行代码即可解决上诉问题

> #### 法一：
因为这是一个比较常用的方法，所以我将它写在了UITableView的Category里。

```
//
//  UITableView+UCSExtraCellHidden.h
//  
//
//  Created by 陈怀远 on 15/8/24.
//  Copyright (c) 2015年 Brandan. All rights reserved.
//

#import <UIKit/UIKit.h>

@interface UITableView (UCSExtraCellHidden)
-(void)setExtraCellLineHidden: (UITableView *)tableView;
@end

```

```
//
//  UITableView+UCSExtraCellHidden.m
//  
//
#import "UITableView+UCSExtraCellHidden.h"

@implementation UITableView (UCSExtraCellHidden)

-(void)setExtraCellLineHidden: (UITableView *)tableView
{
    UIView *view = [UIView new];
    
    view.backgroundColor = [UIColor clearColor];
    
    [tableView setTableFooterView:view];
}

@end
```
接着，在所需要用到的地方直接调用即可

```
- (void)viewWillAppear:(BOOL)animated
{
    [super viewWillAppear:YES];
    
    [self.tableView setExtraCellLineHidden:self.tableView];
    
}

```

调用后效果如下

![](http://img.blog.csdn.net/20151218110834957)

> #### 法二：
当然你也可以屏蔽系统自带的分割线，自己定义分割线，也可以达到上图所示的效果。

**1. 设置自带的分割线样式为空**

```
_tableView.separatorStyle = UITableViewCellSeparatorStyleNone;
```

**2. 在cell的初始化中添加lineView**

```
self.lineView = [[UIView alloc] initWithFrame:CGRectMake(0, kCellHeight - 0.5, kScreenWidth, 0.5)]; 	// kCellHeight:cell高度   kScreenWidth：屏幕宽度
        _lineView.backgroundColor = UIColorFromRGB(0xefeff4);
        [self.contentView addSubview:self.lineView];
        
```
