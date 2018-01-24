---
layout: post
title: Java语言基础06---代码块、继承
comments: true
date: 2016-05-12 11:34:22
tags:
	- Java
---


## <font color=orange>代码块</font>
在Java中使用{}括起来的代码称为代码块,根据位置和申明的不同可以分为:局部代码块、静态代码块、构造代码块和同步代码块.
<!--more-->

* 局部代码块(不常用)
	* 在方法中出现,限定变量生命周期,及早释放,提高内存利用率


	
	public static void main(String[] args) {
		System.out.println("Hello world");
		
		//局部代码块,出了局部{}之后内部变量会释放
		{
			int x = 1;
			System.out.println(x);
		}
	}

* 构造代码块(不常用)
	* 在类中方法外使用,多个构造方法中相同的代码放在一起,每次调用构造方法都会执行,并且在构造方法前执行.


	class Student {
		//私有化成员变量
		private int age;
		private String name;
		
		//默认构造方法
		public Student() {
		}

		//重载构造方法
		public Student(int age, String name) {
			this.age = age;
			this.name = name;
		}

		//set和get
		public void setAge(int age) {
			this.age = age;
		}
		public int getAge() {
			return this.age;
		}

		public void setName(String name) {
			this.name = name;
		}
		
		public String getName() {
			return this.name;
		}
		
		//构造代码块, 每次调用构造方法都会执行,并且在构造方法之前执行
		{
			System.out.println("构造代码块");
		}
	}


* 静态代码块
	* 在类中使用, 使用static修饰
	* 用于给类进行初始化, 在加载类的时候执行,并且只会执行一次
	* 一般用于加载驱动
	* 静态代码块在类加载进内存时执行,肯定先于构造代码块, 父类的静态代码块先于子类的静态代码块执行.


	//静态代码块
	static {
		System.out.println("静态代码块");
	}

* 同步代码块


## <font color=orange>继承</font>
将相同的属性和方法抽取成父类,不同的属性和方法继承于父类.<font color=red>Java中只支持单继承, 使用关键字extends.</font>

	//父类
	class Animal {
		String color;
		int leg;
		
		public void eat() {
			System.ou.println("eat");
		}
	}

	//子类
	class Cat extends Animal {
		//
	}

	class Dog extends Aniaml {
		//
	}
好处:

* 可以提高代码的复用性
* 提高了代码的维护性
* 让类与类之间产生了关系,是多态的前提
 
弊端:

* 类的耦合性增强了

注意事项:

* Java中所有类都直接或者间接继承了Object类, 定义一个类的时候没有使用extends关键字,那么这个类直接继承于Object类
* 子类只能继承父类所有非私有的成员(方法和变量)
* 子类不能继承父类的构造方法,但是可以通过super来访问父类的构造方法
* 不要为了部分功能而去继承
* 子类中所有构造方法默认都会访问父类中的默认空参构造方法: super(); 如果父类没有默认构造方法,那么可以在子类构造方法中①使用super访问父类有参构造方法super("213",12)②或者使用this访问子类其他的构造方法
	* this或者super必须是构造方法的第一条语句.
* 子类可以重写父类的方法

### super和this
* super代表当前对象的父类引用
* this代表当前对象的引用

### 重写
子类可以重写父类的方法,返回值类型可以是子父类,如果子类需要父类方法的功能,可以使用super调用父类方法. 
* 父类私有方法不能被重写
* 子类重写父类方法时,权限不能更低,一般权限都是一样.

### override(重写)和overload(重载)
* overload : 重载,可以改变返回值类型,参数列表不同,方法名一样(或者是子父类)
* override : 重写,子类中和父类方法声明一样的方法,返回值类型一致或者子父类.

### final关键字
* 修饰类,类不能被继承, 如String类
* 修饰变量
	* 修饰基本数据类型, 就是常量了,只能赋值一次,不能改变,一般会和public static一起使用
	* 修饰引用数据类型, 不能指向新的引用,但是对象的属性可以修改.
	* 修饰成员变量的初始化时机:
		* 显示初始化值
		* 在构造方法里面赋值 
* 修饰方法, 方法不能被重写.
