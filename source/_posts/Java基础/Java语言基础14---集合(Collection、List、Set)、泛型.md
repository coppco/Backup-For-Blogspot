---
layout: post
title: Java语言基础14---集合(Collection、List、Set)、泛型
comments: true
date: 2016-05-28 10:21:32
tags:
	- Java
---

数组的长度是固定的, 当添加的元素超过了数组的长度时,需要重新定义新的数组, Java内部提供了集合类, 可以存储任意类型, 长度是可以改变的, 随着元素的增加而增加, 随着元素的减少而减少.
* Objective-C里面字符串(NSString)、数组(NSArray)、字典(NSDictionary)和集合(NSSet)都是不可变的, 而NSMutableString、NSMutableArray、NSMutableDictionary和NSMutableSet分别对应是可变的.
* Swift里面let声明的是不可变的, var声明的是可变的.
<!--more-->


* 数组和集合的区别
	* 数组可以存储基本数据类型(值),可以存储引用数据类型(地址值).
	* 集合只能存储引用数据类型, 当存储基本数据类型的时候, 系统会自动装箱变为对象.
	* 数组长度固定, 不能自动增长.
	* 集合的长度的可变的, 可以根据原始的增加而增加.
* 什么时候使用
	* 元素个数固定的推荐使用数组
	* 元素个数不是固定的推荐使用集合

## <font color=orange>Collection</font>
Collection是util包下面的一个接口
* List  有序, 有索引, 元素可以重复
	* Vector     被ArrayList替代了, 数组实现
	* ArrayList  数组实现
	* LinkedList 链表实现
* Set   无序, 无索引, 元素不能重复
	* HashSet	哈希算法
		* LinkedHashSet 使用链表实现的
	* TreeSet	二叉树算法

* Collection常用方法
	* boolean add(Object e)  往集合中添加元素
		* 如果是List集合, 一直返回true
		* 如果是Set集合, 当集合中有重复元素会返回false
	* boolean remove(Object e) 从集合中移除元素
		* 底层也是通过equals()来判断
	* void clear()  清空集合
	* boolean contains(Object e) 判断集合是否包含某元素
		* 底层使用的对象的equals()方法判断, 如判断是否包含某个对象, 可以重写类的这个方法
	* boolean isEmpty()  判断集合是否为空  
	* int size()  集合中元素个数
	* Object[] toArray()  把集合转为数组

	* boolean addAll(Collection c) 把集合c中的所有元素添加进来
		* 如果使用add()来添加一个集合, 被添加进去的集合作为一个对象, 而addAll()方法, 每一个元素分别添加进集合
	* boolean containsAll(Collection<?> c) 判断集合是否包含集合c中所有元素
	* boolean removeAll(Collection<?> c) 从集合中移除交集
	*  boolean retainAll(Collection<?> c)  保留两个集合的交集
		*  会把交集的结果赋值给调用者
		* 如果调用者改变返回true, 未变化返回false
	* Iterator<E> iterator() 获取迭代器, Iterator是一个接口
		* 	boolean hasNext() 是否可以继续迭代
		*	E next() 获取下一个元素
		*	void remove() 从迭代器指向的 collection 中移除迭代器返回的最后一个元素（可选操作）


	Collection dd = new ArrayList();
		dd.add(new Student("张三", 13));
		dd.add(new Student("李四", 23));
		dd.add(new Student("王五", 33));
		dd.add(new Student("熊二", 43));
		dd.add(new Student("熊大", 53));
		dd.add(1);
		Iterator it = dd.iterator();
		while (it.hasNext()) {
			Object o = it.next();
			if (o instanceof Student) {
				Student s = (Student)o;
				System.err.println(s.getName() + "  " + s.getAge());
			}
		}
 

### <font color=orange>List</font>
List除了实现了Collection接口里面的方法外, 还有自己的方法
* void add(int index, E element)  在列表的指定位置插入指定元素（可选操作）。 
* E remove(int index)  移除列表中指定位置的元素（可选操作）。
	* 删除int类型的时候, 不会自动装箱, 当做索引了
* E get(int index)  返回列表中指定位置的元素。
	* 可以通过索引遍历取到值
* int indexOf(Object o)  返回此列表中第一次出现的指定元素的索引；如果此列表不包含该元素，则返回 -1。  
* E set(int index, E element)  用指定元素替换列表中指定位置的元素（可选操作）。 


	List list = new ArrayList();
		list.add(10);
		list.add(11);
		list.add(12);
		list.add(13);
		list.add(14);
		list.add(15);
		
		for (int i = 0; i < list.size(); i++) {
			System.out.println(list.get(i));
		}

### 并发修改异常
ConcurrentModificationException 当方法检测到对象的并发修改，但不允许这种修改时，抛出此异常。 
* 例如在遍历集合的同时,增加元素就会抛出此异常 


	List list = new ArrayList();
		list.add(10);
		list.add(11);
		Iterator it = list.iterator();
		while (it.hasNext()) {
			System.out.println(it.next());
			list.add("a");//会抛出异常
		}
* 解决方法
	* 迭代器迭代元素, 迭代器修改元素(ListIterator的特有功能add)
		* ListIterator可以从前向后和从后向前来遍历
		* ListIterator也可以增加(add)、删除(remove)、修改(set)
		>	List l1 = new ArrayList();
l1.add(1);
l1.add(2);
l1.add(3);
ListIterator it = l1.listIterator();
while (it.hasNext()) {
&#8195;&#8195;Object o = it.next();
&#8195;&#8195;System.out.println(o);
&#8195;&#8195;if (o.equals(2)) {				
&#8195;&#8195;&#8195;&#8195;it.add("a");
&#8195;&#8195;}
}
System.out.println(l1);

	* 集合遍历元素, 集合修改元素, 删除和添加注意索引的变化
	>	List list = new ArrayList<>();
list.add(1);
list.add(2);
list.add(3);
for (int i = 0; i < list.size(); i++) {
&#8195;&#8195;Object o = list.get(i);
&#8195;&#8195;System.out.println(o);
&#8195;&#8195;if (o.equals(2)) {
&#8195;&#8195;&#8195;&#8195;list.add(100);	
&#8195;&#8195;}
}

### <font color=orange>Vector类</font>
它的迭代通过Enumeration<E> elements()  来实现的
* Enumeration
	* boolean hasMoreElements()  测试此枚举是否包含更多的元素。 
	* E nextElement() 如果此枚举对象至少还有一个可提供的元素，则返回此枚举的下一个元素。 
* Vector    线程安全的, 效率低 
* ArrayList 线程不安全的, 效率高

### 数组和链表
* 数组
	* 查询快, 修改也快
	* 增删慢
* 链表
	* 查询慢, 修改也慢
	* 增删快

* List的单个子类的特点
	* ArrayList
		* 底层数据结构是数组, 查询快, 增删慢
		* 线程不安全, 效率高
	* Vector
		* 底层数据结构是数组, 查询快, 增删慢
		* 线程安全, 效率低
	* LinkedList
		* 底层数据结构是链表, 查询慢, 增删快
		* 线程不安全, 效率高


### <font color=orange>LinkedList</font>
LinkedList除了实现了Collection和List的接口外, 还有自己的方法.
* addFirst(E e) 和 addLast(E e) 在首尾添加元素
* getFirst() 和 getLast() 获取首尾元素
* removerFirst() 和 removeLast() 移除首尾元素
* get(int index) 根据下标获取元素


## <font color=orange>泛型</font>

* 泛型好处
	* 提高安全性(将运行期的错误转换到编译期) 
	* 省去强转的麻烦
* 泛型使用注意事项
	* 前后的泛型必须一致,或者后面的泛型可以省略不写(1.7的新特性菱形泛型) 
	* <>中放的必须是引用数据类型  


	//泛型的使用
	ArrayList<Person> list = new ArrayList<>();
	list.add(new Person("张三", 24));
	list.add(new Person("李四", 24));
	Iterator<Person> it = list.iterator();
	while (it.hasNext()) {
		//注意这里不能多次调用next方法,不然循环会遗漏
		Person person = (Person) it.next();
		System.out.println(person.getName());
	}

* 泛型类
	* 在定义类的时候后面使用<T>修饰, 其中T只是一个符号, 其他字母也可以, 一般使用大写字母.
	* 泛型方法
		* 实例方法可以使用类的泛型, 如果不一致, 需要在方法名前面添加自己的泛型.
	* 静态泛型方法,  必须使用自己的泛型类型, 需要在方法名前面添加泛型


	class Person<T> {
		private String name;
		private int age;

		public void show(T t) {
			System.out.println(t);
		}
		
		public<E> void hit(E e) {
			System.out.println(e);
		}
	
		public static<F> void sleep(F f) {

		}
		
	}
	
* 泛型接口


	interface Inter<T> {
		public abstract void show(T t);
	}


	class Student implements Inter<String> {

		@Override
		public void show(String t) {
		
		}
	
	}

### <font color=orange>泛型通配符</font>
* <?> 
	* 表示任意类型, 如果没有明确指定就是Object及其子类
* <? extends E>
	* 向下限定, E及其子类
* <? super E>
	* 向上限定, E及其父类

### <font color=orange>增强for循环</font>
底层使用迭代器实现的, 数组和集合通用. Eclipse中可以使用fore来快速提示, JDK1.5新特性.

	ArrayList<String> list = new ArrayList<>();
		list.add("a");
		list.add("a");
		list.add("a");
		list.add("a");
		for (String string : list) {
			System.out.println(string);
		}

* 几种循环删除
	* 普通for循环, 需要使用集合的删除方法
		>	ArrayList<String> list = new ArrayList<>();
		list.add("a");
		list.add("b");
		list.add("b");
		list.add("d");
		for (int i = 0; i < list.size(); i++) {
		&#8195;&#8195;if ("b".equals(list.get(i))) {
		&#8195;&#8195;&#8195;&#8195;list.remove(i--);
		&#8195;&#8195;}
		}
	* 迭代器遍历, 使用迭代器添加删除方法
		>	ArrayList<String> list = new ArrayList<>();
		list.add("a");
		list.add("b");
		list.add("b");
		list.add("d");
		Iterator<String> it = list.listIterator();
		while (it.hasNext()) {
		&#8195;&#8195;String string = (String) it.next();
		&#8195;&#8195;if ("b".equals(string)) {
		&#8195;&#8195;&#8195;&#8195;it.remove();
		&#8195;&#8195;}
		}
	* for增强循环, 不能实现删除 

### <font color=orange>静态导入</font>
静态导入的是类中的静态方法, jdk1.5 新增特性
* 格式
	* import static 包名.类名.方法名; //一个静态方法
	* import static 包名.类名.*; //所有静态方法
* 导入后, 可以直接使用, 不用加类名, 但是如果多个方法名一样的方法, 反而会搞不清楚, 一般很少使用

### <font color=orange>可变参数</font>
定义方法的时候不知道该定义多少个参数
* 格式
	* 修饰符 返回值类型 方法名(数据类型…  变量名){}
* 注意事项：
	* 这里的变量其实是一个数组
	* 如果一个方法有可变参数，并且有多个参数，那么，可变参数肯定是最后一个


### <font color=orange>Array和List转换</font>
* 数组转集合
	* Arrays的asList()方法, 可以把数组转为集合
		* 基本数据类型数组转为集合, 整个数组作为对象转为集合中元素.
		* 引用数据类型数组转为集合, 每个元素转为集合中元素
	* 不能增加删除
* 集合转数组
	* 使用List的<T> T[] toArray(T[] a)  方法, 如果数组a的length小于等于集合的size, 那么转换后的数组长度等于集合的size, 如果数组的length大于集合的size, 那么数组里面的值都是null

## HashSet
没有特有的方法, 和Collection的方法一样, 只不过往Set里面添加元素的时候, 先判断hashCode值是否一样, 不一样就存储进去, 一样再判断equals是否为true, 当为true时不存储, 为false的时候才会存储进Set.一般自定义类型可以使用Eclipse生成 Alt + Shift + s + h来生成hashCode和equals方法.


	HashSet<Integer> hs = new HashSet<>();
		Random random = new Random();
		while (hs.size() < 10) {
			hs.add(random.nextInt(20) + 1);
		}
		
		for (Integer integer : hs) {
			System.out.println(integer);
		}

### <font color=orange>LinkedHashSet</font>
HashSet的子类, 可以保证存入顺序.

## <font color=orange>TreeSet</font>
TreeSet集合是用对象元素进行排序的, 同时也可以保证元素的唯一性.
* 里面的自定义类可以实现Comparable接口, 重写compareTo(E e)方法. TreeSet集合中如何存储元素取决于compareTo()方法的返回值, 当返回0, 那么不会存储, 返回小于0存储子左边, 返回大于1存储在右边.
* 或者创建TreeSet的时候, 使用比较器(Comparator)排序

	
	//使用比较器来实现排序, 可以决定是否保留重复
	public static void sorted(List<String> ls) {
		//使用匿名内部类来实现
		TreeSet<String> ts = new TreeSet<>(new Comparator<String>() {
			@Override
			public int compare(String o1, String o2) {
				if (o1.compareTo(o2) == 0) {
					return -1;
				}
				return o1.compareTo(o2);
			}
			@Override
			public boolean equals(Object obj) {
				
				return super.equals(obj);
			}
		});
		
		
		for (String string : ls) {
			ts.add(string);
		}
	}
