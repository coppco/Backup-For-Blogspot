---
layout: post
title: Java语言基础07---多态、abstract、接口
comments: true
date: 2016-08-13 11:08:48
tags:
	- Java
---

## <font color=orange>多态</font>
多态(polymorphism)是面向对象的三大特性之一,是面向对象思想的核心.多态是具有表现多种形态能力的特征,同一个实现接口使用不同的实例而执行不同的操作.
<!--more-->

* 父类可以引用子类实例


	class Person {
		int age = 10;

		public void desc() {
			System.out.println("Person");
		}
		
		public static void method() {
			System.out.println("Person static");
		}
	} 

	class Teacher extends Person {
		int age = 20;

		public void desc() {
			System.out.println("Teacher");
		}		

		public static void method() {
			System.out.println("Teacher static");
		}
	}

	public static void main(String[] args) {
		Person p = Teacher();
		System.out.println(p.age); //①10
		p.desc();  //②Teacher
		p.method(); //③Person static
	}


* 成员变量, 编译看左边,运行看左边①
* 成员方法, 编译看左边,运行看右边②
* 静态方法, 编译看左边,运行看左边③

### 向上转型和向下转型
* 基本数据类型
	* 自动类型转换, 小范围-->大范围
	* 强制类型转换


	int i = 10;
	byte b = 20;
	
	//自动类型转换
	i = b;
	//强制类型转换
	b = (byte)i;

* 引用数据类型
	* 向上转型, 父类引用指向子类对象
		* Person p = Teacher();
		* 不能使用子类的特有属性和方法
	* 向下转型, 把父类对象转换为子类对象
		* Teacher t = (Teacher)p;


### 多态的好处

* 可以减少编码的工作量
* 提高程序的可维护性和扩展性
	* 子类重写父类的方法
	* 如将父类作为参数类型, 那么父类和子类作为参数传入
	* 可以根据实际的类型,决定使用哪个方法


### instanceof关键字
判断前边的引用对象是否是后边的数据类型
	
	Person p = Tearcher();

	//判断对象p是不是Teacher类型
	if (p instanceof Teacher) {
		Teacher t = (Teacher)p;
	} else {
		System.out.println("Other");
	}


## 抽象类和抽象方法
* 抽象类和抽象方法必须使用abstract关键字修饰
* 抽象类不一定有抽象方法(可以没有),有抽象方法的类一定是抽象类或者接口
* 抽象类不能实例化, 可以由具体子类实例化.
* 抽象类的子类
	* 要么是抽象类
	* 要么重写抽象类中的所有抽象方法

抽象类特点:

* 成员变量, 可以是变量或者常量, abstract不能修饰成员变量
* 成员方法可以是抽象的,也可以是非抽象的
	* 使用abstr修饰的方法是抽象方法, 强制要求子类做的事
		* 抽象方法没有方法体
	* 非抽象方法, 子类继承的事情,提高代码复用性
* 有构造方法
* 不能和static、final、private等关键字共存

## 接口(和OC、Swift里面的协议很像)
一个Java接口是一些方法特征的集合,但是没有方法的实现.Java中的接口中定义的方法在不同的地方被实现,可以具有完全不同的行为.

* 使用关键字interface定义接口
	

	interface 接口名 {
	}

* 类实现接口使用implements


	class 类名 implements 接口名 {
	}

* 接口不能实例化
* 可以通过多态来实例化,通过子类来实例化
	* 接口的子类可以是抽象类
	* 接口的子类也可以是非抽象类, 要重写接口中的所有抽象方法.
* 接口中的变量, 实际中是常量, 系统会自动添加上public static final关键字.一般接口是存放常量最好的地方.
* 接口没有构造方法
* 接口里面的方法只能是抽象方法
* 接口可以多继承, 类可以实现多个接口
