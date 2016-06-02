title: JZShare - ShareSDK如何自定义分享UI
categories: iOS
date: 2016-05-27 10:22:01
tags: [ShareSDK, 自定义UI, 分享， iOS]
---


> 首先附上源码：[Jozo的Github -- JZShare](https://github.com/chenzht/JZShare)

当我们用ShareSDK集成分享的功能，难免会遇到产品提来的需求，他会找设计自行设计出一套UI，然后指着那美轮美奂的效果告诉你，做成这样。

![](http://7xusmm.com1.z0.glb.clouddn.com/mdJozo/1464428529645.png)


<!--more-->
那么，怎样抛弃Mob原生的UI，来自行定义UI呢，其实也很简单。SDK里本来就有摒弃UI的调用分享的方法。本文的一切也将围绕它展开。

> 本文使用Mob版本为[ShareSDK For iOS v3.3.0](http://www.mob.com/#/downloadDetail/ShareSDK/ios)

```
/**
 *  分享内容
 *
 *  @param platformType             平台类型
 *  @param parameters               分享参数
 *  @param stateChangeHandler       状态变更回调处理
 */
+ (void)share:(SSDKPlatformType)platformType
   parameters:(NSMutableDictionary *)parameters
onStateChanged:(SSDKShareStateChangedHandler)stateChangedHandler;
```

#### 下面开始正式的代码实现：

首先自定义一个类 "ShareCustom"

**ShareCustom.h**

```
#import <Foundation/Foundation.h>
@interface ShareCustom : NSObject

+(void)shareWithContent:(id)publishContent;//自定义分享界面
```

**ShareCustom.m**

```
// 适配
#define DevicesScale ([UIScreen mainScreen].bounds.size.height==480?1.00:[UIScreen mainScreen].bounds.size.height==568?1.00:[UIScreen mainScreen].bounds.size.height==667?1.17:1.29)

// 颜色
#define UIColorFromRGB(rgbValue) [UIColor  colorWithRed:((float)((rgbValue & 0xFF0000) >> 16))/255.0  green:((float)((rgbValue & 0xFF00) >> 8))/255.0  blue:((float)(rgbValue & 0xFF))/255.0 alpha:1.0]

// 获取屏幕尺寸
#define kScreenHeight ([UIScreen mainScreen].bounds.size.height)
#define kScreenWidth ([UIScreen mainScreen].bounds.size.width)

// 设备类型
#define SYSTEM_VERSION   [[UIDevice currentDevice].systemVersion floatValue]

//屏幕宽度相对iPhone6屏幕宽度的比例
#define KWidth_Scale    [UIScreen mainScreen].bounds.size.width/375.0f


#import "JZShareCustom.h"
/**
 @author Jozo, 16-05-24 12:05:09
 
 导入ShareSDK分享功能
 */
#import <ShareSDK/ShareSDK.h>
#import <ShareSDKUI/ShareSDK+SSUI.h>
#import <ShareSDKConnector/ShareSDKConnector.h>

//腾讯开放平台（对应QQ和QQ空间）SDK头文件
#import <TencentOpenAPI/TencentOAuth.h>
#import <TencentOpenAPI/QQApiInterface.h>

//微信SDK头文件
#import "WXApi.h"


@implementation JZShareCustom

static id _publishContent;//类方法中的全局变量这样用（类型前面加static）
static UIVisualEffectView *_effectView;
/*
 自定义的分享类，使用的是类方法，其他地方只要 构造分享内容publishContent就行了
 */

+(void)shareWithContent:(id)publishContent/*只需要在分享按钮事件中 构建好分享内容publishContent传过来就好了*/
{
    _publishContent = publishContent;
    UIWindow *window = [UIApplication sharedApplication].keyWindow;
    
    
    UIBlurEffect * blur = [UIBlurEffect effectWithStyle:UIBlurEffectStyleLight];
    _effectView = [[UIVisualEffectView alloc]initWithEffect:blur];
    _effectView.frame = CGRectMake(0, 0, kScreenWidth, kScreenHeight);
    [window addSubview:_effectView];
    
    
    /**
     点击退出手势
     */
    UITapGestureRecognizer *tap = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(dismiss)];
    [_effectView addGestureRecognizer:tap];
    
    
    /**
     加上一层黑色透明效果
     */
    UIView *blackV = [[UIView alloc] initWithFrame:CGRectMake(0, 0, kScreenWidth, kScreenHeight)];
    blackV.backgroundColor = [UIColor blackColor];
    blackV.alpha = 0.2;
    [_effectView addSubview:blackV];
    
    
    /**
     Share Content
     */
    UIView *shareView = [[UIView alloc] initWithFrame:CGRectMake((kScreenWidth-220 * DevicesScale)/2.0f, (kScreenHeight-249 * DevicesScale)/2.0f,  220 * DevicesScale, 249*DevicesScale)];
    shareView.tag = 441;
    [window addSubview:shareView];
    
    UIImageView *shareImg = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"分享_bg"]];
    shareImg.frame = CGRectMake(0, 0, shareView.frame.size.width, shareView.frame.size.height);
    [shareView addSubview:shareImg];
    
    UILabel *titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(0, 0, shareView.frame.size.width, 45*KWidth_Scale)];
    titleLabel.text = @"分享给小伙伴";
    titleLabel.textAlignment = NSTextAlignmentCenter;
    titleLabel.font = [UIFont systemFontOfSize:18*KWidth_Scale];
    titleLabel.textColor = UIColorFromRGB(0x374552);
    titleLabel.backgroundColor = [UIColor clearColor];
    [shareView addSubview:titleLabel];
    
    NSArray *btnImages = @[@"分享_qq", @"分享_微信好友", @"分享_QQ空间", @"分享_朋友圈"];
    NSArray *btnTitles = @[ @"QQ", @"微信好友", @"QQ空间", @"朋友圈",];
    for (NSInteger i=0; i<btnImages.count; i++) {
        
        CGFloat bt_width =  85 * DevicesScale;
        if(kScreenHeight == 568) {
            bt_width = 95 *DevicesScale;
        }
        
        CGFloat gap = (shareView.frame.size.width - 2*bt_width) / 3.0;
        CGFloat top = 0.0f;
        if (i<2) {
            top = 0*KWidth_Scale;
            
        }else{
            top = 0*KWidth_Scale + bt_width;
        }
        
        
        UIButton *button = [[UIButton alloc] initWithFrame:CGRectMake(gap * (i % 2 + 1) + i % 2 * bt_width, titleLabel.frame.origin.y + titleLabel.frame.size.height+top, bt_width, bt_width)];
        [button setImage:[UIImage imageNamed:btnImages[i]] forState:UIControlStateNormal];
        
        UILabel *lbl = [[UILabel alloc] initWithFrame:CGRectMake(0, bt_width - 20, bt_width, 20)];
        lbl.font = [UIFont systemFontOfSize:12*DevicesScale];
        lbl.textAlignment = NSTextAlignmentCenter;
        lbl.textColor = UIColorFromRGB(0x374552);
        lbl.text = btnTitles[i];
        [button addSubview:lbl];
        
        
        button.tag = 331+i;
        [button addTarget:self action:@selector(shareBtnClick:) forControlEvents:UIControlEventTouchUpInside];
        [shareView addSubview:button];
    }
    
    UIButton *cancleBtn = [[UIButton alloc] initWithFrame:CGRectMake((kScreenWidth - 24 *DevicesScale) / 2.0f, shareView.frame.origin.y + shareView.frame.size.height + 8, 24*DevicesScale, 24*DevicesScale)];
    [cancleBtn setBackgroundImage:[UIImage imageNamed:@"分享_关闭"] forState:UIControlStateNormal];
    cancleBtn.tag = 339;
    [cancleBtn addTarget:self action:@selector(shareBtnClick:) forControlEvents:UIControlEventTouchUpInside];
    cancleBtn.alpha = 1.0;
    [_effectView.contentView addSubview:cancleBtn];
    
    //为了弹窗不那么生硬，这里加了个简单的动画
    shareView.transform = CGAffineTransformMakeScale(1/300.0f, 1/270.0f);
    _effectView.contentView.alpha = 0;
    [UIView animateWithDuration:0.35f animations:^{
        shareView.transform = CGAffineTransformMakeScale(1, 1);
        _effectView.contentView.alpha = 1;
    } completion:^(BOOL finished) {
        
    }];
}

+(void)shareBtnClick:(UIButton *)btn
{
    
    int shareType = 0;
    id publishContent = _publishContent;
    if (btn.tag == 339) {
        
        [self dismiss];
        return;
    }
    switch (btn.tag) {
        case 331:
        {
            shareType = SSDKPlatformSubTypeQQFriend;
        }
            break;
            
        case 332:
        {
            shareType = SSDKPlatformSubTypeWechatSession;
        }
            break;
            
        case 333:
        {
            shareType = SSDKPlatformSubTypeQZone;
        }
            break;
            
        case 334:
        {
            shareType = SSDKPlatformSubTypeWechatTimeline;
        }
            break;
            
        default:
            break;
    }
    
    /*
     调用shareSDK的无UI分享类型，
     */
    
    [ShareSDK share:shareType parameters:publishContent onStateChanged:^(SSDKResponseState state, NSDictionary *userData, SSDKContentEntity *contentEntity, NSError *error) {
        
        
        switch (state) {
            case SSDKResponseStateSuccess:
            {
                
                NSLog(@"分享成功！");
                break;
            }
            case SSDKResponseStateFail:
            {
                
                NSLog(@"分享失败！");
                break;
            }
            default:
                break;
        }
        
        
        
    }];
    
}

+ (void)dismiss {
    UIWindow *window = [UIApplication sharedApplication].keyWindow;
    UIView *shareView = [window viewWithTag:441];
    
    //为了弹窗不那么生硬，这里加了个简单的动画
    shareView.transform = CGAffineTransformMakeScale(1, 1);
    [UIView animateWithDuration:0.35f animations:^{
        shareView.transform = CGAffineTransformMakeScale(1/300.0f, 1/270.0f);
        _effectView.contentView.alpha = 0;
    } completion:^(BOOL finished) {
        
        [shareView removeFromSuperview];
        [_effectView removeFromSuperview];
    }];
}


@end
```

#### 可以直接拷贝以上代码，然后飘红的地方稍加修改即可

然后在需要调用分享的地方调用以下方法就可以自定义分享UI啦，当然你也可以在文章上方的源码Demo里自己体会，十分简单

```
#pragma mark - Private Method
- (void)goShare {
    
    //1、创建分享参数
    NSArray* imageArray = @[[UIImage imageNamed:@"share_icon"]];
    if (imageArray) {
        
        NSMutableDictionary *shareParams = [NSMutableDictionary dictionary];
        [shareParams SSDKSetupShareParamsByText:@"IM/音视频/呼叫中心/电话会议等通讯能力全方位展示，实力派，很任性。"
                                         images:imageArray
                                            url:[NSURL URLWithString:@"http://113.31.88.170:8081/cfct-server/demo/demo.html?to=UC_DEMO"]
                                          title:@"云之讯UC"
                                           type:SSDKContentTypeAuto];
        

        //调用自定义分享
        [ShareCustom shareWithContent:shareParams];
    }
    
}

```








That's all, thank you.