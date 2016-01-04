title: UIButton中拖动与点击事件重合的处理方法
categories: iOS
date: 2016-01-04 11:58:29
tags: [iOS, UIButton, 手势]
---

在开发中，我们也许会碰到以上这种需求，如图所示，我们可以随意的拖动“会议中”的button，而且点击button会实现页面跳转。
<!--more-->
![](http://img.blog.csdn.net/20160104120248025)

实现代码十分简单，只需要用到UIButton的两个点击事件即可

```
[_bt_enterConf addTarget:self action:@selector(dragMoving:withEvent:) forControlEvents:UIControlEventTouchDragInside];

[_bt_enterConf addTarget:self action:@selector(enterConf)forControlEvents: UIControlEventTouchUpInside];


#pragma mark 拖动浮动小button(实现了拖动区域的限制)
- (void)dragMoving:(UIControl *)c withEvent:ev
{

    CGPoint point = [[[ev allTouches] anyObject] locationInView:self.window];   
    CGFloat x = point.x;    
    CGFloat y = point.y;
   
    CGFloat btnx = c.frame.size.width / 2.;   
    CGFloat btny = c.frame.size.height / 2.;
      
    if(x <= btnx)        
    {        
        point.x = btnx;        
    }
    
    if(x >= self.window.bounds.size.width - btnx)       
    {
		point.x = self.window.bounds.size.width - btnx;        
    }
    
    if(y <= btny + 20)        
    {      
        point.y = btny + 20        
    }
    
    if(y >= self.window.bounds.size.height - btny - 20)
        
    {       
        point.y = self.window.bounds.size.height - btny - 20;        
    }
    
    c.center = point;  
}

```

但是，这样做后会带来一个问题，即**每次拖动完成后都会触发TouchUpInside**事件，然后进行页面的跳转，而实际上我们并不需要这个跳转，这便是两个事件重合所带来的问题。我尝试的第一种解决方法是定义了一个isDragged的BOOL，当触发了拖动事件后，令`_isDragged = YES`,然后在TouchUpInside绑定的事件最开始加上

```
if (_isDraged == YES) {
            
	_isDraged = NO;
	return;
	          
}
```
这样确实解决了问题，拖动结束后不会进行页面跳转了，然而当我把测试硬件换成了iPhone 6s后，发现手机对拖动事件十分敏感，也就是说，想要实现**点击跳转页面**的功能十分不易，基本上**必会触发拖动事件**，最后我改成了如下代码。

```
[_bt_enterConf addTarget:self action:@selector(dragMoving:withEvent:) forControlEvents:UIControlEventTouchDragInside];
    
UITapGestureRecognizer* singleTap = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(enterConf)];
[self.bt_enterConf addGestureRecognizer:singleTap];
```

终于将问题全部解决，如有更好的方法，欢迎讨论~