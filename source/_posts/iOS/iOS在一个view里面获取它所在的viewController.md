---
layout: post
title: iOS在一个view里面获取它所在的viewController
comments: true
date: 2015-12-12 17:30:22
tags:
    - iOS
    - Swift
    - Objective-C
    - 资料整理
---

开发的时候, 有时候在层级很深的view视图中有一个按钮, 需要跳转页面, 这个时候就需要获取它所在的viewController的NavigationController进行push, 使用block可能需要很多层级, 通知也可以使用, 有点大材小用.我们可以为UIView添加一个Category, 以后使用就方便了.

<!--more-->

>   获取UIView对象所在的控制器,不存在返回nil
@property (nonatomic, strong, readonly)UIViewController \*viewController
-(UIViewController \*)viewController {
&emsp;&emsp;for (UIView \*view = self; view; view = view.superview) {
&emsp;&emsp;&emsp;&emsp;UIResponder \*nextResponder = [view nextResponder];
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;if ([nextResponder isKindOfClass:[UIViewController class]]) {
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;return (UIViewController \*)nextResponder;
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;}
&emsp;&emsp;&emsp;&emsp;}
&emsp;&emsp;return nil;
}

