---
layout: post
title: Java语言基础03---键盘录入、流程控制语句和方法
comments: true
date: 2016-08-05 16:23:44
tags:
	- Java
---

## <font color=orange>键盘录入</font>
1. 导入包
	* 格式: import java.util.Scanner;
	* 位置: 在class上面
2. 创建键盘录入对象
	* 格式: Scanner sc = new Scanner(System.in);
3. 通过对象获取数据
	* 格式: int x = sc.nextInt();
4. OC和Swift使用UITextField控件实现键盘输入
<!--more-->

## <font color=orange>流程控制语句</font>
* 顺序结构
	* 语句从上自下依次执行
* 选择结构
	* if语句
		* if (表达式) { //code
		* if (表达式) { //code } else { //code }
		* if (表达式1) { //code } else if (表达式2) { //code }
		* if语句使用注意
			* 表达式无论复杂还是简单,结果必须是boolean类型
				* OC中非0即是真,Swift和Java一样
			* 如果语句体里面只有一条语句,大括号可以不写(int x = 10; 其实是两句话,先声明后赋值)
				* OC和Java一致, 表达式括号不能省略
				* Swift大括号已经不能省略, 但是表达式的括号可以省略
			* 一般if语句有左大括号就没有分号,右分号就没有左大括号
	* switch语句
		* 格式
>switch (表达式) {
 case 值1:
	//code1;
 	break;   
 case 值2: 
	//code2;
	break; 
 default:
	//code3; 
	break;
}		
		* OC中表达式可以为基本数据类型, 枚举,字符串
		* Swift中表达式类型比较多, 而且case里面的值还可以添加条件、区间等, break默认会自动添加.不需要手动添加,如果需要向下执行可以添加fallthrough关键字
		* Java中switch的表达式可以接受哪些类型
			* byte、char、short、int
			* long类型不可以
			* JDK1.5之后可以接受枚举
			* JDK1.7之后可以接受字符串
		* case里面的值必须是常量
		* Java中Switch遇到Switch右大括号和break会跳出,否则会继续向下执行语句(和OC一样, Swift中每个case默认会加break)
* 循环结构
	* for 循环语句
>	for (初始表达式; 条件表达式; 循环后的操作表达式) {
	循环体;
	}

		* 条件表达式结果要为boolean类型
		* 循环体如果只有一条语句,大括号可以省略,一般不建议省略
		* 和OC一样, Swift新版本已经移除C语言风格的for循环了
	* while语句
>	while(判断条件语句) {
	循环体;
	控制条件语句;
	}

		* 控制条件语句一定不要忘记了,不然会死循环.
	* do-while语句
>	do {
		循环体;
		控制条件语句;
	}
	 while(判断条件语句)

		* do-while至少会执行一次循环体
		* OC和Java中一致, Swift中为repeat-while


# <font color=red>注意:if、for和while 一定要注意在条件判断语句()的后面不要有;(分号)</font>

###例如:使用for循环打印出乘法口诀

	for (int i = 1;i <= 9 ;i++ ) {
		for (int j = 1;j <= i ;j++ ) {
			System.out.print(j + "*" + i + "=" + i * j + "\t");
		}
		System.out.println();
	}

	* 其中'\t'或者"\t"是制表符, \是转义字符
	* '\t' 制表符
	* '\r' 回车
	* '\n' 换行
	* '\"' "符号
	* '\'' '符号

## <font color=orange>break、continue和return</font>
* break只能在循环语句和switch语句中使用,跳出循环和switch语句
* continue只能在循环中使用,会跳过本次循环,继续下次循环
* return的作用是结束方法的, 返回

## <font color=orange>标号: 可以跳出指定的循环</font>


	//标号: 合法的标识符+:   如 a: 就是标号
	a: for (int i = 1;i <= 10 ;i++ ) {
		b: for (int j = 1;j <= 10 ;j++ ) {
			if (i * j == 8) {
				System.out.print("i = " + i + "\tj = " + j + '\n');
				break a; //跳出a对应的for循环
			}
		}
	}


## <font color=orange>方法</font>

	//Java方法一般格式
	修饰符 返回值类型 方法名(参数类型 参数名1, 参数类型 参数名2...) {
		方法体语句;
		return 返回值;
	}


* 修饰符: public、static等
* 返回值类型:就是功能结果的数据类型
* 方法名: 符合命名规则即可
* 参数
	* 实际参数: 实际参与运算的
	* 形式参数: 方法定义上的,用于接受实际参数
* 参数类型: 就是参数的数据类型
* 参数名: 就是变量名
* return: 结束方法, 返回值为void,可以省略
* 返回值: 返回的结果,返回值为void,可以省略
* Swift方法和Java方法格式类似,只不过参数列表里面的格式是: 参数名1: 参数类型, 参数名2: 参数类型.  和OC方法写法不一样.


	//计算两个数中的最大值
	public static int max(int a, int b) {
		return a > b ? a : b;
	}

### Java中的方法重载: 方法名相同,参数列表不同,与返回值类型无关
* 参数类型不同
* 参数个数不同
	* 顺序不同(很少使用)


	public static int add(int a, int b) {
		return a + b;
	}
	
	public static int add(int a, int b, int c) {
		return a + b + c;
	}
