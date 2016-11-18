---
layout: post
title: Swift里面的一些零碎的知识点
comments: true
date: 2016-11-07 09:32:36
tags:
    - 资料整理
    - Swift
---
这里记录记录一下Swift3里面的一些"边边角角",比较零碎!!!
<!--more-->
**<font color=orange>1. 使用type(of: )来获取类名</font>**
`dynamicType方法已经废弃了,取而代之的是type(of: )方法`
`let a: Int? = 10`
`type(of: 13.14)  //结果Double`
`type(of: a)   //结果Optional<Int>`
PS:如果是<font color=red>NSObject或其子类</font>也可以使用classForCoder, 或者类名.classForCoder()和类名.self
`let view = UIView()`
`view.classForCoder  //结果UIView`
`UIView.self  //结果UIView`
`UIView.classForCoder()  //结果UIView`
</br>
**<font color=orange>2. 使用@discardableResult关键字来忽略方法的返回值</font>**
`某些方法有返回值,但是有时候我们不使用, 如果不定义值来接收它,会有黄色警告⚠️!!!, 这个时候就可以在方法前面添加这个关键字即可.如果不添加这个关键字,也可以使用_来忽略返回值`
`@discardableResult`
`func createButton() -> UIButton{}`
`或者不添加关键字使用_ = createButton()来忽略`
