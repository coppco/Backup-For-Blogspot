---
layout: post
title: Java语言基础01---注释、关键字和标识符
comments: true
date: 2016-08-04 14:20:06
tags:
	- Java
---

## <font color=orange>注释</font>
用于解释说明程序的文字(和OC、Swift一致)
* 单行注释
	* 格式://注释文字
* 多行注释
	* 格式:/*注释文字*/
* 文档注释
	* 格式:/**注释文字*/

<!--more-->
## <font color=orange>关键字</font>
被Java语言赋予特定含义的单词,如: class、static等, Java中的关键字全部为小写字母,关键字不能作为类名来使用, goto和const作为保留字也是关键字,也不能使用.(和OC、Swift差不多的关键字)

## <font color=orange>标识符</font>
就是给类、接口、方法、变量等起名字时使用的字符序列

### 组成规则(和OC、Swift差不多,多了一个$符号)
* 英文大小写字母
* 数字
* $和_

### 注意事项(和OC、Swift差不多,多一个$符号)
* 不能以数字开头
* 不能是Java的关键字
* 区分大小写

### 命名规则
* 包(其实就是文件夹,用于解决相同类名问题): 一般使用域名倒着写 com.github.xxx
* 类或者接口(类命名和OC、Swift一致)：首字母大写, 多单词要求每个单词首字母大写(驼峰命名)如: Person、PublicTools
* 方法和变量(和OC、Swift一致):　一个单词,都小写,多个单词,第二个单词开始首字母大写.如: get、maxCount
* 常量: 一个单词,使用大写字母, 多个单词,大写中间使用下划线_ 如: MAX、MAX_VALUE

