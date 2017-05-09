---
layout: post
title: Java语言基础05---面向对象、封装、构造方法
comments: true
date: 2016-08-08 09:49:20
tags:
	- Java
---

Java和OC、Swift一样都是面向对象的语言,有面向对象的思想还是比较好理解的.面向对象的一般特征:
* 封装(encapsulation)
* 继承(inheritance)
* 多态(polymorphism)

## OC和Swift内存管理机制: 引用计数机制,alloc和retain--->引用计数会加1, release和autoRelease--->引用计数会减1, 当引用计数为0的时候,该对象就会释放.
## Java有垃圾回收机制,当没有任何引用指向该对象,该对象就会变为垃圾,由垃圾回收机制不定时进行回收,而OC和Swift并没有垃圾回收机制.

<!--more-->

理解面向对象里面的类、对象、成员变量、成员方法、静态方法、静态属性等概念.

	//定义一个类Student
	class Student {
		//属性
		int age;
		String name;
		String gender;
		float wight;
		float height;

		public void study() {
			System.out.println(name + "学习了");
		}
	}

	Student zhangSan = new Student();
	zhangSan.name = "张三";
	System.out.println(zhangSan.age);
	zhangSan.study(); //张三学习了

* Student 是一个类
* age、name等是成员变量
	* 调用 : 对象名.成员变量名
* 使用static修饰的变量是静态变量
* zhangSan是实例对象
* study()是成员方法
	* 调用 : 对象名.方法名
* 方法前面有static的是静态方法


### 成员变量和局部变量
* 在类中的位置
	* 成员变量 : 在类中方法外
	* 局部变量 : 在方法中或者方法声明上
* 在内存中的位置
	* 成员变量 : 属于对象,对象在堆内存
	* 局部变量 : 属于方法,方法在栈内存
* 生命周期
	* 成员变量 : 随着对象的创建而存在,对象消失而消失
	* 局部变量 : 随着方法的调用而存在,随着方法的出栈而消失
* 初始化值不同
	* 成员变量 : 有默认初始化值
		* byte、short、int、long默认初始化值是0
		* float、double  默认初始化值是0.0
		* char  默认初始化值是'\u0000'
		* boolean  默认初始化值是false
		* 引用数据类型  默认初始化值是null
	* 局部变量 : 没有默认初始化值,必须定义赋值,才能使用


### 匿名对象
	new Student().study();

* 匿名对象只适合对方法的一次调用,调用完毕就会变为垃圾,多次调用会产生多个对象.
* 匿名对象可以作为参数传递
* 可以提高代码的复用性


### 封装
封装就是将属性私有化,提供公有的方法访问私有属性,在方法内部隐藏实现的细节.
* public   修饰的方法和成员变量,可以在本类和其他类中使用
	* 注意使用public修饰的类的名称必须和Java文件名相同.
	* 错误如下 : 类Student是公共的, 应在名为 Student.java 的文件中声明
* private  修饰的方法和成员变量,只能在本类中使用
	* 可以为成员变量提供set和get方法,以改变或者或者成员变量
* this关键字
	* 在成员方法中,表示引用对象本身
	* 和OC、Swift里面的self类似


	class Person {	
		private int age;

		public void setAge(int age) {
			this.age = age;
		}

		public int getAge() {
			return this.age;
		}
	}


## 构造方法
* 给对象的属性进行初始化, 如果一个类没有构造方法,系统默认会提供一个无参构造方法.
* 构造方法可以重载
	* 如果我们给出带参数的构造方法, 那么无参构造方法系统就不会给出了,需要自己实现.
* 特点
	* 方法名和类名相同(大小写也一样)
	* 没有返回值类型, 连void也没有
	* 没有具体的返回值, 默认是return; 可以省略不写


	class Teacher {
		private String name;

		//默认构造方法
		public Teacher() {
			return;
		}

		//构造方法的重载
		public Teacher(String name) {
			this.name = name;
		}
	}	


## static
* 随着类的加载而加载
* 优先于对象存在
* 静态变量被类的所有对象共有, 存放在方法区的静态区中, 而实例变量放在堆区或者栈区.
* 可以通过类调用,也可以通过对象名调用,推荐使用类名调用
* 一般称为静态方法(类方法),静态变量(类变量), 成员变量也叫实例变量(对象变量),成员方法也称为实例方法.
* 注意事项
	* 在静态方法中不能使用this关键字
	* 静态方法中只能访问静态成员变量和静态成员方法
	* 非静态方法中可以访问成员变量和静态成员变量,成员方法和静态成员方法
	* 如果工具类里面方法都是static, 可以私有化默认构造方法.


### main方法解释
>	public static void main(String[] args) {}

* public : 被jvm调用,所以权限为public
* static : 被jvm调用,不需要创建对象,直接类名调用
* void : 被jvm调用,不需返回值
* String[] args : 字符串数组,以前是用来接收键盘录入的, 现在使用Scanner


### 帮助文档的使用
* 给类、变量和方法添加文档注释, 使用javadoc可以制作文档说明书
* 查看JDK的帮助文档

