title: 3DTouch_ShortcutItem
date: 2015-11-13 10:23:15
tags: [3D Touch, iPhone 6s, iOS]
categories: iOS
---
## 3D Touch，新一代 Multi‑Touch。
iPhone 6s 推出了一种可以让你与手机进行互动的全新方式。这一次，iPhone 能够感应你按压屏幕的力度。除了轻点、轻扫、双指开合这些熟悉的 Multi‑Touch 手势之外，3D Touch 还带来 Peek 和 Pop，为 iPhone 的使用体验开拓出全新的维度。而且，当你使用 3D Touch 时，iPhone 将回以轻微的触感，让你不仅能够看到按下屏幕的操作效果，还能感觉得到。

![pic1](http://ww3.sinaimg.cn/large/9c2363adgw1exgwgxs3xrj208j0gyt9t.jpg)
<!-- more -->
### 判断`shortcut`是否可用：

在`ViewController.swift`文件中的`viewDidLoad`方法中添加如下代码，即可判断：

```
// 判断设备是否支持 3D Touch

if self.traitCollection.forceTouchCapability == UIForceTouchCapability.Available {
    print("支持")
} else {
    print("不支持")

}
```

注意，现在真机只有iPhone 6s和iPhone 6s Plus支持3D Touch



### 静态`shortcutItem`添加方法：
![pic2](http://ww3.sinaimg.cn/large/9c2363adgw1exgv0wj3wnj20yl04zq57.jpg)

1. 打开工程的`Info.plist`文件
2. 添加如下键值对内容如下：
plist.Info文件
`UIApplicationShortcutItemIconType` : item的样式
`UIApplicationShortcutItemTitle` : item的标题
`UIApplicationShortcutItemType` : item的唯一标示符

如果想要添加多个选项，直接添加多个字典即可。

运行项目，得到如下效果。

![pic3](http://ww2.sinaimg.cn/large/9c2363adgw1exgw6pohltj20oa0bfmyc.jpg)

我们可用看到效果了，可用点击了，但是没点击反应啊，怎么可以处理点击事件呢，是这样的。看下面：

在`AppDelegate.swift`文件中，添加如下方法：

```
func application(application: UIApplication, performActionForShortcutItem shortcutItem: UIApplicationShortcutItem, completionHandler: (Bool) -> Void) {
}
```
>当有按钮被点击，这个方法就会被执行。那这个方法里面该做什么呢？

判断我们点击了哪个按钮，然后根据按钮做不同的处理：

```
func application(application: UIApplication, performActionForShortcutItem shortcutItem: UIApplicationShortcutItem, completionHandler: (Bool) -> Void) {

    // 根据唯一标示符进行判断
    if shortcutItem.type == "com.lanou3g.3dtouch.setting" {
        // 推出设置控制器
        let settingVC = SettingViewController()
        self.window?.rootViewController?.showDetailViewController(settingVC, sender: nil)
    }
}
```
>注意：判断点击的哪个按钮根据的是type，就是唯一标示符



### 动态添加`shortcutItem`

在`AppDelegate.swift`文件的`application:didFinishLaunchingWithOptions`方法中，添加如下代码，即可动态添加`shortcutItem`:

```
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject:
AnyObject]?) -> Bool {

    // 创建shortcutItem
    let likeShortcutItem = UIApplicationShortcutItem(type: "com.lanou3g.3dtouch.like", localizedTitl  "喜欢", localizedSubtitle: "喜欢点个赞", icon: UIApplicationShortcutIcon(templateImageName: "like"), userInfo: nil)
    let shareShortcutItem = UIApplicationShortcutItem(type: "com.lanou3g.3dtouch.shared", localizedTitle: "分享", localizedSubtitle: nil, icon: UIApplicationShortcutIcon(type: UIApplicationShortcutIconType.Share), userInfo: nil)

    // UIApplicationShortcutItem 代表一个item
    // type： 唯一标示符的属性
    // localizedTitle: 显示的标题
    // localizedSubtitle: 显示的二级标题
    // icon：显示的图片，可以自定义，也可以使用系统提供的样式
    // userInfo: 包含一些信息

    // 把item放到数组中，并赋值给shortcutItems属性
    UIApplication.sharedApplication().shortcutItems = [likeShortcutItem, shareShortcutItem]

    return true
}
```
同样，运行就可以看到效果了

![pic4](http://ww1.sinaimg.cn/large/9c2363adgw1exgw74cvw2j20ni0ekjsw.jpg)


### 总结：

关于ShortcutItem，就如下几个类：
>
UIApplicationShortcutItem  
UIApplicationShortcutIcon: 代表图标的类  
UIApplicationShortcutIconType: 代表图片样式的枚举，有很多，比如：.Add / .Share / .Search / .Message / .Favorite / …
>
后续关于3D Touch内容，后续更新。。