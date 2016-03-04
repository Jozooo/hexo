title: mutableAttributeString
date: 2016-03-04 10:23:15
tags: [mutableAttributeString, iOS]
categories: iOS
---
```
 NSString *text = [NSString stringWithFormat:@"%@ 年化收益",model.rate];
    NSMutableAttributedString *str = [[NSMutableAttributedString alloc] initWithString:text];
    NSRange ran = [text rangeOfString:model.rate];

    NSDictionary *dict = @{NSForegroundColorAttributeName:[UIColor orangeColor],NSFontAttributeName:[UIFont systemFontOfSize:20]};
    [str addAttributes:dict range:ran];
    _shouYiLvLable.attributedText = str;

效果:
   10%(黄色,20号大字体) 年化收益(黑色,12号小字体)

```