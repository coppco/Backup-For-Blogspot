---
layout: post
title: Java语言介绍
comments: true
date: 2016-08-03 14:52:26
tags:
	- Java
---

# <font color=orange>Java语言平台</font>
* J2SE(Java 2 Platform Standard Edition)标准版
  * 是为开发普通桌面和商务应用程序提供的解决方案,该技术体系是其他两者的基础，可以完成一些桌面应用程序的开发
* J2ME(Java 2 Platform Micro Edition)小型版
  * 是为开发电子消费产品和嵌入式设备提供的解决方案
* J2EE(Java 2 Platform Enterprise Edition)企业版
  * 是为开发企业环境下的应用程序提供的一套解决方案,该技术体系中包含的技术如 Servlet、Jsp等，主要针对于Web应用程序开发 

<!--more-->

# <font color=orange>Java的跨平台性</font>

* 通过在不同操作系统来运行Java虚拟机(JVM Java Virtual Machine), 由JVM来负责Java程序在该系统中的运行

# <font color=orange>JRE和JDK</font>
* JRE(Java Runtime Environment) Java运行环境
  * 包括Java虚拟机和Java所需的核心类库
* JDK(Java Development Kit) Java开发工具包
  * 包括Java开发工具(编译工具javac.exe和打包工具jar.exe等)和JRE

# <font color=orange>JDK安装路径下的目录解释</font>

* bin目录：该目录用于存放一些可执行程序。
  * 如javac.exe（java编译器）、java.exe(java运行工具)，jar.exe(打包工具)和
    javadoc.exe(文档生成工具)等。
* db目录：db目录是一个小型的数据库。
  * 从JDK 6.0开始，Java中引用了一个新的成员JavaDB，这是一个纯Java实现、开源的数据库管理系统。这个数据库不仅轻便，而且支持JDBC 4.0所有的规范，在学习JDBC 时，不再需要额外地安装一个数据库软件，选择直接使用JavaDB即可。
* jre目录："jre"是 Java Runtime Environment 的缩写，意为Java程序运行时环境。此目录是Java运行时环境的根目录，它包括Java虚拟机，运行时的类包，Java应用启动器以及一个bin目录，但不包含开发环境中的开发工具。
* include目录：由于JDK是通过C和C++实现的，因此在启动时需要引入一些C语言的头文件，该目录就是用于存放这些头文件的。
* lib目录：lib是library的缩写，意为 Java 类库或库文件，是开发工具使用的归档包文件。
* src.zip文件：src.zip为src文件夹的压缩文件，src中放置的是JDK核心类的源代码，通过该文件可以查看Java基础类的源代码。



# <font color=orange>Java开发工具</font>
* notepad    window系统记事本
* Editplus/Notepad++
* Eclipse
* MyEclipse