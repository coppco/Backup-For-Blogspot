---
layout: post
title: iOS适配iOS7遇到的坑
comments: true
date: 2016-01-10 17:37:02
tags:
    - 坑点
    - 资料整理
    - iOS
    - Objective-C
    - Swift
---
最近的项目需要适配iOS7, 这里记录一下遇到的坑点.

<!--more-->

## <font color=orange size=5>1. iOS7.0版本报错 Assertion failure in -[XXX layoutSublayersOfLayer:], /SourceCache/UIKit/UIKit-2935.138/UIView. </font>
这是由于重写了view的layoutSubviews方法, 但是里面没有添加[super layoutSubviews]语句, 在iOS8以后版本没有问题, 但是在iOS8.0以下版本就报错.

## <font color=orange size=5>2. iOS8.0以前使用字符串的`-(BOOL)containsString:(NSString *)str NS_AVAILABLE(10_10, 8_0)`方法 </font>
导致某些判断无效,甚至崩溃, 这是因为这个方法是iOS8.0才能使用的方法, 可以新建一个NSString的Category, 在里面重写这个方法.

>   -(BOOL)containsString:(NSString *)str {
&emsp;&emsp;if (str.length == 0) {
&emsp;&emsp;&emsp;&emsp;return false;
&emsp;&emsp;}
&emsp;&emsp;if ([self rangeOfString:str].location != NSNotFound) {
&emsp;&emsp;&emsp;&emsp;return true;
&emsp;&emsp;}
&emsp;&emsp;return false;
}

## <font color=orange size=5>3. 当使用 == 和 != 比较两个对象的时候(尤其是字符串的时候)可能出现判断错误 </font>
这是由于 == 和 != 判断的是地址值,  实际测试的时候明明两个字符串内容一致, 但是使用== 和 != 判断出现了错误,   这个时候可以使用 isEqualToString:方法, 这个方法判断的是字符串里面的内容.同理对于对象应该判断具体的属性(曾经比较两个NSIndexPath出错了, 后来比较具体的row和section才可以).
