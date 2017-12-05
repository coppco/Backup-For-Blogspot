---
layout: post
title: iOS判断当前控制器是Present还是Push
comments: true
toc: true
date: 2016-10-26 15:03:12
tags:
	- iOS
	- Objective-C
	- Swift
---

有时候同一个控制器可能是Present进来的, 也可能是Push进来的, 这里提供一个方法判断当前控制是Present进来的, 还是Push进来的, 方便返回按钮做一些操作.

<!--more-->

## <font color=orange>使用当前NavigationController来判断</font>
可以根据当前导航栏控制的子控制数量来进行判断: 
```objc
if (!(self.navigationController.viewControllers.count > 1)) {
    //Present进来的
} else {
    //Push进来的
}
```
## <font color=orange>使用presentingViewController属性进行判断</font>
在这之前我们先看看UIViewController的几个属性: 
```objc
/*
  If this view controller is a child of a containing view controller (e.g. a navigation controller or tab bar
  controller,) this is the containing view controller.  Note that as of 5.0 this no longer will return the
  presenting view controller.
*/
@property(nullable,nonatomic,weak,readonly) UIViewController *parentViewController;

// This property has been replaced by presentedViewController.
@property(nullable, nonatomic,readonly) UIViewController *modalViewController NS_DEPRECATED_IOS(2_0, 6_0) __TVOS_PROHIBITED;

// The view controller that was presented by this view controller or its nearest ancestor.
@property(nullable, nonatomic,readonly) UIViewController *presentedViewController  NS_AVAILABLE_IOS(5_0);

// The view controller that presented this view controller (or its farthest ancestor.)
@property(nullable, nonatomic,readonly) UIViewController *presentingViewController NS_AVAILABLE_IOS(5_0);

```
* parentViewController: 显示控制器
* modalViewController: 已经弃用了, 使用presentedViewController替代
* presentedViewController: 当前控制器正在Present显示的控制器, 例如: `[A presentViewController:B animated: true completion: nil];`, 那么A的presentedViewController就是B.
* presentingViewController: 当前控制器被Present的控制器, 例如: `[A presentViewController:B animated: true completion: nil];`, 那么B的presentingViewController就是A.

```objc
if (self.presentingViewController) {
    //判断1
    [self dismissViewControllerAnimated:YES completion:nil];
} else {
    //判断2
    [self.navigationController popViewControllerAnimated:YES];
}
```

