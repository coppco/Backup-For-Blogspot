---
layout: post
title: Java的第一个程序HelloWorld
comments: true
date: 2016-05-04 09:42:28
tags:
	- Java
---

在JDK/bin安装目录下面新建一个文本文件, 命名为HelloWorld.java,  里面写上代码

	class HelloWorld {
		public static void main(String[] args) {// main函数
			System.out.println("我的第一个Java程序") //打印函数
		}
	}

<!--more-->

### 在当前目录打开命令行:Windows 7和8下面按住Shift, 右键点击'在此处点击打开命令窗口'

1. 使用 javac HelloWorld.java 进行编译, 会生成HelloWorld.class
2. 然后使用java HelloWorld 运行,会输出"我的第一个Java程序"



### 书写注意

1. 一般文件名和里面的类名一致
2. 文件扩展名隐藏导致编译失败
3. 关键字注意大小写如 class、String、System、main等
4. 括号需要匹配如{}、()
5. java中的使用的基本都是英文符号(可以设置输入法的:在中文下使用英文符号)

### Java语言的书写格式
* 大括号要对齐,并且成对写
* 左大括号前面有空格
* 遇到左大括号要缩进,Tab
* 方法和程序块之间加空行让程序看起来清晰
* 并排语句之间加空格,例如for语句
* 运算符两侧加空格

### path环境变量的配置和作用
* 如果不配置环境变量只能在jdk/bin目录下书写代码, 不容易管理
* 删除自己的代码容易把JDK文件删除
  </br>
  如像windows下面的notepad 命令,在任意文件夹下运行都会打开记事本,就需要配置path环境变量
#### 配置方式
	* xp系统
		* 右键点击桌面计算机→选择属性→选择高级选项卡→点击环境变量→下方系统变量中查找path→双击path→将jdk安装目录下的bin目录添加到最左边并添加分号。
	* win7/win8系统
		* 右键点击桌面计算机→选择属性→选择高级系统设置→选择高级选项卡→点击环境变量→下方系统变量中查找path→双击path→将jdk安装目录下的bin目录添加到最左边并添加分号。
	* 先配置JAVA_HOME, 在系统变量里面添加变量名为JAVA_HOME, 变量值为D:\Program Files\Java\jdk1.7.0_72的系统变量, 之后在Path里面新增%JAVA_HOME%\bin添加到最左边并添加分号。这样就可以动态获取JAVA_HOME里面的值,这样就可以在自己的代码工作区域书写代码了.(推荐)

### classpath环境变量的配置和作用
* classpath是配置Java的类文件路径, 在任何目录下面运行都是在该目录下面查找class文件, JDK1.5以后版本可以不用配置,默认当前目录
* path配置的是可执行的文件.exe,配置后可以在不同的盘符下访问path路径下的可执行文件


### EditPlus的使用
在EditPlus-----工具----参数设置------工具----用户工具----添加工具----选择应用程序----->分别欠添加javac和java的快捷方式
如图所示: ![说明](http://oak4eha4y.bkt.clouddn.com/EditPlus%E8%AF%B4%E6%98%8E.png)
快捷方式如下: ![快捷方式](http://oak4eha4y.bkt.clouddn.com/EditPlus%E5%BF%AB%E6%8D%B7%E6%96%B9%E5%BC%8F.png)

#### EditPlus模板的设置
1. 在EditPlus-----工具----参数设置-----文件----模板----Java----template.java载入修改为Java的格式
2. 在EditPlus-----工具----参数设置-----文件----设置&语法----Java----自动完成----java.acp加载
3. 如果改起来麻烦可以找已经改好的文件直接覆盖进去
4. 每次保存的时候会创建bak文件, 不想要可以在在EditPlus-----工具----参数设置-----文件----保存时创建备份文件----勾去掉----应用即可

