---
layout: post
title: iOS10更改约束后动画调用layoutIfNeeded没有动画
comments: true
date: 2017-04-20 15:58:06
tags:
    - iOS
    - Swift
    - Objective-C
    - 坑点
---

最近做一个动画的时候, 因为用到了约束, 所以在改变约束之后, 需要使用动画来实现, 但是发现在iOS10.2 系统上面没有效果, 但是在iOS9、iOS8上面没有问题. 遂网上查看了一下,  发现在iOS10中, Apple改变了layoutIfNeed(期望不改变view的位置).

<!--more-->

>   //改变位置
[self.imageV mas_remakeConstraints :^(MASConstraintMaker *make) {
&emsp;&emsp;make.centerY.mas_equalTo(self);
&emsp;&emsp;make.centerX.mas_equalTo(self);
&emsp;&emsp;make.width.mas_equalTo(self.mas_width).multipliedBy(0.75);
}];
//旋转
self.imageV.transform = CGAffineTransformRotate(self.imageV.transform, -M_1_PI / 2);
[UIView animateWithDuration:0.35 animations:^{
&emsp;&emsp;[self.imageV layoutIfNeeded];//这一句在iOS10没有动画效果
&emsp;&emsp;self.imageV.transform = CGAffineTransformIdentity;
}];


上面的代码在iOS10以下系统没有问题, 有一个动画效果, 但是在iOS10上面是没有任何效果的.

## <font color=orange>解决方法: 因为在iOS10, Apple改变了layoutIfNeed实现, Apple给出的解决方案是: 调用[superView layoutIfNeeded]</font>

>   [UIView animateWithDuration:0.35 animations:^{
&emsp;&emsp;if([[UIDevice currentDevice].systemVersion floatValue] >= 10.0) {
&emsp;&emsp;&emsp;&emsp;[self.imageV.superview layoutIfNeeded];
&emsp;&emsp;} else {
&emsp;&emsp;&emsp;&emsp;[self.imageV layoutIfNeeded];
&emsp;&emsp;}
&emsp;&emsp;self.imageV.transform = CGAffineTransformIdentity;
}];
