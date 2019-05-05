---
layout: post
title: Swift中使用runtime进行字典转模型时的一个注意点
comments: true
date: 2016-08-02 11:09:26
tags:
    - Swift
    - 坑点
    - 杂谈
    - iOS
---


最近使用runtime实现Swift中字典转模型的时候,使用下面获取属性列表的方法无法获取声明为Bool类型的属性, 导致不能正确赋值.
```
var count: UInt32 = 0
let properties = class_copyPropertyList(className, &count) //属性列表
```   

<!--more-->

# 总结
* <font color=black size=4>纯Swift类型无法使用runtime运行时, 可以在属性、函数前面添加__@objc__, 它看可以把纯Swift的API导出给Objecttive-C使用</font>
* <font color=black size=4>继承于NSObject的类,默认都是添加了__@objc__,可以使用OC中runtime来获取属性列表</font>
* <font color=black size=4>加了__@objc__标识的方法、属性无法保证都会被运行时调用，因为Swift会做静态优化。要想完全被动态调用，必须使用__dynamic__修饰。使用dynamic修饰将会隐式的加上@objc标识</font>
* <font color=red size=4>若方法的参数、属性类型为Swift特有(如Character、Tuple、Bool)、无法映射到Objective-C的类型，则此函数、属性无法添加__dynamic__修饰或__@objc__（会编译错误）</font>

# 后记 最近看到一个不是使用KVC的字典转模型的一个三方框架HandyJSON,有兴趣的可以去看看.[传送门](https://github.com/alibaba/HandyJSON)
