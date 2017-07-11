---
layout: post
title: iOS打包静态库并支持bitcode
comments: true
date: 2016-10-21 11:02:29
tags:
	- iOS
	- Swift
	- Objective-C
---

当我们开发一个供人使用SDK时, 只希望暴露.h头文件, 而不想暴露.m的实现, 这个时候我们可以制作静态库, Xcode可以制作静态库和动态库, 但是Apple要求上架Appstore只能使用静态库, 所有使用动态库的不允许上架.

* 静态库
	* `.a`或`.framework`
	* 链接时，静态库会被完整地复制到可执行文件中， 被多次使用就有多份冗余拷贝
* 动态库
	* `.dylib`或`.framework`
	* 链接时不复制，程序运行时由系统动态加载到内存，供程序调用，系统只加载一次，多个程序共用，节省内存

<!--more-->

# <font color=orange>静态库</font>
* <font color=orange>iOS设备的CPU架构</font>
	* 模拟器
		* iPhone 4S、iPhone 5: <font color=orange>i386</font>
		* iPhone 5S、iPhone 6(S)、iPhone 6(S)P: <font color=orange>x86_64</font>
	* 真机
		* <font color=orange>armv6</font>: iPhone、iPhone 2、iPhone 3G、iPod Touch(第一代)、iPod Touch(第二代)
		* <font color=orange>armv7</font>: iPhone 3Gs、iPhone 4、iPhone 4s、iPad、iPad 2
		* <font color=orange>armv7s</font>: iPhone 5、iPhone 5c (静态库只要支持了armv7,就可以在armv7s的架构上运行)
		* <font color=orange>arm64</font>: iPhone 5s、iPhone 6、iPhone 6 Plus、iPhone 6s、iPhone 6s Plus、iPad Air、iPad Air2、iPad mini2、iPad mini3
* <font color=orange>制作静态库`.a`</font>
	* 新建项目选择`Cocoa Touch Static Library`
	* 导入需要包含的文件
	* 添加需要暴露的头文件: `TARGETS`---`Build Phases`---`+`---`New Headers Phases`
	* 修改最低适配版本: `PROJECT`---`Deployment Target`---`iOS Deployment Target`---`修改为可以支持的最低版本`
	* 设置适配所有模拟器架构: `PROJECT`---`Build Setting`---`Build Active Architecture Only`---设为NO
	* 分别选择模拟器和真机(或Generic iOS Device)进行编译, 在`Products`---`.a文件`---`右键`---`Show in Finder`
		* Debug模式, 默认情况下`Build Active Architecture Only`设置为了YES
			* `Debug-iphoneos`: 适用于真机
			* `Debug-iphonesimulator`: 适用于模拟器
		* Release模式, 默认情况下`Build Active Architecture Only`设置为了NO
			* `Release-iphoneos`: 适用于真机
			* `Release-iphonesimulator`: 适用于模拟器
* <font color=orange>查看静态库支持的架构</font>
	* 终端运行`lipo -info .a文件的路径`
* <font color=orange>制作同时支持真机和模拟器的静态库</font>
	* 1、在Release模式下分别选择模拟器和真机(或Generic iOS Device)编译, 也可以在设置了`Build Active Architecture Only`为NO的情况下分别选择模拟器和真机(或Generic iOS Device)编译
	* 2、终端运行`lipo -create .a文件路径 .a文件路径 -output 新.a文件名`, 那么新的.a文件就在当前终端所在的路径了.
	* 3、再次查看新的.a文件所支持的框架, 如看到`Architectures in the fat file: xxx.a are: armv7 i386 x86_64 arm64`即代表完成.
*  <font color=orange>适配bitcode</font>
	* `TARGETS`---`项目名`---`Build Setting`---`Apple LLVM x.x - Custom Compiler Flags`---在`Other C Flags`和`Other C++ Flags`里面添加<font color=orange>`-fembed-bitcode`</font>
	* 设置后查询.a文件支持不支持bitcode
		* otool -arch armv7 -l xxxx.a | grep __bitcode | wc -l  
		* otool -arch i386 -l xxxx.a | grep __bitcode | wc -l 
		* otool -arch x86_64 -l xxxx.a | grep __bitcode | wc -l
		* otool -arch arm64 -l xxxx.a | grep __bitcode | wc -l  