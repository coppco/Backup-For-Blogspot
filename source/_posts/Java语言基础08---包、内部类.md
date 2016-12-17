---
layout: post
title: Java语言基础08---包、内部类
comments: true
date: 2016-08-12 11:57:37
tags:
	- Java
---

## <font color=orange>包</font>
将.class进行分类存放, 包实际就是文件夹

* 定义包的格式
	* package 包名;
	* 多级包用.分开即可
		* package com.github.Java;

<!--more-->
* 注意事项
	* package语句必须是程序的第一条可执行语句
	* package语句在一个Java文件中只有一个
	* 如果没有package,默认表示无包名
	* 带包的java文件编译命令 javac -d . Hello.java
	* 带包的java文件运行命令 java 包名.Hello
* 包与包之间的访问
	* 不同包里的无关类,能被访问的必须使用public
	* 在当前包文件中要使用其他包里的类
		* 使用全路径   包名.类名
		* 使用import关键字导入
			* import java.util.Scanner; //导入一个类(常用)
			* import java.util.*; //导入包里面的所有类

* package、import、class的顺序

### 修饰符 
* 权限修饰符
	* private
	* 默认
	* protected
	* public

权限如下:(Y---可以, N---不可)

||本类|同一个包下(子类和无关类)|不同包(子类)|不同包(无关类)|
|:---:|:---:|:---:|:---:|:---:|
|private|Y|N|N|N|
|默认|Y|Y|N|N|
|protected|Y|Y|Y|N|
|public|Y|Y|Y|Y|

* 状态修饰符
	* static 
	* final
* 抽象修饰符
	* abstract

## <font color=orange>内部类</font>
* 内部类

	* 内部类可以直接访问外部类的成员,包括私有的
		* 外部类名.this.成员名
	* 外部类要访问内部类,必须创建对象
	* 外部类.内部类 对象名 = 外部类对象.内部类对象;


	class Outer {
		class Inner {
			public void method() {
				System.out.println("method");
			}
		}
	}
	Outer.Inner oi = new Outer().new Inner();
	oi.method();

* 私有内部类
	* 只能在类的内部访问


	class Outer {
		private class Inner {
			public void method() {
				System.out.println("method");
			}
		}
	}

* 静态内部类


	class Outer {
		static class Inner {
			public void method() {
				System.out.println("method");
			}
		}
	}
	Outer.Inner oi = new Outer.Inner();
	oi.method();
	
* 局部内部类访问局部变量
	* 局部内部类只能访问声明为final的变量
	* JDK1.8之后取消了,可以访问非final修饰的变量
* 匿名内部类 : 内部类的简化写法
	* 本质, 是一个继承了该类或者实现了该接口的子类匿名对象
	* 如果匿名内部类重写了多个方法,可以使用父类引用指向子类对象, 但是如果匿名内部类有自己独有的方法,这种方法就不能使用了,因为匿名内部类没有类名,无法强转类型.一般匿名内部类适用于只重写一个方法时使用.
	* 匿名内部类生成的class名字, 是外部类名$编号.class, 如Outer$1.class
	* 格式


	new 类名或者接口名() {
		//重写方法
	}

* 在Outer里面补全代码,输出Hello world!


	public static void main(String[] args) {
		//在Outer里面补齐代码, 要求输出Hello world!
		Outer.method().show();
	}
	//接口
	interface Inter {
		void show();
	}

	class Outer {
		//使用匿名内部类
		public static Inter method() {
			return new Inter() {
				public void show() {
					System.out.println("Hello world!");
				}
			};
		}
	}