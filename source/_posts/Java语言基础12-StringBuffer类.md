---
layout: post
title: Java语言基础12---StringBuffer、StringBulider
comments: true
date: 2016-08-15 10:30:07
tags:
	- Java
---


## StringBuffer
StringBuffer继承于Object的final类, 线程安全的可变字符序列, 一个类似于String的字符串缓冲区, 可以通过调用某些方法改变该序列的长度和内容.每个字符串缓冲区都有一定容量, 如果内部缓冲区溢出, 容量会自动增大.

<!--more-->

* String和StringBuffer区别
	* String对象是一个不可变的字符序列
	* StringBuffer是一个可变的字符序列
* StringBuffer构造方法
	* StringBuffer() : 构造一个不带字符的字符串缓冲区, 初始容量为16个字符
	* StringBuffer(CharSequence seq) : 构造一个字符串缓冲区, 它包含与CharSequence指定的字符
	* StringBuffer(int capacity) : 构造一个不带字符, 但是指定初始容量的字符串缓冲区
	* StringBuffer(String str) : 构造一个字符串缓冲区, 并将其内容初始化为指定字符串内容
* capacity()
	* 容器的容量 = length() + 初始容量


	StringBuffer sb = new StringBuffer();
	System.out.println(sb.length());  //0
	System.out.println(sb.capacity());//16

### 常用方法, 操作的都是StringBuffer对象本身
* public StringBuffer append(xxx, xxx) : 把任意类型添加到字符串缓冲区里面
	* 只会创建一个对象, 每次append都会往缓冲区添加字符, 操作的是原字符串缓冲区
* public StringBuffer insert(int index, xxx xxx) : 往指定位置插入任意类型数据
	* 注意会越界
* public StringBuffer deleteCharAt(int index) : 删除指定位置的字符
* public StringBuffer delete(int start, int end) : 删除指定范围中的字符串
	* 包含头不包含尾
* public StringBuffer replace(int start, int end, String str) : 用指定字符串替换范围内的字符串
* public StringBuffer reverse() : 字符串反转

### 字符串截取
* public String substring(int start) : 从指定位置截取字符串, StringBuffer对象本身不会变化, 返回字符串
* public String substring(int start, int end) : 返回字符串, StringBuffer不会变化

### String和StringBuffer之间的转换
* String -----> StringBuffer
	* 构造方法
	>	StringBuffer sb = new StringBuffer("coppco");
	* append方法
	>	StringBuffer sb1 = new StringBuffer();
		sb1.append("coppco");
* StringBuffer -----> String
	* 构造方法
	>	StringBuffer sb = new StringBuffer("coppco");
		String s1 = new String(sb);
	* toString()方法
	>	String s2 = sb.toString();
	* substring()方法
	>	String s3 = sb.substring(0);

## StringBuilder
StringBulider里面的方法和StirngBuffer都一样.

* StringBuilder和StringBuffer的区别
	* StringBuffer是jdk1.0版本的, 线程安全(同步), 效率低
	* StringBulider是jdk1.5版本的, 线程不安全(不同步),效率高
* StringBulider、StringBuffer和String的区别
	* String是一个不可变的字符序列
	* StringBulider、StringBuffer是可变的字符序列

### StringBuilder、StringBuffer和String作为参数传递
* String虽然作为引用数据类型, 但是当做参数传递的时候, 和基本数据类型一样的, 值不会变化
* StringBuilder、StringBuffer作为参数传递的时候会改变