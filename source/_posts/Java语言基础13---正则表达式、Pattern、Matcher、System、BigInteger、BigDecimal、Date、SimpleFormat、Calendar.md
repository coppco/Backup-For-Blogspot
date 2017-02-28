---
layout: post
title: Java语言基础13---正则表达式、Pattern、Matcher、System、BigInteger、BigDecimal、Date、SimpleFormat、Calendar
comments: true
date: 2016-08-18 11:37:07
tags:
	- Java
---

## <font color=orange>正则表达式</font>

正则表达式: 是指一个用来描述或者匹配一系列符合某个语法规则的字符串的单个字符串, 其实是一种规则.

<!--more-->
* java.util.regex.Pattern类
	* 边界匹配器 
		* ^ 行的开头 
		* $ 行的结尾 
		* \b 单词边界 
		* \B 非单词边界 
		* \A 输入的开头 
		* \G 上一个匹配的结尾 
		* \Z 输入的结尾，仅用于最后的结束符（如果有的话） 
		* \z 输入的结尾 
	* 字符类
		* []  表示单个字符串
			* [abc]  a、b或者c
			* [^abc] : 任意字符除了a、b或c
			* [a-zA-Z]  a到z或A到Z
			* [a-d[m-p]]  a到d或m到p
			* [a-z&&[def]]  d、e和f(交集)
			* [a-z&&[^bc]]  a到z除了b、c(交集)
			* [a-z&&[^m-p]]  a到z,并且非m到p(交集)
	* 预定义字符类
		* Java中表示\表示转义, \\\\实际才表示\\
		* .  任意字符
		* \d  数字: [0-9]
		* \D  非数字: [^0-9]
		* \s  空白字符: [\t\n\x0B\f\r]
		* \S  非空白字符: [^\s]
		* \w  单词字符: [a-zA-Z_0-9]
		* \W  非单词字符: [^\w]
	* 数量词
		* ?  一次或者一次也没有
		* \*  零次或者多次
		* \+  一次或者多次
		* {n}  正好n次
		* {n,}  至少n次
		* {n,m}  n到m次
	
	* 组合捕获
		* 捕获组可以通过从左到右计算其开始来编号.
		* 如(.)\\1(.)\\2+  \\1表示匹配的表达式1又出现异常, \\2表示匹配的表达式2至少出现一次, 而且还可以在后面的还可以获取这个()里面的字符如: $1、$2等
* 字符串匹配正则表达式, boolean matches(String regex)


	System.out.println("".matches("[abc]?"));//true
	System.out.println("d".matches("[abc]?"));//false

* 字符串的分割方法, String[] split(String regex)
	

	String[] arr = "哈哈.呵呵.嘿嘿.啊啊".split("\\."); //.表示任意字符,如果要使用点(.)分割,需要转义
	for (int i = 0; i < arr.length; i++) {
		System.out.println(arr[i]);
	}


* 字符串的替换方法, 使用后面的字符串替换前面匹配的字符串, String replaceAll(String regex)


	String s1 = "这里是11co2ppco1的博客!";
	String s2 = s1.replaceAll("\\d", "");
	System.out.println(s2);

	//组的使用
	String s3 = "我...我.我.....我...是是...是....是...个..个.个....个...结..结....巴....巴..巴";
	String s4 = s3.replaceAll("\\.", "");
	System.out.println(s4);
	String s5 = s4.replaceAll("(.)\\1+", "$1");//一个字符连续出现多次就使用这个字符替换
	System.out.println(s5);

## <font color=orange>Pattern和Matcher类</font>
* Pattern  正则表达式的编译表示形式
* Matcher  匹配器


	Pattern p = Pattern.compile("a*b");
	Matcher m = p.matcher("aaaaab");
	boolean b = m.matches();
	//上面代码等价于
	boolean b = Pattern.matches("a*b", "aaaaab");
	
	//当需要从字符串里面获取匹配的字符串时,这样很方便
	String s1 = "我的名字是coppco, 在github上面使用hexo + node.js搭建了一个博客.";
	Pattern pattern = Pattern.compile("[a-zA-Z]+\\.?[a-zA-Z]+");
	Matcher matcher = pattern.matcher(s1);
		
	while (matcher.find()) {
		System.out.println(matcher.group());
	}

## <font color=orange>Math类</font>
里面封装了常用的数学方法.
* 常用常量
	* E
		* Math.E
	* PI
		* Math.PI
* 常用方法
	* 绝对值
		* abs(double/float/int/long value)
	* 向上取最小的整数值, 但是返回结果为double类型
		* double ceil(double a)
	* 向下去最大的整数值, 但是返回值为doulbe类型
		* double floor(double a)
	* 获取两个值中的最大值
		* max(a, b)
	* 获取两个值中的最小值
		* min(a, b)
	* 指数
		* pow(底数, 指数)
	* 随机数0.0~1.0之间的随机小数(不包括1.0)
		* random()
	* 四舍五入
		* int round(double/float a)
	* 求平方根
		* sqrt

## <font color=orange>Random</font>
此类的实例用于生成伪随机数, 使用相同的种子创建Random实例, 那么得到的随机数字序列相同.
* 构造方法
	* Random()
		* 种子不固定
	* Random(long seed)
		* 种子固定, 每次运行都会是一样的结果
* 常用方法
	* nextInt()  返回int范围内的随机数
	* nextInt(int n)  返回0--n-1范围内的随机数
	* nextFloat(), nextDouble(), nextLong)等


## <font color=orange>System类</font>
System 类包含一些有用的类字段和方法。它不能被实例化. 在 System 类提供的设施中，有标准输入、标准输出和错误输出流；对外部定义的属性和环境变量的访问；加载文件和库的方法；还有快速复制数组的一部分的实用方法。 

* 静态属性
	* err  标准错误输出流
	* in   标准输入流
	* out  标准输出流
* 常用静态方法
	* gc()  运行垃圾回收器
		* 会调用对象的finalize()方法
	* exit(int status)  退出Java虚拟机
		* status作为状态码, 非0表示异常
	* currentTimeNillis()  返回现在距离1970年1月1日之间的毫秒值
	* arraycopy(Object src, int srcPos, Object dest, int destPos, int length) 数数组拷贝
		* src  源数组
		* srcPos  源数组位置
		* dest  目标数组
		* destPos  目标数组位置
		* length  拷贝的数量

## <font color=orange>BigInteger</font>
不可变的任意精度的整数.主要用于一些比较大的数字之间的加减乘除运算.
* add		加
* subtract	减
* multiply	乘
* divide	除
* divideAnRemainder	求余

## <font color=orange>BigDecimal</font>
不可变的、任意精度的有符号十进制数。在计算机中使用二进制表示数, 而对于浮点数有可能（注意是可能）是不准确的，它只能无限接近准确值，而不能完全精确。
	
	System.out.println(2.0-1.1); //0.8999999999999999

	//可以使有BigDecimal来解决
	BigDecimal bd1 = new BigDecimal(2.0);
	BigDecimal bd2 = new BigDecimal(1.1);
	//0.899999999999999911182158029987476766109466552734375只是精度高了,但是不能解决根本问题
	System.out.println(bd1.subtract(bd2));
	//推荐
	BigDecimal bd3 = new BigDecimal("2.0");
	BigDecimal bd4 = new BigDecimal("1.1");
	System.out.println(bd3.subtract(bd4)); //0.9
	//推荐
	BigDecimal bd5 = BigDecimal.valueOf(2.0);
	BigDecimal bd6 = BigDecimal.valueOf(1.1);
	System.out.println(bd5.subtract(bd6));//0.9

## <font color=orange>Date类</font>
* 一个在sql包下面
* 一个在util包下面
	* 构造方法
		* Date()  当前时间
		* Date(long date)  传入一个和1970年1月1日间隔的时间(毫秒值)
	* getTime()  获取时间距离1970年1月1日的间隔毫秒值
	* setTime(long time)
		* 设置距离1970年1月1日的间隔毫秒值
	* 好多方法已经过期, 一般使用Calendar类

## <font color=orange>DateFormat和SimpleDateFormat</font>
日期格式化类, 可以把Date和字符串之间进行格式转换.
* DateFormat是抽象类, 不能实例化, 一般使用SimpleDateFormat类
* 或者使用DateFormat df1 = DateFormat.getDateInstance();获取子类对象
* 日期和时间模式

|字母|日期或时间元素|表示|示例|
|:---:|:---:|:---:|:---:|
|G|Era 标志符|Text|AD|
|y|年|Year|1996; 96|  
|M|年中的月份|Month|  July; Jul; 07|  
|w|  年中的周数|  Number|  27|  
|W|  月份中的周数|  Number|  2|  
|D|  年中的天数|  Number|  189|  
|d|  月份中的天数|  Number|  10|  
|F|  月份中的星期|  Number|  2|  
|E|  星期中的天数|  Text|  Tuesday; Tue|  
|a|  Am/pm 标记|  Text|  PM|  
|H|  一天中的小时数（0-23）|  Number|  0|  
|k|  一天中的小时数（1-24）|  Number|  24|  
|K|  am/pm 中的小时数（0-11）|  Number|  0|  
|h|  am/pm 中的小时数（1-12）|  Number|  12|  
|m|  小时中的分钟数|  Number|  30|  
|s|  分钟中的秒数|  Number|  55|  
|S|  毫秒数|  Number|  978|  
|z|  时区|  General time zone|  Pacific Standard Time; PST; GMT-08:00|  
|Z|  时区|  RFC 822 time zone|  -0800|  
* format(Date date)
	* 把日期对象格式化成字符串
* parse(String str)
	* 把字符串格式化为日期格式


	SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
	//将日期Date转为字符串
	String dateStr = format.format(new Date());
	//将字符串转为Date
	Date date = format.parse("2015-09-09 12:22:55");
		
## <font color=orange>Calendar和GregorianCalendar</font>
* Calendar是一个抽象日历类, 使用Calendar rightNow = Calendar.getInstance();可以获取子类对象GregorianCalendar实例
* 常用方法
	* int get(int field)  返回给定日历字段的值
		* <font color=red>注意month是从0开始的, 所以获取月份需要结果1</font>
	* add(int field, int amount)  为给定的日历字段添加或减去指定的时间量。
	* void set(int field, int value)  将给定的日历字段设置为给定值。 


	Calendar calendar = Calendar.getInstance();
	int day = calendar.get(Calendar.DAY_OF_MONTH);
	System.out.println(day);
	calendar.add(Calendar.YEAR, 1);//年增加1
	System.out.println(calendar.get(Calendar.YEAR));//获取年
	calendar.set(Calendar.YEAR, 2019);//设置年
	calendar.set(2020, 6, 1);//设置2019年7月1日
	System.out.println(calendar.get(Calendar.YEAR));