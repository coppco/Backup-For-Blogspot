---
layout: post
title: 3D Touch的使用
comments: true
date: 2016-04-07 15:53:09
tags:
    - Objective-C
    - Xcode
---
3D Touch，其原理上是增加了一个压力的感触，通过区分轻按和重按来进行不同的用户交互。它只能在iPhone 6s, iOS 9.0系统以上才能使用.
## <font color=orange>一、Home Screen Quick Action使用</font>
通过主屏幕的应用icon，我们可以用3D Touch呼出一个菜单，进行快速定位应用功能模块相关功能的开发, 目前最多只支持4个.目前有两种方式:

<!--more-->

### <font color=purple>1、静态设置</font>
在info.plist里面添加UIApplicationShortcutItems数组, 数组中对应得item为字典
必填项（下面两个键值是必须设置的）：

UIApplicationShortcutItemType 这个键值设置一个快捷通道类型的字符串 
UIApplicationShortcutItemTitle 这个键值设置标签的标题

选填项（下面这些键值不是必须设置的）：

UIApplicationShortcutItemSubtitle 设置标签的副标题
UIApplicationShortcutItemIconType 设置标签Icon类型
UIApplicationShortcutItemIconFile  设置标签的Icon文件
UIApplicationShortcutItemUserInfo 设置信息字典(用于传值)

### <font color=purple>2、动态设置</font>
在AppDelegate中didFinishLaunchingWithOptions方法中设置3DTouch的桌面的快捷方式按钮（UIApplicationShortcutItem／UIMutableApplicationShortcutItem）, 注意UIApplicationShortcutItem的type是为了识别哪个按钮被点击的.
```
//iOS 9以后才能使用
if ([[UIDevice currentDevice].systemVersion floatValue] >= 9.0) {
    //不存在的时候去添加
    if (application.shortcutItems == nil || application.shortcutItems.count == 0) {
        //UIApplicationShortcutIcon可以是系统的样式, 也可以是自定义图标
        UIApplicationShortcutItem *recharge = [[UIApplicationShortcutItem alloc] initWithType:HJShortcutItemTypeRecharge localizedTitle:@"快速充值" localizedSubtitle:@"" icon:[UIApplicationShortcutIcon iconWithTemplateImageName:@"recharge"] userInfo:nil];

        UIApplicationShortcutItem *withdraw = [[UIApplicationShortcutItem alloc] initWithType:HJShortcutItemTypeWithdraw localizedTitle:@"提现" localizedSubtitle:@"" icon:[UIApplicationShortcutIcon iconWithTemplateImageName:@"withdraw"] userInfo:nil];
        UIApplicationShortcutItem *myRecord = [[UIApplicationShortcutItem alloc] initWithType:HJShortcutItemTypeMyRecord localizedTitle:@"账单" localizedSubtitle:@"" icon:[UIApplicationShortcutIcon iconWithTemplateImageName:@"myRecord"] userInfo:nil];
        application.shortcutItems = @[myRecord, withdraw, recharge];
    }
}
```
在- (void)application:(UIApplication *)application performActionForShortcutItem:(UIApplicationShortcutItem *)shortcutItem completionHandler:(void (^)(BOOL))completionHandler方法中获取到执行的事件. 
```
//App图标的3D Touch事件, willFinishLaunchingWithOptions和didFinishLaunchingWithOptions返回true才会执行
- (void)application:(UIApplication *)application performActionForShortcutItem:(UIApplicationShortcutItem *)shortcutItem completionHandler:(void (^)(BOOL))completionHandler {
    if ([[UIDevice currentDevice].systemVersion floatValue] >= 9.0) {
        if ([shortcutItem.type isEqualToString:HJShortcutItemTypeRecharge]) {//充值
            //code
        } else if ([shortcutItem.type isEqualToString:HJShortcutItemTypeWithdraw]) {
            //code
        } else if ([shortcutItem.type isEqualToString:HJShortcutItemTypeMyRecord]) {
        }
        //如果操作已经完成传入true
        if (completionHandler) {
            completionHandler(true);
        }
    }
}
```
<font color=green>__预览__:</font>
![应用图标3D Touch](http://oak4eha4y.bkt.clouddn.com/3dtouch_icon.png)

## <font color=orange>二、Peek and Pop使用</font>
当在一个控制器上使用3D Touch的时候, 会经历三个步骤
1. 提示用户这里有3D Touch的交互，会使交互控件周围模糊
2. 继续深按，会出现预览视图
3. 通过视图上的交互控件进行进一步交互

1️⃣、 控制器应遵守UIViewControllerPreviewingDelegate协议,并且先判断当前设备是否支持3DTouch操作, 然后进行注册.
```
if (self.traitCollection.forceTouchCapability == UIForceTouchCapabilityAvailable) {//3DTouch可用
    //注册协议, sourceView是监听的那个视图, 可以对cell进行监听
    [self registerForPreviewingWithDelegate:self sourceView:cell];
}
```
2️⃣、实现UIViewControllerPreviewingDelegate的代理方法
2.1 这个方法是返回需要预览的控制器
```
- (nullable UIViewController *)previewingContext:(id <UIViewControllerPreviewing>)previewingContext viewControllerForLocation:(CGPoint)location {
    UIViewController *vc = [[UIViewController alloc] init];
    vc.view.backgroundColor = [UIColor redColor];
    //设置预览控制器大小
    //vc.preferredContentSize = CGSizeMake(200, 200);
    CGFloat h = 50;
    CGFloat y = (self.view.height - h) * 0.5;
    //[previewingContext sourceView]可以拿到对应的cell；
    //NSIndexPath *indexPath = [_tableView indexPathForCell:(UITableViewCell* )[previewingContext sourceView]];
    //设置不被虚化范围
    //previewingContext.sourceRect = sourceRect;
    return vc;
}
```
2.2 继续按压会执行
```
- (void)previewingContext:(id <UIViewControllerPreviewing>)previewingContext commitViewController:(UIViewController *)viewControllerToCommit NS_AVAILABLE_IOS(9_0) {
    [self.navigationController pushViewController:viewControllerToCommit animated:false];
}
```
3️⃣、在弹出的预览控制器实现下面方法, 在向上滑动的时候, 底部会出现选项.
```
- (NSArray<id<UIPreviewActionItem>> *)previewActionItems {
    UIPreviewAction *action1 = [UIPreviewAction actionWithTitle:@"Aciton1" style:UIPreviewActionStyleDefault handler:^(UIPreviewAction * _Nonnull action, UIViewController * _Nonnull previewViewController) {
            NSLog(@"Aciton1");
    }];

    UIPreviewAction *action2 = [UIPreviewAction actionWithTitle:@"Aciton2" style:UIPreviewActionStyleDefault handler:^(UIPreviewAction * _Nonnull action, UIViewController * _Nonnull previewViewController) {
            NSLog(@"Aciton2");
    }];

    UIPreviewAction *action3 = [UIPreviewAction actionWithTitle:@"Aciton3" style:UIPreviewActionStyleDefault handler:^(UIPreviewAction * _Nonnull action, UIViewController * _Nonnull previewViewController) {
            NSLog(@"Aciton3");
    }];
    return @[action1,action2,action3];
}
```
<font color=green>__预览__:</font>
![应用图标3D Touch](http://oak4eha4y.bkt.clouddn.com/peek-pop.png)




