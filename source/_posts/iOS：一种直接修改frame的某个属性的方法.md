title: iOS：一种直接修改frame的某个属性的方法
date: 2015-11-13 17:22:19
tags: [CGRect, Frame, UI, iOS]
categories: iOS
---
在iOS中，view的frame属性使用地十分频繁，特别是调整UI。而正常情况下我们无法对frame的某个属性(如x,y,width,height等)进行单独修改，比如：

```
someView.frame.x = 100；	// This is a wrong code;
```

这种方式是不允许的，但实际上我们更经常遇到的是frame的大部分元素值保持不变，只改变其中的一部分。相信这个烦恼困扰了不少人，于是我们不得不用以下两种方法去达到目的：

```
法1：
CGRect frame = someView.frame;
frame.x =100;
frame.width = 200;
someView.frame = frame;
 
法2：
someView.frame = CGRectMake(100, XXX, 200, XXX);
```

法2看起来也很精简，但实际上也很麻烦，因为实际应用场景中x, y, width, height四个值都是依赖别的变量，导致法2的语句非常长。简而言之，以上方法都不够“优雅”。那怎样才算优雅呢？我觉得如果我们能如下这样直接修改某个值就完美了：
<!--more-->
```
someView.x = 100;
someView.width = 200;
```

我们跳过someView的frame属性，直接修改了我们想要的元素值。幸运的是，我们使用category可以相当方便地达到目的，这是一件一劳永逸的事情，引入一次category后整个工程都可以使用这种修改方法

```
//
//  UIView+Convenience.h
//  UC_Demo_1.0.0
//
//  Created by Barry on 15/5/11.
//  Copyright (c) 2015年 Barry. All rights reserved.
//

#import <UIKit/UIKit.h>

@interface UIView (Convenience)

@property (nonatomic) CGPoint frameOrigin;
@property (nonatomic) CGSize frameSize;

@property (nonatomic) CGFloat frameX;
@property (nonatomic) CGFloat frameY;

@property (nonatomic) CGFloat frameRight;
@property (nonatomic) CGFloat frameBottom;

@property (nonatomic) CGFloat frameWidth;
@property (nonatomic) CGFloat frameHeight;

- (BOOL)containsSubView:(UIView *)subView;
- (void)removeAllSubViews;

@end
```

```
//
//  UIView+Convenience.m
//  UC_Demo_1.0.0
//
//  Created by Barry on 15/5/11.
//  Copyright (c) 2015年 Barry. All rights reserved.
//

#import "UIView+convenience.h"
#import <QuartzCore/QuartzCore.h>

@implementation UIView (Convenience)

- (BOOL)containsSubView:(UIView *)subView
{
    for (UIView *view in [self subviews]) {
        if ([view isEqual:subView]) {
            return YES;
        }
    }
    return NO;
}

- (void)removeAllSubViews {
    for (UIView *view in [self subviews]) {
        [view removeFromSuperview];
    }
}

- (CGPoint)frameOrigin {
    return self.frame.origin;
}

- (void)setFrameOrigin:(CGPoint)newOrigin {
    self.frame = CGRectMake(newOrigin.x, newOrigin.y, self.frame.size.width, self.frame.size.height);
}

- (CGSize)frameSize {
    return self.frame.size;
}

- (void)setFrameSize:(CGSize)newSize {
    self.frame = CGRectMake(self.frame.origin.x, self.frame.origin.y,
                            newSize.width, newSize.height);
}

- (CGFloat)frameX {
    return self.frame.origin.x;
}

- (void)setFrameX:(CGFloat)newX {
    self.frame = CGRectMake(newX, self.frame.origin.y,
                            self.frame.size.width, self.frame.size.height);
}

- (CGFloat)frameY {
    return self.frame.origin.y;
}

- (void)setFrameY:(CGFloat)newY {
    self.frame = CGRectMake(self.frame.origin.x, newY,
                            self.frame.size.width, self.frame.size.height);
}

- (CGFloat)frameRight {
    return self.frame.origin.x + self.frame.size.width;
}

- (void)setFrameRight:(CGFloat)newRight {
    self.frame = CGRectMake(newRight - self.frame.size.width, self.frame.origin.y,
                            self.frame.size.width, self.frame.size.height);
}

- (CGFloat)frameBottom {
    return self.frame.origin.y + self.frame.size.height;
}

- (void)setFrameBottom:(CGFloat)newBottom {
    self.frame = CGRectMake(self.frame.origin.x, newBottom - self.frame.size.height,
                            self.frame.size.width, self.frame.size.height);
}

- (CGFloat)frameWidth {
    return self.frame.size.width;
}

- (void)setFrameWidth:(CGFloat)newWidth {
    self.frame = CGRectMake(self.frame.origin.x, self.frame.origin.y,
                            newWidth, self.frame.size.height);
}

- (CGFloat)frameHeight {
    return self.frame.size.height;
}

- (void)setFrameHeight:(CGFloat)newHeight {
    self.frame = CGRectMake(self.frame.origin.x, self.frame.origin.y,
                            self.frame.size.width, newHeight);
}

@end
```
策略很简单，希望对大家有所帮助。