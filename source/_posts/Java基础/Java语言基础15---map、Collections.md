---
layout: post
title: Java语言基础15---map、Collections工具类
comments: true
date: 2016-06-02 14:07:07
tags:
	- Java
---
public interface Map<K,V＞ 将映射到值的对象, 一个映射不能包含重复的键, 每个键最多只能映射到一个值.
* 和Objective-C里面的NSDictionary和Swift里面的Dictionary相似.
* Map是双列集合, Set是单列集合, 底层依赖的是双列集合Map.

<!--more-->

## <font color=orange>Map</font>
* Map
	* 子类
		* HashMap  效率最高, 没特殊要求一般使用这个
		* LinkedHashMap 可以保证存取顺序
		* TreeMap 可以排序
	* 常用方法
		* 添加功能
			* V put(K key,V value):添加元素。
				* 如果键是第一次存储，就直接存储元素，返回null
				* 如果键不是第一次存在，就用值把以前的值替换掉，返回以前的值
		* b:删除功能
			* void clear():移除所有的键值对元素
			* V remove(Object key)：根据键删除键值对元素，并把值返回
		* c:判断功能
			* boolean containsKey(Object key)：判断集合是否包含指定的键
			* boolean containsValue(Object value):判断集合是否包含指定的值
			* boolean isEmpty()：判断集合是否为空
		* d:获取功能
			* Set<Map.Entry<K,V＞＞ entrySet():
			* V get(Object key):根据键获取值
			* Set<K＞ keySet():获取集合中所有键的集合
			* Collection<V＞ values():获取集合中所有值的集合
		* e:长度功能
			* int size()：返回集合中的键值对的个数
	* 常用遍历方法
		* 先获取keySet, 然后通过keySet的迭代器来使用Map的get(Object key)获取值
		* 使用增强for循环, 遍历map.keySet, 然后根据键获取值
		* 通过获取键值对对象(Map.Entry<K,V)来获取键和值(迭代器和增强for循环)
		>	//里面的为了显示＞，用了全角的>
		Map<Car, String> map = new HashMap<＞();
		map.put(new Car("red", 134.333f), "redCar");
		map.put(new Car("blue", 8989.98f), "blueCar");
		map.put(new Car("purple", 32423.22f), "purpleCar");
		//System.out.println(map);
		//增强for循环
		for (Car car : map.keySet()) {
			&#8195;&#8195;System.out.println(map.get(car));
		}
		System.out.println("------------");
		//keySet的迭代器
		Set<Car＞ set = map.keySet();
		Iterator<Car＞ it = set.iterator();
		while (it.hasNext()) {
			&#8195;&#8195;System.out.println(map.get(it.next()));
		}
		System.out.println("------------");
		Set<Map.Entry<Car, String＞＞ entrySet = map.entrySet();
		Iterator<Map.Entry<Car, String＞＞ entryIt = entrySet.iterator();
		while (entryIt.hasNext()) {
			&#8195;&#8195;Map.Entry<Car, String＞ keyValue = entryIt.next();
			&#8195;&#8195;Car key = keyValue.getKey();
			&#8195;&#8195;String value = keyValue.getValue();
			&#8195;&#8195;System.out.println(value + key);
		}
		System.out.println("------------");
		for (Map.Entry<Car, String＞ entry : map.entrySet()) {
			&#8195;&#8195;System.out.println(entry.getValue() + entry.getKey());
		}

### 统计字符串里面的每个字符串出现次数


	String st = "abccbaabccbaaaabbbccc";
		Map<Character, Integer> map = new HashMap<>();
		for (char c : st.toCharArray()) {
			/*if (!map.containsKey(c)) {
				map.put(c, 1);
			} else {
				map.put(c, map.get(c) + 1);
			}*/
			map.put(c, map.containsKey(c) ? map.get(c) + 1 : 1);
		}
		for (Entry<Character, Integer> entry : map.entrySet()) {
			System.out.println(entry.getKey() + ":" + entry.getValue() + "个");
		}


### HashMap和Hashtable
* 区别
	* HashMap是线程不安全的, 效率高,JDK1.2
	* Hashtable是线程安全的, 效率相对低点, JDK1.0
	* Hashtable不能存储null键和null值
	* HashMap可以存储null键和NULL值


### <font color=orange>Collections工具类</font>
* 常用方法
	* 排序
		* public static <T> void sort(List<T> list)
	* 查找, 存在就返回index, 否则返回-(插入点+1)
		* public static <T> int binarySearch(List<?> list,T key)
	* 工具默认排序结果查找最大值
		* public static <T> T max(Collection<?> coll)
	* 反转
		* public static void reverse(List<?> list)
	* 随机置换,集合中顺序随机
		* public static void shuffle(List<?> list)
