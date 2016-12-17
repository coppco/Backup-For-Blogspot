---
layout: post
title: Java语言基础10---Java中常见API、Object
comments: true
date: 2016-08-13 17:13:05
tags:
	- Java
---

## API
API(Application Programming Interface) : 应用程序编程接口.Java提供给我们使用的类, 这些类将底层的实现封装了起来.

<!--more-->

## Object
Object是类层次结构的根类, 每个类都使用Object作为超类, 所有对象都实现了这个类的方法.

* 构造方法Object()
	* 子类的构造方法默认访问的是父类的无参构造方法
* int hashCode() : 返回该对象的哈希码值
* Class getClass() : 返回此对象的运行时类.
* String toString() : 返回该对象的字符串表示.
	* getClass().getName() + "@" + Integer.toHexString(hashCode())
	* 类名@对象的hashCode的16进制数
	* 一般子类重写这个方法, 方便输出属性值 使用快捷键 Alt + Shift + s  然后再按s即可自动生成.
	* System.out.println(s);和System.out.println(s.toString());输出的是一样的
* boolean equals() : 判断两个对象是否相等.
	* 比较两个对象内存地址值是否相等
	* 一般用来重写,通过比较属性来判断对象是否相同

### ==和equals的区别
* 共同点
	* 都可以用来比较, 返回值都是boolean
* 区别
	* == 是比较运算符, 可以比较基本数据类型(比较值), 也可以比较引用数据类型(比较的是地址值)
	* equals  只能比较引用类型数据, 没有重写之前,底层依赖的是==, 一般可以重新来根据属性判断是否相等.