---
layout: post
title: Java语言基础11---Scanner、String
comments: true
date: 2016-08-14 09:44:37
tags:
	- Java
---


## <font color=orange>Scanner</font>
一个可以使用正则表达式来解析基本类型和字符串的简单文本扫描器.OC和Swift里面对应的分别是NSScanner和Scanner.

<!--more-->

* System类下有一个静态的字段
	* public static final InputStream in : 标准的键盘输入流.
* 一般方法
	* hasNextXxx() : 判断是否还有下一个输入项, 其中Xxx可以是Int, Double等,如果只是判断下一个是否包含字符串, 可以省略Xxx
	* nextXxx() : 获取下一个输入项. Xxx的含义和上一个方法中的Xxx相同
	* 默认情况下Scanner使用空格、回车等作为分隔符
* 当使用Scanner连续录入整数和字符串的时候, 当输入一个整数后, 键盘上面录入的实际是整数 \r\n, nextLine() 可以接受任意类型, , 只要遇到\r\n就结束了.
	* 解决办法
		1. 使用两个Scannner对象(不推荐)
		2. 都录入成字符串


## <font color=orange>String</font>
String类代表字符串, Java程序中所有的字符串字面量(如"abc")都作为此类的实例.字符串常量存放在方法区里面的常量池.

* 构造方法
	* String() : 空参构造方法
	* String(byte[] bytes) : 把字节数组转出字符串
		* 使用的是GBK码表
		>	byte[] arr = {97,98,99};
String str = new String(arr); //abc
	* String(byte[] bytes,int index, int length) : 把字节数组的一部分转为字符串
	* String(char[] value) : 把字符串数组转成字符串
	* String(char[] value,int index,int length) : 把字符数组的一部分转为字符串
* 字符串的==和equals
	* 字符串常量都是存放在方法区里面的常量池, 如果常量池里面存在这个字符串对象,就直接使用,没有才会创建.
	* == 可以比较基本数据类型也可以比较引用数据类型
		* 基本数据类型的时候比较的是值
		* 引用数据类型的时候比较的是地址值
	* equals
		* String的equals判断的是: 两个字符串的字符序列是否相等.
	* String s3 = "a" + "b" + "c"; //Java有常量优化机制, 编译的时候会使用"abc"
	* String s6 = new String("def");创建2个对象(常量池里面一个和堆内存中的副本, 两个对象的地址值不同)
	* String s9 = s7 + "3" : API里面说 + 是通过StringBuffer或者(StringBuilder)类(字符串缓冲区)和append方法实现, 字符串转换通过toString方法实现, 实际上在堆上面重新开辟了空间.
	* 如果一个常量和一个变量equals, 一般都是把常量写在前面, 这样可以防止空指针异常.


	String s1 = "abc";
	String s2 = "abc";
	System.out.println(s1 == s2);  //true
	System.out.println(s1.equals(s2));//true
		
	String s3 = "a" + "b" + "c"; //Java有常量优化机制, 编译的时候会使用"abc"
	String s4 = "abc";
	System.out.println(s3 == s4); //true
	System.out.println(s3.equals(s4));//true
		
	String s5 = "def";
	String s6 = new String("def");
	System.out.println(s5 == s6); //false
	System.out.println(s5.equals(s6));//true

	String s7 = "12";
	String s8 = "123";
	String s9 = s7 + "3";
	System.out.println(s8 == s9);//false
	System.out.println(s8.equals(s9))//true

* 字符串的判断功能
	* boolean equals(Object obj) : 比较字符串的字符序列是否相同, 区分大小写
	* boolean equalsIgnoreCase(String str) : 判断字符串内容是否相同, 忽略大小写
	* boolean contains(String str) : 判断字符串是否包含其他字符串
	* boolean startsWith(String str) : 判断字符是否以xxx开始
	* boolean endsWith(String str) : 判断字符串是否以xxx结尾
	* boolean isEmpty() : 判断字符串是否为空

* String类的获取index、char方法
	* int length() : 获取字符串的字符个数
		* 数组的length是属性
		>	String s1 = "coppco";
		String s2 = "这里是中文哦!";
		String s3 = "";
		System.out.println(s1.length());//6
		System.out.println(s2.length());//7
		System.out.println(s3.length());//0
	* char charAr(int index) : 获取指定位置的字符
		* 注意不要越界
	* int indexOf(int ch) : 返回指定字符在此字符串中第一次出现的索引
		* 如果字符串中不存在该字符就会返回-1
		* 传入的参数是int, 传入char类型会自动提升至int
		* 参数列表也可以传入fromIndex, 从指定位置开始查找
	* int indexOf(String str) :返回指定字符串在此字符串中第一次出现的索引
		* 在字符串没有找到返回-1
		* 参数列表也可以传入fromIndex, 从指定位置开始查找
	* int lastIndexOf() : 从后往前查找该字符或者字符串在字符串中第一次出现的位置
		* 参数列表也可以传入fromIndex, 从指定位置往前查找
	* String substring(int start) : 从指定位置截取字符串, 默认到末尾
		* 注意方法有返回值
	* String substring(int start, int end) : 截取开始和结尾位置中间的字符串.
		* 包含头不包含尾
		* 注意方法有返回值
* String的转换功能
	* byte[] getBytes() : 把字符串转换为字节数组
		* 通过GBK码表把字符串转为字节数组
		* GBK码表一个中文有两个字节.
		* GBK码表中中文的第一个字节肯定是负数.
	* char[] toCharArray() : 把字符串转为字符数组
	* static String valueOf() : 可以把任意类型转为字符串
		* 当传入的是对象的时候, 调用的是类的toString()方法.
	* String toUpperCase() : 把字符串大写
	* String toLowerCase() : 把字符串小写
	* String concat(String str) : 字符串拼接
		* 和用 + 结果一样, 但是 + 功能更强大
* String类的其他方法
	* String replace(char old, char new) : 使用新的字符替换旧的字符
	* String replace(String old, String new) : 使用新的字符串替换旧的字符串
	* String trim() : 去除字符串两边的空格
	* 字符串的比较
		* int compareTo(String str) : 比较两个字符串
		* int compareToIgnoreCase(String str) : 不分大小写的比较两个字符串.
			* 返回值是字符的Unicode码表值相减