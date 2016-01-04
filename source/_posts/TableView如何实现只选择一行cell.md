title: TableView如何实现只选择一行cell
date: 2014-11-15 23:02:14
tags: [iOS, UITableView, Cell]
categories: iOS
---

相信大家总遇到这样的情况，我们需要写一个tableView，让其实现“选项卡”的功能，即每次只能选择其中一行cell，而且cell前会有一个小对勾，这个用自定义的cell，或者给selectedBackgroundView赋值，也不难实现，但cell有一个叫accessoryType得属性，可以让我们轻松实现以上功能，而且十分轻便。

换不多说，上代码。
<!--more-->

```
- ( UITableViewCellEditingStyle)tableView:( UITableView *)tableView editingStyleForRowAtIndexPath:( NSIndexPath *)indexPath
{
    return UITableViewCellEditingStyleInsert;
}

- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
{
    
    
    NSInteger newRow = indexPath.row;
    NSInteger oldRow = _lastIndexPath.row;
    
    if (newRow != oldRow)
    {
        UITableViewCell *newCell = [tableView cellForRowAtIndexPath:
                                    indexPath];
        newCell.accessoryType = UITableViewCellAccessoryCheckmark;
        
        UITableViewCell *oldCell = [tableView cellForRowAtIndexPath:
                                    _lastIndexPath];
        oldCell.accessoryType = UITableViewCellAccessoryNone;
        
        _lastIndexPath = indexPath;
    }
    
    [tableView deselectRowAtIndexPath:indexPath animated:YES];
    
}
```
## **注意注意**

## *_lastIndexPath一定要先初始化赋值！*
否则可能造成一些小问题。
That'all, thanks for reading.