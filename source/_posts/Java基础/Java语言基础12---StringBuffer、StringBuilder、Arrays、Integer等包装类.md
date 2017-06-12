---
layout: post
title: Java语言基础12---StringBuffer、StringBulider、Arrays、Integer等包装类
comments: true
date: 2016-08-20 10:30:07
tags:
	- Java
---


## <font color=orange>StringBuffer</font>
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
* public StringBuffer append(xxx xxx) : 把任意类型添加到字符串缓冲区里面
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

## <font color=orange>StringBuilder</font>
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

### 数组的排序
*	冒泡排序
	* 相邻的两个数相比较,把大数往后放,第一次把最大数放在最后面.


	for (int i = 0; i < arr.length - 1; i++) {
			for (int j = 0; j < arr.length - 1 - i; j++) {
				if (arr[j] > arr[j+1]) {
					int temp = arr[j];
					arr[j] = arr[j+1];
					arr[j+1] = temp;
				}
			}
		}

* 选择排序
	* 用一个索引位置上的元素依次和其他索引位置上面的元素相比较.


	for (int i = 0; i < arr.length - 1; i++) {
			for (int j = i + 1; j < arr.length; j++) {
				if (arr[i] > arr[j]) {
					int temp = arr[i];
					arr[i] = arr[j];
					arr[j]= temp;
				}
			}
		}


## <font color=orange>Arrays数组工具类</font>
* public static String toString(int[] a)
	* 将数组转换为字符串
* public static void sort(int[] a)
	* 数组排序(默认升序)
	* 使用的是快速排序, 效率高
* public static int binarySearch(int[] a, int key)
	* 前提是数组是有序数组,否则数据可能不对
	* 二分查找法查找该数据所在的位置
	* 如果该数据包含在数组中返回索引位置
	* 如果该数据不在数组中,返回  (-(插入点)-1)

## <font color=orange>基本数据类型的包装类</font>
* 将基本数据类型封装成对象的好处在于可以在对象中定义更多的功能丰富操作该数据.
* 常用操作之一: 用于基本数据类型与字符串之间的转换.
* 基本数据类型和对应的包装类

|基本数据类型|对应包装类|
|:---:|:---:|
|byte|Byte|
|short|Short|
|int|Integer|
|long|Long|
|double|Double|
|char|Character|
|boolean|Boolean|

* Integer
	* 提供了多个方法,能处理int和String类型之间的转换,还有其他一些常用常量.
	* 构造方法
		* Integer(int value) 
		* Integer(String str)
			* 如果字符串不是数字字符串,会出现异常
	* int----> String
		* 直接使用字符串 + ""可以转为字符串(推荐使用)
		* public static String valueOf(int i)(推荐使用)
		* int----> Integer ----> String(Integer的toString()方法)
		* 直接使用Integer的toString(int i)静态方法
	* String ----> int
		* 先转为Integer, 然后使用intValue()方法
		* 也可以直接使用Integer的静态方法parseInt(String s)方法
			* 如果字符串s不是数字字符串, 会抛出异常
* JDK5.0开始出现自动装箱和拆箱
	* 注意自动装箱和拆箱的时候, 如果i为null, 会报空指针异常.


	Integer i = 100;
	int b = i + 200;

* 下面的案例
	* Integer
		* equals判断的是int值是否一样
		* == 判断的是内存地址时候一致
		* 使用自动装箱的时候,如果在byte范围(-128~127)的时候那么会从常量池里面取,超过Byte范围才会创建对象(通过源码可以看出来)

	Integer i1 = new Integer(1);
	Integer i2 = new Integer(1);
	System.out.println(i1 == i2);  //false
	System.out.println(i1.equals(i2)); //true

	Integer i3 = 127;
	Integer i4 = 127;
	System.out.println(i3 == i4);  //true
	System.out.println(i3.equals(i4)); //true

	Integer i5 = 128;
	Integer i6 = 128;
	System.out.println(i5 == i6);  //false
	System.out.println(i5.equals(i6)); //true

	
