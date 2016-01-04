title: 如何给UITextView增加一个Placeholder
categories: iOS
date: 2014-12-17 22:15:12
tags: [UITextView, Placeholder, iOS]
---

UITextField想必大家都很熟悉，其中有个叫placeholder的属性。那么当我们不得不用到UITextView时，如何给它也添加一个placeholder呢？实现效果如下图。
<!--more-->
![](http://img.blog.csdn.net/20151218110716035)

其实思路也很简单，在textView上增加一个Label就好，但也有一些值得注意的细节，具体流程如下。

- 定义`@property (nonatomic, strong) UILabel *placeHolderLabel;`

- 初始化(label必须设置为不可用)

```
	_placeHolderLabel = [[UILabel alloc] 	
	_placeHolderLabel.frame =CGRectMake(0, 0, 50, 15);
    _placeHolderLabel.text = @"请输入文字...";
    _placeHolderLabel.enabled = NO;//lable必须设置为不可用
    _placeHolderLabel.textColor = UIColorFromRGB(0x999999);
     _placeHolderLabel.font = [UIFont systemFontOfSize:13.0f];
        
	 [self.textView addSubview:self.placeHolderLabel];
        
```

- 在textView的代理方法里实现相应方法，**注意**，此前要将textView的delegate设置为self

```
- (void)textViewDidBeginEditing:(UITextView *)textView
{
    _placeHolderLabel.text = @"";
}

- (void)textViewDidEndEditing:(UITextView *)textView
{
    if (textView.text.length == 0) {
        _placeHolderLabel.text = @"请输入文字...";
    }else{
        _placeHolderLabel.text = @"";
    }
}

```
以上。

