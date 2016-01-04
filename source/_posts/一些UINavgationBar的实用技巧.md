title: 一些UINavgationBar的实用技巧
categories: iOS
date: 2014-12-15 22:36:53
tags: [iOS, UINavgationBar]
---
开发中几乎每个页面都会碰大盘Navgation，那么如何定制它的一些属性来满足不同的页面要求呢，下面就会列举一些。

### 1. Nav背景/Title颜色

```
//set NavigationBar 背景颜色&title 颜色（全局）
    [self.navigationBar setBarTintColor:[UIColor colorWithRed:20/255.0 green:155/255.0 blue:213/255.0 alpha:1.0]];
    [self.navigationBar setTitleTextAttributes:[NSDictionary dictionaryWithObjectsAndKeys:[UIColor whiteColor],UITextAttributeTextColor,nil]];
    
//设置NavigationBar背景颜色（当前）
	[[UINavigationBar appearance] setBarTintColor:[UIColor redColor]];
//@{}代表Dictionary
	[[UINavigationBar appearance] setTitleTextAttributes:@{NSForegroundColorAttributeName:[UIColor whiteColor]}];


```
<!--more-->

### 2. 隐藏navigationbar底部黑线

```
//隐藏navigationbar底部黑线
    if ([self.navigationBar respondsToSelector:@selector( setBackgroundImage:forBarMetrics:)]){
        NSArray *list = self.navigationBar.subviews;
        for (id obj in list) {
            if ([obj isKindOfClass:[UIImageView class]]) {
                UIImageView *imageView=(UIImageView *)obj;
                NSArray *list2=imageView.subviews;
                for (id obj2 in list2) {
                    if ([obj2 isKindOfClass:[UIImageView class]]) {
                        UIImageView *imageView2=(UIImageView *)obj2;
                        imageView2.hidden=YES;
                    }
                }
            }
        }
    }


```

### 3. 自定义返回键

```
// 自定义返回键

	UIButton *backBtn = [UIButton buttonWithType:UIButtonTypeCustom];

	backBtn.frame = CGRectMake(0, 0, 44, 44);

	[backBtn setImage:[UIImage imageNamed:@"back.png"] forState:UIControlStateNormal];

	[backBtn addTarget:self action:@selector(doBack:) forControlEvents:UIControlEventTouchUpInside];
	
	UIBarButtonItem *backItem = [[UIBarButtonItem alloc] initWithCustomView:backBtn];

	self.navigationItem.leftBarButtonItem = backItem;
```

### 4. 再来一个tabbar的,改变tabbar线条颜色

```
//改变tabbar 线条颜色

    CGRect rect = CGRectMake(0, 0, kScreenWidth, 1);

    UIGraphicsBeginImageContext(rect.size);

    CGContextRef context = UIGraphicsGetCurrentContext();

    CGContextSetFillColorWithColor(context, [[UIColor clearColor] CGColor]);

    CGContextFillRect(context, rect);

    UIImage *img = UIGraphicsGetImageFromCurrentImageContext();

    UIGraphicsEndImageContext();

    　[self.tabBar setShadowImage:img];

    　[self.tabBar setBackgroundImage:[[UIImage alloc]init]];

    [self.tabBar setShadowImage:img];

    [self.tabBar setBackgroundImage:[[UIImage alloc]init]];
```