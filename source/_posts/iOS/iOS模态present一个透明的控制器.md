---
layout: post
title: iOS模态present一个透明的控制器
comments: true
date: 2016-05-10 16:56:57
tags:
    - iOS
    - Swift
    - Objective-C
    - 资料整理
---

最近做一个项目需要弹出一个上面透明, 下面有内容的视图, 最开始想自定义一个View来实现,  后面想想看能不能使用系统自带的方法来模态显示一个控制器来实现, 结果可以实现.

<!--more-->

>   //使用下面这个方法可以模态显示一个控制器(默认从底部弹出)
-(void)presentViewController:(UIViewController *)viewControllerToPresent animated: (BOOL)flag completion:(void (^ __nullable)(void))completion NS_AVAILABLE_IOS(5_0);

默认情况下, 弹出的视图会遮挡住下面的视图, 如果希望有一个透明效果, 除了要设置模态显示的控制器view的背景颜色为一个alpha不为1的颜色外, 还需要下面的设置.


>   //假如viewController为需要模态显示的控制器
//背景颜色
viewController.view.backgroundColor = [UIColor clearColor];
viewController.definesPresentationContext = true;
viewController.modalPresentationStyle = UIModalPresentationOverCurrentContext;
[self presentViewController:viewController animated:true completion:nil];

# <font color=orange>Warning: 上面的代码在iOS8以后版本运行是可以的, 但是在iOS8以前的版本还是没有透明效果</font>
翻看API发现UIModalPresentationOverCurrentContext是iOS8以后使用的, 在iOS7使用不起效果,  这个时候需要把        [UIApplication sharedApplication].keyWindow.rootViewController.modalPresentationStyle设置为UIModalPresentationCurrentContext.

>   [UIApplication sharedApplication].keyWindow.rootViewController.modalPresentationStyle = UIModalPresentationCurrentContext;
[self presentViewController:gameVC animated:false completion:nil];
[UIApplication sharedApplication].keyWindow.rootViewController.modalPresentationStyle = UIModalPresentationFullScreen; 
