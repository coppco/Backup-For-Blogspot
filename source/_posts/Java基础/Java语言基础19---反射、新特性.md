---
layout: post
title: Java语言基础19---反射、新特性
comments: true
date: 2016-09-18 09:37:51
tags:
	- Java
---


当程序要使用某个类时, 如果该类还没有加载到内存, 则系统会通过加装、连接、初始化三步类实现对这个类进行初始化.
* 加载
	* 将class文件读入内存, 并创建一个class对象
* 连接
	* 验证: 是否有正确的内存结构
	* 准备: 为类的静态成员变量分配内存并默认初始化值
	* 解析: 将类的二进制数据中的符号引用替换为直接引用
* 初始化, 字节码文件加装,默认初始化, 显示初始化, 构造方法初始化

* 加载时机
	* 创建类的实例
	* 访问类的静态变量或者静态方法
	* 使用反射方式创建类或者接口对应的class对象
	* 初始化某个类的子类
	* 直接使用java.exe命令运行某个类
<!--more-->

## <font color=orange>反射</font>
* 反射概述
	* JAVA反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；
	* 对于任意一个对象，都能够调用它的任意一个方法和属性；
	* 这种动态获取的信息以及动态调用对象的方法的功能称为java语言的反射机制。
	* 要想解剖一个类,必须先要获取到该类的字节码文件对象。
	* 而解剖使用的就是Class类中的方法，所以先要获取到每一个字节码文件对应的Class类型的对象。

* 三种方式
	* Object类的getClass()方法,判断两个对象是否是同一个字节码文件
		* 示例
		>	Person p = new Person();
			Class clazz = p.getClass();
	* 静态属性class,锁对象
		* 示例
		>	Class clazz = Person.class;
	* Class类中静态方法forName(),读取配置文件
		* 示例
			>	Properties map = new Properties();
			map.load(new FileInputStream("config.ini"));
			Class clazz = Class.forName(map.getProperty("class"));
			//调用该类的无参数构造方法创建对象
			Person p = (Person)clazz.newInstance();
			p.setName("张三");
			System.out.println(p.getName());


## Class
Class 类的实例表示正在运行的 Java 应用程序中的类和接口。枚举是一种类，注释是一种接口。每个数组属于被映射为 Class 对象的一个类，所有具有相同元素类型和维数的数组都共享该 Class 对象。基本的 Java 类型（boolean、byte、char、short、int、long、float 和 double）和关键字 void 也表示为 Class 对象。 
* 常用方法
	* 静态方法通过类名字符串(包含包名)获取Class对象
		* static Class<?> forName(String className) 根据类名的字符串(包含包名)来创建Class对象 
		* static Class<?> forName(String name, boolean initialize, ClassLoader loader) 使用给定的类加载器，返回与带有给定字符串名的类或接口相关联的 Class 对象。
	* 使用无参构造方法创建对象 
		* T newInstance() 调用无参构造方法创建实例对象。 
	* 获取public有参构造方法
		* Constructor<T> getDeclaredConstructor(Class<?>... parameterTypes) 返回一个 Constructor 对象，该对象反映此 Class 对象所表示的类或接口的指定构造方法。 
		* Constructor<?>[] getDeclaredConstructors() 返回 Constructor 对象的一个数组，这些对象反映此 Class 对象表示的类声明的所有构造方法。
			* 通过Constructor的 T newInstance(Object... initargs) 方法创建对象
	* 获取private有参构造方法
		* Constructor<T> getDeclaredConstructor(Class<?>... parameterTypes)  返回一个 Constructor 对象，该对象反映此 Class 对象所表示的类或接口的指定构造方法。 
		* Constructor<?>[] getDeclaredConstructors() 返回 Constructor 对象的一个数组，这些对象反映此 Class 对象表示的类声明的所有构造方法。 
	* 获取public成员变量
		* Field getField(String name) 返回一个 Field 对象，它反映此 Class 对象所表示的类或接口的指定公共成员字段。 
 		* Field[] getFields() 返回一个包含某些 Field 对象的数组，这些对象反映此 Class 对象所表示的类或接口的所有可访问公共字段。 
	* 获取private成员变量
		* Field getDeclaredField(String name) 返回一个 Field 对象，该对象反映此 Class 对象所表示的类或接口的指定已声明字段。 
		* Field[] getDeclaredFields()  返回 Field 对象的一个数组，这些对象反映此 Class 对象所表示的类或接口所声明的所有字段。 
		* 私有成员变量, 修改值需要设置对应Field对象的accessible为true
			* void setAccessible(boolean flag) 将此对象的 accessible 标志设置为指示的布尔值。 
	* 获取public方法
		* Method getMethod(String name, Class<?>... parameterTypes) 返回一个 Method 对象，它反映此 Class 对象所表示的类或接口的指定公共成员方法。 
		* Method[] getMethods() 返回一个包含某些 Method 对象的数组，这些对象反映此 Class 对象所表示的类或接口（包括那些由该类或接口声明的以及从超类和超接口继承的那些的类或接口）的公共 member 方法。 
		* 泛型擦除(泛型反射)
		>	//泛型只在编译的时候有效, 运行期泛型会被擦除
		ArrayList<String> list = new ArrayList<>();
		list.add("a");
		list.add("b");
		//list.add(123); //字符串集合中直接添加int数出错, 可以使用反射来添加
		Method m = list.getClass().getMethod("add", Object.class);
		m.invoke(list, 123);
		System.out.println(list); //a, b, 123
	* 获取private方法
		* Method getDeclaredMethod(String name, Class<?>... parameterTypes) 返回一个 Method 对象，该对象反映此 Class 对象所表示的类或接口的指定已声明方法。 
		* Method[] getDeclaredMethods() 返回 Method 对象的一个数组，这些对象反映此 Class 对象表示的类或接口声明的所有方法，包括公共、保护、默认（包）访问和私有方法，但不包括继承的方法。 

### 动态代理
在Java中java.lang.reflect包下提供了一个Proxy类和一个InvocationHandler接口，通过使用这个类和接口就可以生成动态代理对象。JDK提供的代理只能针对接口做代理。我们有更强大的代理cglib，Proxy类中的方法创建动态代理类对象
* Proxy
	* public static Object newProxyInstance(ClassLoader loader,Class<?>[] interfaces,InvocationHandler h) 创建对象
* InvocationHandler接口中的方法
	* InvocationHandler Object invoke(Object proxy,Method method,Object[] args)

### 模版(Template)设计模式
模版方法模式就是定义一个算法的骨架，而将具体的算法延迟到子类中来实现.
* 优点
	* 使用模版方法模式，在定义算法骨架的同时，可以很灵活的实现具体的算法，满足用户灵活多变的需求
* 缺点
	* 如果算法骨架有修改的话，则需要修改抽象类


## <font color=orange>新特性</font>

### JDK5新特性
>* 枚举

* JDK5之前自己实现枚举类
	

	//写法一: 使用私有构造方法, 静态final变量, 无参构造方法
	public class Week {
		public static final Week MON = new Week();
		public static final Week TUE = new Week();
		private Week() {}
	}	

	//写法二:私有构造方法, 静态final变量, 有参构造方法
	public class Week {
		public static final Week MON = new Week("星期一");
		public static final Week TUE = new Week("星期二");
		private Week() {}
	
		private String name;
		private Week(String name) {
			this.name = name;
		}
		public String getName() {
			return name;
		}
	}

	//写法三: 使用抽象类, 使用静态final变量指向其匿名内部类的子类对象.
	public abstract class Week {
		public static final Week MON = new Week("星期一") {
			public void show() {
				System.out.println(getName);
			};
		};
		public static final Week TUE = new Week("星期二") {
			public void show() {
				System.out.println(getName);
			};
		};
	
		private String name;

		private Week() {}

		private Week(String name) {
		this.name = name;
		}

		public String getName() {
			return name;
		}
	
		public abstract void show();
	}

* JDK5以后实现方式
	* 定义枚举类要用关键字enum
	* 所有枚举类都是Enum的子类
	* 枚举类的第一行上必须是枚举项，最后一个枚举项后的分号是可以省略的，但是如果枚举类有其他的东西，这个分号就不能省略。建议不要省略
	* 枚举类可以有构造器，但必须是private的，它默认的也是private的。
	* 枚举类也可以有抽象方法，但是枚举项必须重写该方法
	* 枚举可以在switch语句中的使用
	
	
	//方式1: 
	enum Week {
		MON,TUE;
	}

	//方式2: 
	public enum Week {
		MON("星期一"),TUE("星期二");
	
		private String name;
		private Week(String name) {
			this.name = name;
		}
		public String getName() {
			return name;
		}
	}

	//方式三
	public abstract enum Week {
		MON("星期一") {
			@Override
			public void show() {
				System.out.println(getName());
			}
		},TUE("星期二") {
			@Override
			public void show() {
				System.out.println(getName());
			}
		};
		private String name;

		public String getName() {
			return name;
		}

		private Week(String name) {
			this.name = name;
		}

		public abstract void show();
	}
* 枚举类常用方法
	* int ordinal() 返回此枚举的序数(从0开始)
	* int compareTo(E o) 比较的是编号
	* String name() 获取实例的名称
	* String toString() 
	* <T> T valueOf(Class<T> type,String name) 通过字节码文件获取枚举对象
	* values() 获取所有枚举值

>* 自动拆装箱

* 基本数据类型和其包装类可以自动转换.
>* 泛型

* 用来解决Object的类型转换等安全问题.
>* 可变参数

* 方法的参数可以有多个, 使用...表示
>* 静态导入

* 可以导入类里面的静态方法
>* 增强for循环

* for(类型 变量名: 集合/数组) {} 底层使用迭代器实现
>* 互斥锁

* ReentrantLock, 可以用来替代synchronizedhe和wait、notify 和 notifyAll等方法.


### JDK7新特性
* 二进制的字面量
	* 使用0b开头的0和1字符串表示, 如 0b111.
* 数组字面量可以出现下划线
	* 100_0_00
* switch 语句可以使用字符串
* 泛型简化, 菱形泛型
* 异常的多个catch合并, 每个异常使用|隔开
* try-with-resources (格式: try (//code) {//code}), 1.7标志的异常关流语句, 自动关流.

### JDK8新特性
* 接口中可以定义有方法体的方法.
	* 如果是非静态,必须用default修饰
	* 如果是静态方法, 不用default修饰
* 局部内部类在调用它所在的方法里面的局部变量的时候, final可以省略, 默认会自动添加关键字final. 
