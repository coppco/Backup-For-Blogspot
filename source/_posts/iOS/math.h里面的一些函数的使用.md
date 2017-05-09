---
layout: post
title: math.h里面的一些函数的使用
comments: true
date: 2016-07-18 16:05:27
tags:
    - math
    - iOS
    - Swift
    - Objective-C
---

Objective-C和Swift里面常常有一些数字运算,但是实际结果可能不是我们需要的,这个时候可以使用一些数学函数来处理.今天在群里看到下面一个例子,所以记录下来!
<!--more-->

例如在Swift中:下面代码的结果是<font color=red>`2.399999999999977`</font>而不是`2.4`
```
let max:Double = 651
let min:Double = 648.6
let result = max - min
```

在这里可以借助Objective-C里面的math.h里面定义的函数或者Swift里面Darwin,使用的时候需要导入相应的头文件:
```
#import <math.h>  //Objective-C中导入
import Darwin  //Swift中导入
```

#里面的函数很多,这里只是介绍几个常用的函数
* 三角函数 warning:这里使用的是弧度,而不是角度,使用的时候注意转换
    * double sin (double);正弦 
    * double cos (double);余弦 
    * double tan (double);正切 
* 反三角函数
    * double asin (double); 结果介于[-PI/2, PI/2] 
    * double acos (double); 结果介于[0, PI] 
    * double atan (double); 反正切(主值), 结果介于[-PI/2, PI/2] 
    * double atan2 (double, double); 反正切(整圆值), 结果介于[-PI, PI] 
* 指数与对数
    * double exp (double);求取自然数e的幂 
    * double sqrt (double);开平方 
    * double log (double); 以e为底的对数 
    * double log10 (double);以10为底的对数 
    * double pow(double x, double y）;计算以x为底数的y次幂 
    * float powf(float x, float y); 功能与pow一致，只是输入与输出皆为浮点数 
* 取整
    * double ceil (double); 取上整 ceil(10.3)结果为11
    * double floor (double); 取下整  floor(10.3)结果为10
* 四舍五入
    * double round(double);  如果想保留2位小数可以使用round(number * 100) / 100, 根据小数位改变100值
* 绝对值 
    * double fabs (double);求绝对值 
* 取整与取余 
    * double modf (double, double*); 将参数的整数部分通过指针回传, 返回小数部分 
    * double fmod (double, double); 返回两参数相除的余数 

# 在上例中可以使用<font color=red>`round(result * 1000) / 1000`</font>结果为`2.4`





