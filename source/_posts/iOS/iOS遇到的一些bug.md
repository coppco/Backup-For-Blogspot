---
layout: post
title: iOS遇到的一些bug
comments: true
date: 2016-04-07 18:21:14
tags:
    - 坑点
    - 资料整理
    - iOS
    - Objective-C
    - Swift
---
这里记录开发的时候遇到的一些BUG.
<!--more-->

## <font color=orange size=5>1. iOS7.0版本报错 Assertion failure in -[XXX layoutSublayersOfLayer:], /SourceCache/UIKit/UIKit-2935.138/UIView. </font>

这是由于重写了view的layoutSubviews方法, 但是里面没有添加[super layoutSubviews]语句, 在iOS8以后版本没有问题, 但是在iOS8.0以下版本就报错.

## <font color=orange size=5>2. 当使用NSTimer或者CADisplayLink改变shaper的strokeEnd来做动画和其他动画不同步 </font>

这是由于修改strokeEnd系统默认会有一个动画效果,  如果不移除掉造成动画时间不同步.

>   [layer removeAllAnimations];




