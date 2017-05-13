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


## <font color=orange size=5>2. 当使用 == 和 != 比较两个对象的时候(尤其是字符串的时候)可能出现判断错误 </font>
这是由于 == 和 != 判断的是地址值,  实际测试的时候明明两个字符串内容一致, 但是使用== 和 != 判断出现了错误,   这个时候可以使用 isEqualToString:方法, 这个方法判断的是字符串里面的内容.

## <font color=orange size=5>3. 当使用NSTimer或者CADisplayLink改变shaper的strokeEnd来做动画和其他动画不同步 </font>

这是由于修改strokeEnd系统默认会有一个动画效果,  如果不移除掉造成动画时间不同步.

>   [layer removeAllAnimations];


