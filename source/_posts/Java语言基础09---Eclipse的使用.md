---
layout: post
title: Java语言基础09---Eclipse的使用
comments: true
date: 2016-08-12 20:52:52
tags:
	- Java
	- Elicpse
	- IDE
---

## IDE

IDE是集成开发环境(Integrated Development Environment)

* Java常用的IDE工具
	* Eclipse
		* 由IBM研发的
		* 免费
		* 纯Java语言编写
		* 免安装
		* 扩展性强
		* 下载地址:[官网](http://eclipse.org)
	* MyEclipse
		* 在Eclipse基础上追加的功能性插件,对插件收费
		* 在WEB开发中提供强大的系统架构平台
* Objective-C和Swift使用的IDE工具
	* Xcode
<!--more-->

## Eclipse界面介绍和设置
* 视窗 : 每一个基本的窗体被称为视窗
	* PackageExplorer, 包资源管理器: 显示项目结构、包、类以及资源
	* Outline, 大纲视窗: 显示类的结构, 方便查找、识别和修改
	* Console, 控制台: 程序运行的结果在该窗口显示
	* Hierarchy : 显示Java基础层次结构, 选中类后F4
* 视图 : 是由某些视窗的组合而成
	* Java视图
	* Debug视图
* 汉化
	* Eclipse3.5开始, 安装目录下面多了一个dropins目录,只要将插件解压后发到该目录即可
	* 卸载插件, 删除即可
	* 一般开发不推荐使用汉化版本
* 程序的编译和运行环境配置(一般不改)
	* window---Preferences---Java
		* 编译环境Compiler, 默认选择最高版本
		* 运行环境Installed JREs, 默认是按照的那个JDK, 建议配置了Java的环境变量.
		* 低编译,高运行----可以
		* 高编译,低运行----不可以
		* 一般编译和运行版本一致
* 去掉方法里面的默认todo注释
	* window---Preferences---Code Style---Code Templates---Code---找到相应的模板---Edit---删除不要的注释
* 行号的显示和隐藏
	* 在代码区域的最左边的空白区域, 右键---Show Line Numbers即可
* 字体大小和颜色的设置
	* Java代码区域的字体设置
		* window--preferences---General---Appearance---Colors And Fonts---Java---Java Edit Text Font---Edit
	* 控制台区域的字体设置
		* window--preferences---General---Appearance---Colors And Fonts---Debug---Console Font---Edit
	* 其他文件
		* window--preferences---General---Appearance---Colors And Fonts---Basic---Text Font---Edit
* 显示窗口, 如Outline、Console等窗口
	* window---Show View	
* 取消悬浮窗
	* window---Preferences---Java---Editor--Hovers---Combined Hovers---勾去掉.
	* 去掉悬浮窗之后, 想看的话,鼠标放上去之后再按F2又可以显示了

### Eclipse内容辅助键的使用

* alt + /   起到提示作用,提高编码效率
	* 输入一些关键字可以自动导入包、代码的自动补齐
	* 和Xcode里面的自动提示差不多,Xcode里面是Esc快捷键, 不过Xcode里面是自动提示的,这里需要使用这个快捷键.
	* 也可以自定义提示
		* window---preferences---Java---Editor---Templates---New
		* Xcode里面也可以自定义代码块

### eclipse的快捷键
* Ctrl + n : 新建
* Ctrl + Shift + f : 格式化代码
	* PS : 有时候按了无效是因为 输入法的快捷键冲突导致的, 可以修改输入法的快捷键.
	* 如搜狗输入法修改如下 : ![去掉快捷键](http://oak4eha4y.bkt.clouddn.com/formatterCodeFail.png)
* Ctrl + Shift + o : 导入包
* Ctrl + / : 添加/取消单行注释
* Ctrl + Shift + /, Ctrl + Shift + \ : 添加/取消多注释
* 选中代码块  Alt + 上/下箭头 : 代码上下移动
* Ctrl + Alt + 上/下箭头 : 向上/下复制一行
* 选中类名   F3 或者 Ctrl + 鼠标点击 : 查看源码
* Ctrl + Shift + t : 查找具体的类
* Ctrl + o : 查找具体类的具体方法
* Ctrl + 1 : 根据右边生成左边的数据类型, 生成方法
* Ctrl + d : 删除代码
* Alt + Shift + m :　抽取方法
* Alt + Shift + r : 改名
* Alt + Shift + s : 自动生成构造方法和set/get方法
	* c : 空参构造
	* o : 有参构造
	* r : set/get方法
	* v : 重写父类方法
	* m : delegate方法
	* h : 生成hashCode 和 equals方法
	* s : 生成toString方法

### jar
* jar是多个class文件的压缩包
* 别人写好的东西
* 打jar包 : 选中项目---右键---Export---Java---Jar---自己指定一个路径和一个名称---Finish
* 导入jar包 : 复制到项目路径下并添加至构建路径, 选中jar包---右键---build Path---Add to build path
* 移除jar包 : 选中jar包---右键---build Path---remove from build path

### 删除项目和导入项目
* 删除项目
	* 选中项目---右键---delete---(慎重选择是否勾选从本地移除)
	* 不勾选---本地文件还在
	* 勾选---本地文件被删除,回收站也没有(慎重)![删除](http://oak4eha4y.bkt.clouddn.com/delete1.png)
* 导入项目
	* 包资源管理器---右键---import---general---Existing projects into workspace---选中项目所在路径---finish

### Debug
* Debug的作用
	* 调试程序
	* 查看程序执行流程
* 断点
	* 在想要打断点的位置, 代码区域行号左边双击后,会有一个点
	* 代码区域---右键---Debug as 或者 F11
* Debug视图
	* Debug : 里面可以看到方法进栈情况
	* Variables : 查看程序的变量变化
	* ForDemo : 源文件
	* Console : 控制台
	* Breakpoints : 查看/删除断点
	* F6 : 可以一步一步往下走
	* F5 : 进入方法

* 新建类的时候说明:![新建类](http://oak4eha4y.bkt.clouddn.com/eclipse.png)