---
layout: post
title: Java Web学习21---Maven
comments: true
toc: true
date: 2016-12-20 09:12:23
tags:
	- Java
	- Spring
---

&emsp;&emsp;Maven翻译为“专家”，“内行”。Maven是Apache下的一个纯java开发的开源项目，它是一个项目管理工具，使用Maven对java项目进行构建、依赖管理。当前使用Maven的项目在持续增长。
<!--more-->

# <font color=orange>Maven</font>
&emsp;&emsp;Maven是一个项目管理工具，它包含了一个项目对象模型 (Project Object Model)，一组标准集合，一个项目生命周期(Project Lifecycle)，一个依赖管理系统(Dependency Management System)，和用来运行定义在生命周期阶段(phase)中插件(plugin)目标(goal)的逻辑。当你使用Maven的时候，你用一个明确定义的项目对象模型来描述你的项目，然后Maven可以应用横切的逻辑，这些逻辑来自一组共享的（或者自定义的）插件。
&emsp;&emsp;Maven 有一个生命周期，当你运行 mvn install 的时候被调用。这条命令告诉 Maven 执行一系列的有序的步骤，直到到达你指定的生命周期。遍历生命周期旅途中的一个影响就是，Maven 运行了许多默认的插件目标，这些目标完成了像编译和创建一个 JAR 文件这样的工作。
&emsp;&emsp;此外，Maven能够很方便的帮你管理项目报告，生成站点，管理JAR文件，等等。

## <font color=orange>Maven的安装</font>
* [下载地址](http://maven.apache.org/download.cgi)
* 解压后目录
	* `bin`: 可运行程序
		* mvn: 以run方式运行项目
		* mvnDebug: 以debug方式运行项目
	* `boot`: 类加载器
	* `conf`: Maven配置文件
	* `lib`: Maven依赖的jar包
* 安装(安装目录最好英文)
	* Linux
		* 先解压, 如:`/usr/local/maven`, 目录可以更改
		* 运行`vi /etc/profile`
		* 添加下面内容
```
MAVEN_HOME=/usr/local/maven

export MAVEN_HOME

export PATH=${PATH}:${MAVEN_HOME}/bin
```
		* 使文件生效: `source /etc/profile`
	* Windows
		* 需要配置`MAVEN_HOME`环境变量, 将`%JAVA_HOME%\bin`配置环境变量path中.
	* 测试
		* 运行`mvn -v`查看版本
## <font color=orange>项目构建</font>

### <font color=green>传统项目的构建过程</font>

* 构建过程
	* 新建`Java Web`项目
	* 导入jar包, 编写源码以及配置文件
	* 对源码进行编译, 精java文件编译成class文件
	* 执行Junit单元测试
	* 将工程打包成war包, 部署到服务器

### <font color=green>Maven项目的构建过程</font>

Maven将项目构建的过程进行标准化，每个阶段使用一个命令完成.

清理------>编译------>测试------>报告------>打包------>部署

清理阶段对应Maven的命令是`mvn clean`，清理输出的class文件
编译阶段对应Maven的命令是`mvn compile`，将java代码编译成class文件。
打包阶段对应Maven的命令是`mvn package`，java工程可以打成jar包，web包可以打成war包

运行一个Maven工程（web工程）需要一个命令：`mvn tomat:run`

Maven工程构建的优点：
1、一个命令完成构建、运行，方便快捷。
2、Maven对每个构建阶段进行规范，非常有利于大型团队协作开发。

## <font color=orange>依赖管理</font>

&emsp;&emsp;什么是依赖管理？就是对项目所有依赖的jar包进行规范化管理。

### <font color=green>传统项目的依赖管理</font>

&emsp;&emsp;传统的项目工程要管理所依赖的jar包完全靠人工进行，程序员从网上下载jar包添加到项目工程中，如下图：程序员手工将Hibernate、struts2、spring的jar添加到工程中的WEB-INF/lib目录下。

手工拷贝jar包添加到工程中的问题是：
1、没有对jar包的版本统一管理，容易导致版本冲突。
2、从网上找jar包非常不方便，有些jar找不到。
3、jar包添加到工程中导致工程过大。

### <font color=green>Maven的依赖管理</font>
Maven项目管理所依赖的jar包不需要手动向工程添加jar包，只需要在pom.xml（Maven工程的配置文件）添加jar包的坐标，自动从Maven仓库中下载jar包、运行.

使用Maven依赖管理添加jar的好处：
1、通过pom.xml文件对jar包的版本进行统一管理，可避免版本冲突。
2、Maven团队维护了一个非常全的Maven仓库，里边包括了当前使用的jar包，Maven工程可以自动从Maven仓库下载jar包，非常方便。

## <font color=orange>Maven仓库</font>
* 本地仓库: 用来存储从远程仓库或中央仓库下载的插件和jar包，项目使用一些插件或jar包，优先从本地仓库查找 
	* 在maven安装目录下的有`conf/setting.xml`文件，此setting.xml文件用于maven的所有project项目，它作为maven的全局配置。
```xml
 <localRepository>你的本地查看目录</localRepository>
```
* 远程仓库: 如果本地需要插件或者jar包，本地仓库没有，默认去远程仓库下载。远程仓库可以在互联网内也可以在局域网内。
* 中央仓库: 在Maven软件中内置一个[远程仓库地址](http://repo1.maven.org/maven2) ,它是中央仓库，服务于整个互联网，它是由Maven团队自己维护，里面存储了非常全的jar包，它包含了世界上大部分流行的开源项目构件。

### <font color=green>配置Maven中央仓库</font>
由于国内墙的原因, 访问默认的中央仓库很慢, 所以需要配置一些Maven的国内镜像, 如果没有配置mirror, 默认使用[中央仓库](http://repo1.maven.org/maven2), 
* 可以修改的地方
	* Maven安装目录/conf/setting.xml
	* 用户名/.m2/setting.xml
* 添加下面的内容
```xml
<!-- 阿里云仓库 -->
<mirror>
    <id>nexus-aliyun</id>
    <mirrorOf>*</mirrorOf>
    <name>Nexus aliyun</name>
    <!--使用那个public的地址, 导致Intellij IDEA中更新索引失败-->
    <url>http://maven.aliyun.com/nexus/content/repositories/central</url>
</mirror>   
```
* 字段说明
	* id，name：镜像的唯一标识和用户友好的名称；
	* url：镜像的url，用于代替原始仓库的url；  
	* mirrorof：使用镜像的仓库的id，可以使用下面匹配属性：
		* `*`：匹配所有仓库id；
		* `external:*`：匹配所有仓库id，除了那些使用localhost或者基于仓库的文件的仓库；
		* `repo1,repo2`；多个仓库id之间用逗号分隔
		* `!repol`：表示匹配所有仓库，除了repol。

## <font color=orange>Maven项目目录结构</font>
Project
&nbsp;&nbsp;|-src
&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;|-main
&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;|-java        —— 存放项目的.java文件
&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;|-resources   —— 存放项目资源文件，如spring, hibernate配置文件
&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;|-webapp     —— webapp目录是web工程的主目录
&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;|-WEB-INF
&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;|-web.xml
&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;|-test
&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;|-java        ——存放所有测试.java文件，如JUnit测试类
&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;|-resources   —— 测试资源文件
&nbsp;&nbsp;|-target             —— 目标文件输出位置例如.class、.jar、.war文件(运行`mvn clean`后会删除)
&nbsp;&nbsp;|-pom.xml           —— maven项目核心配置文件

## <font color=orange>Maven常用命令</font>
符合Maven项目目录结构的工程可以使用Maven命令.

* compile: `mvn compile`, 会把`src/main/java`目录下的文件编译成class文件并输出到target目录中
* test: `mvn test`, 会运行`src/test/java`下的单元测试类
* clean: `mvn clean`, 会删除`src/target`目录
* package: `mvn package`, 将java工程打包成jar包, web工程打包成war包.
* install: `mvn install`, maven打成jar包或war包发布到本地仓库。

# <font color=orange>创建Maven项目</font>

* Eclipse
	* Preference---Maven---Installations中选择或者新建Maven版本
	* Preference---Maven---User Setting中设置配置文件
	* 选择`Maven Project`, 取消选择`Create a simple project`
		* `Maven-archetype-quickstart`: 创建一个Maven的Java工程
		* `Maven-archetype-webapp`: 创建一个Maven的Web工程
	* 初始化项目, 有的目录没有需要手动创建
* Intellj IDEA
	* `command ,`------`Maven`------`Importing`------勾选`Import Maven projects automaticlly`
	* `command ,`------`Maven`------`Repositories`------`Update`------更新中央仓库索引到本地
	* 选择`Maven`
	* 勾选`Create from archetype`
		* `Maven-archetype-quickstart`: 创建一个Maven的Java工程
		* `Maven-archetype-webapp`: 创建一个Maven的Web工程
	* 初始化项目, 有的目录没有需要手动创建
	* <font color=red>解决不能创建class和package方法</font>
		* `src/main/java`目录不能创建class和package, 可以右键------`Make Directory as`------`Sources Root`即可
		* `src/test/java`目录不能创建class和package, 可以右键------`Make Directory as`------`Test Sources Root`即可
* 配置`pom.xml`
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.coppco</groupId>
  <artifactId>mavenTest</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>mavenTest Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <!--配置依赖-->
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.9</version>
      <!--只在测试时起作用-->
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>servlet-api</artifactId>
      <version>2.5</version>
      <!--只在运行时不起作用, 因为tomcat中有这个jar包-->
      <scope>provided</scope>
    </dependency>
  </dependencies>
  <build>
    <finalName>mavenTest</finalName>
  </build>
</project>
```
* 依赖范围
	* compile: 编译范围，指A在编译时依赖B，此范围为<font color=red>默认依赖范围</font>。编译范围的依赖会用在编译、测试、运行，由于运行时需要所以编译范围的依赖会被打包。
	* provided: provided依赖只有在当JDK或者一个容器已提供该依赖之后才使用， provided依赖在编译和测试时需要，在运行时不需要，比如：servlet api被tomcat容器提供。
	* runtime: runtime依赖在运行和测试系统的时候需要，但在编译的时候不需要。比如：jdbc的驱动包。由于运行时需要所以runtime范围的依赖会被打包。
	* test: test范围依赖 在编译和运行时都不需要，它们只有在测试编译和测试运行阶段可用，比如：junit。由于运行时不需要所以test范围依赖不会被打包。
	* system: system范围依赖与provided类似，但是你必须显式的提供一个对于本地系统中JAR文件的路径，需要指定systemPath磁盘路径，system依赖不推荐使用。

# <font color=orange>Maven项目分模块</font>
实际开发中, 一般创建Maven项目时都是分模块的,即是一个父工程中包含若干个子module.
* 1、先创建一个父工程, 打包方式为pom, Eclipse勾选`Create a simple project`, Intellij IDEA不要勾选`Create from archetype`.
	* 1.1、父工程中pom.xml中引入的依赖jar包, 所有子工程都可以使用. 一般把共同的jar包放在父工程的pom.xml中.
* 2、为父工程添加子module, `new`---`Maven Module`, `parent Project`必选要设置为父工程的GroupId+ArtifactId+version.
	* 2.1、dao-module可以选择打包模式为jar
	* 2.3、添加完成module后, 父工程的pom.xml和子module的pom.xml中会相会有对方的信息.
```xml
	<!--父工程中-->
    <modules>
        <module>web-service</module>
        <module>web-app</module>
        <module>web-dao</module>
    </modules>
    <!--子工程中-->
    <parent>
        <artifactId>day45-crm</artifactId>
        <groupId>com.coppco</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
```
* 3、引入依赖以及依赖版本冲突
	* 3.1、依赖调解原则
		* 第一声明者优先原则: 在pom文件定义依赖，先声明的依赖为准。
		* 路径近者优先原则: a依赖junit4.0, a继承b, 而b依赖junit4.9, 那么相对于a, 最近的路径是junit4.0.
	* 3.1、也可以通过配置`dependency`---`exclusions`来排除冲突的jar包
```xml
        <dependency>
            <groupId>org.apache.struts</groupId>
            <artifactId>struts2-core</artifactId>
            <version>2.3.24</version>
            <!--排除exclusions中的jar包-->
            <exclusions>
                <exclusion>
                    <artifactId>javassist</artifactId>
                    <groupId>javassist</groupId>
                </exclusion>
            </exclusions>
        </dependency>
```
	* 3.3、版本锁定: 面对众多的依赖，有一种方法不用考虑依赖路径、声明优化等因素可以采用直接锁定版本的方法确定依赖构件的版本，版本锁定后则不考虑依赖的声明顺序或依赖的路径，以锁定的版本的为准添加到工程中，此方法在企业开发中常用。
```xml
<dependencyManagement>
  	<dependencies>
  		<!--这里锁定版本为4.2.4 -->
  		<dependency>
  			<groupId>org.springframework</groupId>
  			<artifactId>spring-beans</artifactId>
  			<version>4.2.4.RELEASE</version>
  		</dependency>
		<dependency>
  			<groupId>org.springframework</groupId>
  			<artifactId>spring-context</artifactId>
  			<version>4.2.4.RELEASE</version>
  		</dependency>
  	</dependencies>
  </dependencyManagement>
```
		* 3.3.1、在工程中锁定依赖的版本并不代表在工程中添加了依赖，如果工程需要添加锁定版本的依赖则需要单独添<dependencies></dependencies>标签.
```xml
	<dependencies>
  		<!--这里是添加依赖 -->
  		<dependency>
  			<groupId>org.springframework</groupId>
  			<artifactId>spring-beans</artifactId>
 		</dependency>
		<dependency>
  			<groupId>org.springframework</groupId>
  			<artifactId>spring-context</artifactId>
 		</dependency>
  	</dependencies>
  ```
* 4、完整的SSH框架, pom.xml如下
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  
  <!-- 项目配置 -->
	<groupId>com.coppco</groupId>
    <artifactId>day45-crm</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    
    <!-- 子module -->
    <modules>
        <module>web-service</module>
        <module>web-app</module>
        <module>web-dao</module>
    </modules>
  
  <!-- 为了确定每个框架的版本号 -->
  <!-- 锁定版本 -->
      <properties>
		 <spring.version>4.2.4.RELEASE</spring.version>
		<struts2.version>2.3.24</struts2.version>
		<hibernate.version>5.0.7.Final</hibernate.version>
        <slf4j.version>1.6.6</slf4j.version>
        <log4j.version>1.2.12</log4j.version>
		<shiro.version>1.2.3</shiro.version>
		<cxf.version>3.0.1</cxf.version>
		<c3p0.version>0.9.1.2</c3p0.version> 
		<mysql.version>5.1.6</mysql.version>
	</properties>
  	<!-- 锁定版本，struts2-2.3.24、spring4.2.4、hibernate5.0.7 -->
	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-context</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-aspects</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-orm</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-test</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-web</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.hibernate</groupId>
				<artifactId>hibernate-core</artifactId>
				<version>${hibernate.version}</version>
			</dependency>
			<dependency>
				<groupId>org.apache.struts</groupId>
				<artifactId>struts2-core</artifactId>
				<version>${struts2.version}</version>
			</dependency>
			<dependency>
				<groupId>org.apache.struts</groupId>
				<artifactId>struts2-spring-plugin</artifactId>
				<version>${struts2.version}</version>
			</dependency>

		</dependencies>
	</dependencyManagement>
  
  <dependencies>
  	<dependency>
			<groupId>org.apache.httpcomponents</groupId>
			<artifactId>httpclient</artifactId>
			<version>4.4</version>
		</dependency>
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>fastjson</artifactId>
			<version>1.1.37</version>
		</dependency>
		<dependency>
			<groupId>commons-beanutils</groupId>
			<artifactId>commons-beanutils</artifactId>
			<version>1.9.1</version>
		</dependency>
		<!-- <dependency>
			<groupId>org.quartz-scheduler</groupId>
			<artifactId>quartz</artifactId>
			<version>2.2.1</version>
		</dependency> -->
		<dependency>
		  <groupId>commons-fileupload</groupId>
		  <artifactId>commons-fileupload</artifactId>
		  <version>1.3.1</version>
		</dependency>
	
		<!-- jstl -->
 	    <dependency>
		  <groupId>jstl</groupId>
		  <artifactId>jstl</artifactId>
		  <version>1.2</version>
	    </dependency>
	    
	    <!-- shiro -->
	     <!-- apache shiro dependencies -->
	     <!-- <dependency>
	    	<groupId>org.apache.shiro</groupId>
	    	<artifactId>shiro-all</artifactId>
	    	<version>${shiro.version}</version>
	     </dependency> -->

      
       <!-- spring -->
		<dependency>
		    <groupId>org.aspectj</groupId>
		    <artifactId>aspectjweaver</artifactId>
		    <version>1.6.8</version>
		</dependency>
		
         <dependency>
	    	<groupId>org.springframework</groupId>
	    	<artifactId>spring-aop</artifactId>
	    	<version>${spring.version}</version>
	    </dependency>
	    
		<dependency>
		  <groupId>org.springframework</groupId>
		  <artifactId>spring-context</artifactId>
		  <version>${spring.version}</version>
		</dependency>
		
		<dependency>
		  <groupId>org.springframework</groupId>
		  <artifactId>spring-context-support</artifactId>
		  <version>${spring.version}</version>
		</dependency>
		
		<dependency>
		  <groupId>org.springframework</groupId>
		  <artifactId>spring-web</artifactId>
		  <version>${spring.version}</version>
		</dependency>

		<dependency>
		  <groupId>org.springframework</groupId>
		  <artifactId>spring-orm</artifactId>
		  <version>${spring.version}</version>
		</dependency>
		
		<dependency>
		  <groupId>org.springframework</groupId>
		  <artifactId>spring-beans</artifactId>
		  <version>${spring.version}</version>
		</dependency>
		
		<dependency>
		  <groupId>org.springframework</groupId>
		  <artifactId>spring-core</artifactId>
		  <version>${spring.version}</version>
		</dependency>
		
	
		
		<!-- struts2  begin -->
		<dependency>
			<groupId>org.apache.struts</groupId>
			<artifactId>struts2-core</artifactId>
			<version>${struts2.version}</version>
			<exclusions>
				<exclusion>
					<artifactId>javassist</artifactId>
					<groupId>javassist</groupId>
				</exclusion>
			</exclusions>
		</dependency>
		<dependency>
			<groupId>org.apache.struts</groupId>
			<artifactId>struts2-spring-plugin</artifactId>
			<version>${struts2.version}</version>
		</dependency>
		<dependency>
			<groupId>org.apache.struts</groupId>
			<artifactId>struts2-json-plugin</artifactId>
			<version>${struts2.version}</version>
		</dependency>
		<dependency>
	  		<groupId>org.apache.struts</groupId>
	  		<artifactId>struts2-convention-plugin</artifactId>
	  		<version>${struts2.version}</version>
	  	</dependency>
		<!-- struts2  end -->
		
		<!-- hibernate begin -->
		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-core</artifactId>
			<version>${hibernate.version}</version>
		</dependency>
		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-entitymanager</artifactId>
			<version>${hibernate.version}</version>
		</dependency>
		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-validator</artifactId>
			<version>5.2.1.Final</version>
		</dependency>
		<!-- hibernate end -->
		
		<dependency>
	  		<groupId>c3p0</groupId>
	  		<artifactId>c3p0</artifactId>
	  		<version>${c3p0.version}</version>
	  	</dependency>
		
		<!-- <dependency> 
			<groupId>org.apache.cxf</groupId> 
			<artifactId>cxf-rt-frontend-jaxws</artifactId> 
			<version>${cxf.version}</version> 
		</dependency> 
		<dependency> 
			<groupId>org.apache.cxf</groupId> 
			<artifactId>cxf-rt-transports-http</artifactId> 
			<version>${cxf.version}</version> 
		</dependency> -->
		
        <!-- log start -->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>${log4j.version}</version>
        </dependency>
        
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>${slf4j.version}</version>
        </dependency>
        
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>${slf4j.version}</version>
        </dependency>
        <!-- log end -->
		
		<!-- Javamail -->
	<!--     <dependency>
	      <groupId>javax.mail</groupId>
	      <artifactId>mail</artifactId>
	      <version>1.4.4</version>
	    </dependency> -->
		
		<dependency>
			<groupId>commons-lang</groupId>
			<artifactId>commons-lang</artifactId>
			<version>2.6</version>
		</dependency>
	
		<dependency>
			<groupId>org.codehaus.xfire</groupId>
			<artifactId>xfire-core</artifactId>
			<version>1.2.6</version>
		</dependency> 
        
		<dependency>
           <groupId>dom4j</groupId>
		   <artifactId>dom4j</artifactId>
		   <version>1.6</version>
	    </dependency>
	
		<!-- <dependency> 
			<groupId>org.apache.poi</groupId> 
			<artifactId>poi</artifactId> 
			<version>3.11</version> 
		</dependency>
		<dependency> 
			<groupId>org.apache.poi</groupId> 
			<artifactId>poi-ooxml</artifactId> 
			<version>3.11</version> 
		</dependency>
		<dependency> 
			<groupId>org.apache.poi</groupId> 
			<artifactId>poi-ooxml-schemas</artifactId> 
			<version>3.11</version> 
		</dependency> -->
	
	    <dependency>
	      <groupId>junit</groupId>
	      <artifactId>junit</artifactId>
	      <version>3.8.1</version>
	      <scope>test</scope>
	    </dependency>
	   
	    <dependency>
	    	<groupId>mysql</groupId>
	    	<artifactId>mysql-connector-java</artifactId>
	    	<version>${mysql.version}</version>
	    </dependency>
	   <!--  <dependency>
	    	<groupId>com.oracle</groupId>
	    	<artifactId>ojdbc14</artifactId>
	    	<version>10.2.0.4.0</version>
	    </dependency> -->
	    <dependency>
	    	<groupId>javax.servlet</groupId>
	    	<artifactId>servlet-api</artifactId>
	    	<version>2.5</version>
	    	<scope>provided</scope>
	    </dependency>
	    <dependency>
	    	<groupId>javax.servlet.jsp</groupId>
	    	<artifactId>jsp-api</artifactId>
	    	<version>2.0</version>
	    	<scope>provided</scope>
	    </dependency>
	    <dependency>
	    	<groupId>net.sf.ehcache</groupId>
	    	<artifactId>ehcache-core</artifactId>
	    	<version>2.6.6</version>
	    </dependency>
  </dependencies>
  
   <build>
 	<finalName>day45-crm</finalName>
	<pluginManagement>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.2</version>
				<configuration>
					<source>1.7</source>
					<target>1.7</target>
					<encoding>UTF-8</encoding>
					<showWarnings>true</showWarnings>
				</configuration>
			</plugin>
		</plugins>
	</pluginManagement>
  </build>
</project>
```
* 4、不同子模块之间依赖需要配置pom.xml
```xml
<!--例如service模块 需要依赖 dao模块-->
    <dependencies>
        <dependency>
            <groupId>com.coppco</groupId>
            <artifactId>web-dao</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>
```