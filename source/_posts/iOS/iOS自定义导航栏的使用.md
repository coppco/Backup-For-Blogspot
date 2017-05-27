---
layout: post
title: iOS自定义导航栏的使用
comments: true
date: 2016-04-23 16:12:58
tags:
    - iOS
    - Objective-C
    - Swift
    - 资料整理
---

在开发App过程中, 一般整个App的导航栏大致风格都是一致, 这个时候一般有两种方式来处理:
* 使用系统自带的UINavigationContoller, 然后定义一个base的UIViewController作为基类, 设置好它的导航栏样式.后面的所有Controller都继承于它.
* 自定义导航栏控制器继承于UINavigationController, 在自定义导航栏里面设置.
对于第二种方式来设置, 会导致系统自带的侧滑返回手势失效.

<!--more-->

# <font color=orange>1. 统一设置导航栏</font>

>   //initialize方法在该类第一次使用时只会运行一次, 在这里统一配置
+(void)initialize {
&emsp;&emsp;UINavigationBar *bar = [UINavigationBar appearance];
&emsp;&emsp;//设置背景颜色
&emsp;&emsp;//bar.barTintColor = [UIColor colorWithRed:248 / 255.0 green:79 / 255.0 blue:83 / 255.0 alpha:1];
&emsp;&emsp;//背景图片
&emsp;&emsp;[bar setBackgroundImage:kImage(@"navigation_backgroundImage") forBarMetrics:(UIBarMetricsDefault)];
&emsp;&emsp;//去掉全局导航条下面黑线
&emsp;&emsp;[bar setShadowImage:[UIImage new]];
&emsp;&emsp;//设置标题字体属性
&emsp;&emsp;bar.titleTextAttributes = @{NSForegroundColorAttributeName: [UIColor colorFromRGBValue:0xFFFFFF], 
    NSFontAttributeName : kPingFangLightFont(18)};
&emsp;&emsp;//设置导航栏是否透明, 如果为true, 那么controller里面的布局frame的x从状态栏开始, 否则从导航栏下面开始
&emsp;&emsp;if (__iOS_VERSION > 7.0) {
&emsp;&emsp;&emsp;&emsp;bar.translucent = false;
&emsp;&emsp;}
}

# <font color=orange>2. 解决导航栏侧滑返回失效</font>
* 设置代理

>   //解决侧滑失效问题
-(void)viewDidLoad {
&emsp;&emsp;[super viewDidLoad];
//设置代理
&emsp;&emsp;self.interactivePopGestureRecognizer.delegate = self;
}

* 遵循协议

>   @interface HJNavigationController ()<UIGestureRecognizerDelegate>

* 实现代理协议
>   -(BOOL)gestureRecognizerShouldBegin:(UIGestureRecognizer *)gestureRecognizer {
&emsp;&emsp;return self.viewControllers.count > 1;
}

# <font color=orange>3. 对后续页面进行处理,例如当push一个UIViewController的时候, 希望leftBarButtonItem一样, 可以在这里处理.</font>

>   -(void)pushViewController:(UIViewController \*)viewController animated:(BOOL)animated {
&emsp;&emsp;if (self.viewControllers.count > 0) {
&emsp;&emsp;&emsp;&emsp;//隐藏tabBar
&emsp;&emsp;&emsp;&emsp;viewController.hidesBottomBarWhenPushed = YES;
&emsp;&emsp;&emsp;&emsp;//设置左边按钮
&emsp;&emsp;&emsp;&emsp;viewController.navigationItem.leftBarButtonItem = [UIBarButtonItem barButtonItemWithTitle:nil titleNormalColor:nil titleHighlightedColor:nil normalImage:@"back_normal" highlightedImage:@"back_press" target:self action:@selector(back:) edg:(UIEdgeInsetsMake(0, 0, 0, 0))];
&emsp;&emsp;}
&emsp;&emsp;[super pushViewController:viewController animated:animated];
}
//实现返回方法
-(void)back:(UIButton *)button {
&emsp;&emsp;[self popViewControllerAnimated:true];
}

# <font color=orange>4. 使指定页面的控制器不能侧滑返回</font>
有时产品会希望一个流程完全走完, 而不要中间中断.除了需要对导航栏左侧按钮处理外还需要处理侧滑问题.在指定控制器里面实现下面方法即可.

>   -(void)viewDidAppear:(BOOL)animated{  
&emsp;&emsp;if ([self.navigationController respondsToSelector:@selector(interactivePopGestureRecognizer)]) {  
&emsp;&emsp;&emsp;&emsp;self.navigationController.interactivePopGestureRecognizer.enabled = NO;  
&emsp;&emsp;}  
}  
-(void)viewWillDisappear:(BOOL)animated{  
&emsp;&emsp;self.navigationController.interactivePopGestureRecognizer.enabled = YES;  
}
