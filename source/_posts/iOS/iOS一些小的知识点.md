---
layout: post
title: iOS一些小的知识点
comments: true
date: 2016-03-29 10:26:50
tags:
    - iOS
    - Swift
    - Objective-C
    - 资料整理
---
这里只是记录一些常用的一些小的知识点, 后面遇到会一直更新.
<!--more-->
# <font color=orange>1. 拷贝文本到剪切板</font>

>   UIPasteboard *pasteboard = [UIPasteboard generalPasteboard];
    pasteboard.string = @"拷贝的信息";

# <font color=orange>2. 自定义NavigationController侧滑返回手势无法使用</font>

>   //需要设置代理, 遵循协议UIGestureRecognizerDelegate
self.interactivePopGestureRecognizer.delegate = self;
    //实现方法
-(BOOL)gestureRecognizerShouldBegin:(UIGestureRecognizer *)gestureRecognizer {
&nbsp;&nbsp;return self.viewControllers.count > 1;
}

# <font color=orange>3. 特定控制器不能侧滑返回</font>
有时产品会希望一个流程完全走完, 而不要中间中断.除了需要对导航栏左侧按钮处理还需要处理侧滑问题.在指定控制器里面实现下面方法即可.

>   -(void)viewDidAppear:(BOOL)animated{  
&nbsp;&nbsp;if ([self.navigationController respondsToSelector:@selector(interactivePopGestureRecognizer)]) {  
&nbsp;&nbsp;&nbsp;&nbsp;self.navigationController.interactivePopGestureRecognizer.enabled = NO;  
&nbsp;&nbsp;}  
}  
-(void)viewWillDisappear:(BOOL)animated{  
&nbsp;&nbsp;self.navigationController.interactivePopGestureRecognizer.enabled = YES;  
}

# <font color=orange>4. 判断当前ViewController是push还是present方式显示的</font>

>   if (self.presentingViewController) {
&nbsp;&nbsp;[self dismissViewControllerAnimated:YES completion:nil];
} else {
&nbsp;&nbsp;[self.navigationController popViewControllerAnimated:YES];
}

# <font color=orange>5. 获取view所在的控制器viewController</font>
开发的时候, 有时候在层级很深的view视图中有一个按钮, 需要跳转页面, 这个时候就需要获取它所在的viewController的NavigationController进行push, 使用block可能需要很多层级, 通知也可以使用, 有点大材小用.我们可以为UIView添加一个Category, 以后使用就方便了.

>   获取UIView对象所在的控制器,不存在返回nil
@property (nonatomic, strong, readonly)UIViewController \*viewController
-(UIViewController \*)viewController {
&nbsp;&nbsp;for (UIView \*view = self; view; view = view.superview) {
&nbsp;&nbsp;&nbsp;&nbsp;UIResponder \*nextResponder = [view nextResponder];
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if ([nextResponder isKindOfClass:[UIViewController class]]) {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return (UIViewController \*)nextResponder;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;}
&nbsp;&nbsp;return nil;
}

# <font color=orange>6. 使用Masonry和SnapKit的注意事项</font>
* 首先需要先添加到父视图, 不然会崩溃
* 两个视图相互添加约束的时候需要注意它们要有共同的大容器
* 使用Masonry和SnapKit要做动画的时候
    * 首先更改约束(mas_update或者mas_remake)
    * 然后调用[view layoutIfNeeded];
    >   [UIView animateWithDuration:0.25 delay:0.0f options:UIViewAnimationOptionCurveEaseInOut animations:^{
&nbsp;&nbsp;&nbsp;&nbsp;[view layoutIfNeeded];
}];

# <font color=orange>7. iOS 模态显示一个控制器, 希望这个控制器是透明的</font>

>   //使用下面这个方法可以模态显示一个控制器(默认从底部弹出)
-(void)presentViewController:(UIViewController *)viewControllerToPresent animated: (BOOL)flag completion:(void (^ __nullable)(void))completion NS_AVAILABLE_IOS(5_0);

默认情况下, 弹出的视图会遮挡住下面的视图, 如果希望有一个透明效果, 除了要设置模态显示的控制器view的背景颜色为一个alpha不为1的颜色外, 还需要下面的设置.

>   //假如viewController为需要模态显示的控制器
viewController.view.backgroundColor = [UIColor clearColor];
viewController.definesPresentationContext = true;
viewController.modalPresentationStyle = UIModalPresentationOverCurrentContext;
[self presentViewController:viewController animated:true completion:nil];
