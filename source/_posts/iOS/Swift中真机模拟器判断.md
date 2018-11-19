---
layout: post
title: Swift中真机模拟器判断
comments: true
toc: true
date: 2018-04-16 10:47:33
tags:
	- iOS
	- Swift
	- Objective-C
---

在开发中经常会碰到一些库没有模拟器架构(i386,x86_64), 所以导致无法模拟器运行, 造成开发工作中相当不便。

<!--more-->

### <font color=orange>Objective-C中判断真机模拟器</font>

在Objective-C中, `TargetConditionals.h`中定义了宏`TARGET_OS_SIMULATOR`, 在模拟器SDK中值是1, 而真机中值是0.

所以, 我们可以Objective-C中使用下面的代码进行条件编译

> #if TARGET_IPHONE_SIMULATOR
//模拟器
#define IS_SIMULATOR true
#else
//真机
#define IS_SIMULATOR false
#endif


### <font color=orange>Swift中判断真机模拟器</font>
在Swift中`TARGET_IPHONE_SIMULATOR`已经弃用了, 如果使用下面的代码去判断会失效.

> #if !(TARGET_IPHONE_SIMULATOR)
//真机
#endif


实测, 下面的宏是有效的

> #if !(arch(i386) || arch(x86_64))
//真机
#else
//模拟器
#endif


<font color=red>需要注意的是</font>: 
> 如果Swift项目中需要使用Objective-C代码, 那么在Objective-C代码和桥接文件中还是需要使用宏`TARGET_IPHONE_SIMULATOR`进行判断