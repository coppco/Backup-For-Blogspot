---
layout: post
title: Java语言介绍
comments: true
date: 2016-05-03 14:52:26
tags:
	- Java
---

# <font color=orange>Java语言平台</font>
* JavaSE(Java 2 Platform Standard Edition)标准版
  * 是为开发普通桌面和商务应用程序提供的解决方案,该技术体系是其他两者的基础，可以完成一些桌面应用程序的开发
* JavaME(Java 2 Platform Micro Edition)小型版
  * 是为开发电子消费产品和嵌入式设备提供的解决方案
* JavaEE(Java 2 Platform Enterprise Edition)企业版
  * 是为开发企 7888888业环境下的应用程序提供的一套解决方案,该技术体系中包含的技术如 Servlet、Jsp等，主要针对于Web应用程序开发 

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

# <font color=orange>JDK、JRE的下载</font>
[官网下载地址](https://www.oracle.com/downloads/index.html)

# <font color=orange> Linux安装JDK </font>

* 检查是否安装了JDK
	* `java -version`: 如果未找到命令, 那么就没有安装, 有版本信息那么久已经安装了
* 查看安装了哪些JDK版本
	* `rpm -qa | grep java`
* 卸载JDK
	* `rpm -e --nodeps 卸载的包`: `-e`是卸载, `--nodeps`是不验证软件包的依赖关系
* 安装JDK(tar.gz压缩包), 如果是rpm包直接运行`rpm -ivh rpm包`即可, rpm包默认安装在`/usr/java`目录下
	* 1、通过scp命令把文件传递到Linux服务器: `scp 文件路径 用户名@服务器ip地址:路径`, 输入密码后即可开始传递
	* 2、先把文件拷贝到`/usr/local/java`目录下
	* 3、解压tar压缩包: `tar -xvf tar包名`
	* 4、安装插件: `yum install glibc.i686`
	* 5、配置环境变量
		* 5.1、编辑`/etc/profile`文件, 运行`vi /etc/profile`命令
		* 5.2、在末尾添加一下内容
		>	#set java environment
		#JAVA_HOME后面是你的JDK安装路径, 路径和版本不一样可能不同
		JAVA_HOME=/usr/local/java/jdk1.7.0_72
		CLASSPATH=.:$JAVA_HOME/lib.tools.jar
		PATH=$JAVA_HOME/bin:$PATH
		export JAVA_HOME CLASSPATH PATH
		* 5.3、使更改的配置立即生效: `source /etc/profile`
	* 6、运行: `java -version`如果可以看到版本信息即可
		* `echo ＄JAVA_HOME`: 查看JAVA_HOME环境变量
		* `echo ＄CLASSPATH`: 查看CLASSPATH环境变量
		* `echo ＄PATH`: 查看PATH环境变量

# <font color=orange>Java开发工具</font>
* notepad    window系统记事本
* Editplus/Notepad++
* Eclipse
* MyEclipse
