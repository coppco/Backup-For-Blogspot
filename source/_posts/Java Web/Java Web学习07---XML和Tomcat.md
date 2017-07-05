---
layout: post
title: Java Web学习07---XML和Tomcat
comments: true
date: 2017-01-02 07:02:12
tags:
	- Java
	- XML
	- Tomcat
---
* XML
&emsp;XML 指可扩展标记语言（EXtensible Markup Language）,也是一种标记语言，很类似 HTML.它的设计宗旨是传输数据，而非显示数据它;标签没有被预定义,需要自行定义标签.
* Tomcat
&emsp;Tomcat是Apache 软件基金会（Apache Software Foundation）的Jakarta 项目中的一个核心项目,Tomcat 服务器是一个免费的开放源代码的Web 应用服务器，属于轻量级应用服务器，在中小型系统和并发访问用户不是很多的场合下被普遍使用，是开发和调试JSP 程序的首选,它还是一个Servlet和JSP容器.

<!--more-->

## <font color=orange>XML</font>
&emsp;&emsp;XML 被设计为具有自我描述性,是 W3C 的推荐标准,在电子计算机中，标记指计算机所能理解的信息符号，通过此种标记，计算机之间可以处理包含各种的信息比如文章等。它可以用来标记数据、定义数据类型，是一种允许用户对自己的标记语言进行定义的源语言。 它非常适合万维网传输，提供统一的方法来描述和交换独立于应用程序或供应商的结构化数据。是Internet环境中跨平台的、依赖于内容的技术，也是当今处理分布式结构信息的有效工具。早在1998年，W3C就发布了XML1.0规范，使用它来简化Internet的文档信息传输。
* XML的作用:
&emsp;&emsp;XML 是各种应用程序之间进行数据传输的最常用的工具，并且在信息存储和描述领域变得越来越流行。简单的说，我们在开发中使用XML主要有以下两方面应用:
    * XML做为数据交换的载体，用于数据的存储与传输
    * XML做为配置文件
* <font color=orange>XML书写规范</font>, 我们将符合下述书写规则的XML叫做格式良好的XML文档, 可以通过浏览器浏览.
    * XML必须有根元素(只有一个)
    * XML标签必须有关闭标签
    * XML标签对大小写敏感
    * XML的属性值须加引号(单引号和双引号都可以)
    * 特殊字符必须转义
    * XML中的标签名不能有空格
    * 空格/回车/制表符在XML中都是文本节点
    * XML必须正确地嵌套
    * 后缀名 `.xml`
* <font color=orange>XML的组成部分</font>
    * 声明: 声明当前文件是一个xml文件.<font color=red>必须放在文档的第一行, 前面不能有空格</font>
        * 书写时要注意编码问题, 编码要统一
    >   <?xml version="1.0" encoding="UTF-8"?>
    * 元素
&emsp;&emsp;XML 元素指的是从（且包括）开始标签直到（且包括）结束标签的部分。元素可包含其他元素、文本或者两者的混合物。元素也可以拥有属性。
        * 标签必须结束。
            * `<a>内容</a>`或`<a/>`
        * 标签可以嵌套，但是必须合理嵌套
        * 标签名不能以`xml`的任意大小写字母开头
        * 标签名中不能出现`空格`或者`:`
    * 属性
        * 属性值必须使用引号引起来.
        * 在实际开发中, 标签的属性一般做为子元素存在.
    * 注释
        * `<!--注释-->`与html中一样.
    * CDATA区域
&emsp;&emsp;这个区域中的信息会按照原样输出，不会被解析器解析.CDATA 部分由 "<![CDATA[" 开始，由 "]]>" 结束,如果是一些特殊符号我们也可以使用转义字符.例如:&gt; &lt;等, 例如: `<![CDATA[<abc>]]>`会原样输出`<abc>`.
* <font color=orange>XML约束</font>
    * DTD约束, 一个XML只可以引入一个DTD约束
        * struts、hibernate使用
        * DTD和xml文件关联方式
            * 1、内部连接
                * 格式:  `<!DOCTYPE 文档根节点 [语法]>`
            * 2、外部连接-本地文件
                * 格式:  `<!DOCTYPE 文档根节点 SYSTEM  "DTD文件的本地URL">` 
            * 3、外部连接-网络文件
                * 格式:  `<!DOCTYPE 文档根节点 PUBLIC "DTD名称" "DTD文件的URL">` 
            * DTD语法 
                * 元素
                    * 格式1: `<!ELEMENT 元素名称 类别>`
                        * 类型中的`#PCDATA`使用时必须使用`()`括起来
                    * 格式2: `<!ELEMENT 元素名称 (元素内容)>`
                * 属性
                    * 格式: `<!ATTLIST 元素名称 属性名称 属性类型 默认值>`
    * SCHEMA约束, 一个XML可以引入多个SCHEMA约束
        * web项目、spring等使用
        * 格式 : 约束文件xsd中 `<schema xmlns="http://www.w3.org/2001/XMLSchema" 
targetNamespace="http://www.example.org/bookstore2"
elementFormDefault="qualified">`//xsd文件中的 xmlns是一个固定值,引用的官方规定的自定义schema文档如何编写,targetNamespace给当前的约束文档起个名字,便于xml文件引用(唯一).
        * 格式 : xml文件中引用schema约束: `<bookstore xmlns="http://www.example.org/bookstore2"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.example.org/bookstore2 bookstore2.xsd">`
xmlns:名称空间,和xsd中的targetNamespace保持一致, schemaLocation此属性有两个值。第一个值是需要使用的名称空间。第二个值是供名称空间使用的 XML schema 的位置，两者之间用空格分隔.
* <font color=orange>XML的解析</font>
    * dom解析: 一次性将文档加载进内存中, 形成dom树, 可以对其进行增删改查, 如果XML文件过大, 会内存溢出.
    (Document Object Model, 即文档对象模型) 是 W3C 组织推荐的解析XML 的一种方式。
    * sax解析: 逐行解析, 但是只能查询, 处理速度快.
    (Simple API for XML) 不是官方标准，但它是 XML 社区事实上的标准，几乎所有的 XML 解析器都支持它。
    * 常见XML解析包
        * JAXP: SUN公司提供的DOM和SAX开发包
        * JDom: dom4j
        * jsoup: 处理html的特定解析开发包
        * <font color=orange>dom4j(★)</font>: 比较常见的解析包, hibernate底层使用
            * 使用步骤
                * 1、导入`dom4j-xxxx.jar`包
                * 2、创建核心类`SAXReader`
                >   SAXReader saxReader = new SAXReader();
                * 3、将XML文档加载到内存中形成树
                >  Document document = saxReader.read(...);
                * 4、获取节点
                    * 4.1、获取根节点
                    >   Element root = document.getRootElement();
                    * 4.2、获取节点下所有子节点(不能跨节点).
                    >   List elements = root.elements();
                    //迭代
                    for (Iterator it = elements.iterator(); it.hasNext();) {
                    &emsp;Element elm = (Element) it.next();
                    }
                    * 4.3、获取节点下所有名为servlet的子节点(不能跨节点).
                    >   List servlets = root.elements("servlet");
                    * 4.4、获取节点下面一个名为servlet的子节点.
                    >   Element servlet = root.element("servlet");
                * 5、获取标签体里面的内容
                    * 5.1、获取子节点标签体里面的内容
                    >   String body = root.elementText("子节点名");
                    * 5.2、获取节点的标签体内容
                    >   调用节点的`getText()`或者`getValue()`方法
                * 6、获取属性和值
                    * 6.1、取得节点下所有属性
                    >   List attributes = servletName.attributes();
                    * 6.2、取得节点下指定属性名的属性
                    >   Attribute attribute = servletName.attribute("属性名");
                    * 6.3、获取属性的值
                        * 6.3.1、使用属性的方法
                        >   String value = attribute.getText();
                        //或者
                        String valye = attribute.getValue();
                        * 6.3.2、使用节点的方法
                        >   String value = attributeValue("属性名");
        * XPath解析技术
            * 它依赖于dom4j
            * 使用步骤
                * 1、导入`dom4j-xxxx.jar`和`jaxen-xxxx.jar`包
                * 2、加载XML到内存中
                * 3、`selectNodes("表达式")`和`selectSingleNode("表达式")`
                * 4、表达式写法: `/根节点名/子节点名/子节点名` 、`//子节点名/子节点名`、`//子节点名[@属性名='value']`
* <font color=orange>反射</font>
    * 获取Class文件
        * 1、常用: `Class clazz = Class.forName("全限定名");`
        * 2、`Class clazz = 类名.class;`
        * 3、`Class clazz = 对象.getClass();`
    * 通过Class对象创建实例
    >   Object o = clazz.getInstance(); //通过无参构造方法
    * 通过Class对象获取方法
    >   Method method = clazz.getMethod("方法名", paramType);//paramType是参数类型, 例如: `int.class`
    * 方法执行
    >   method.invoke(实例对象, 参数);

## <font color=orange>Tomcat</font>
静态的Web开发技术: HTML、CSS
动态的Web开发技术: Servlet、JSP、PHP、.net、ASP等
* CS与BS结构
    * C/S结构:即Client/Server (客户端/服务器) 结构，是大家熟知的软件系统体系结构，通过将任务合理分配到Client端和Server端，降低了系统的通讯开销，需要安装客户端才可进行管理操作。客户端和服务器端的程序不同，用户的程序主要在客户端，服务器端主要提供数据管理、数据共享、数据及系统维护和并发控制等，客户端程序主要完成用户的具体的业务。开发比较容易，操作简便，但应用程序的升级和客户端程序的维护较为困难。
    * B/S结构:即Browser/Server (浏览器/服务器) 结构，是随着Internet技术的兴起，对C/S结构的一种变化或者改进的结构。在这种结构下，用户界面完全通过WWW浏览器实现。客户端基本上没有专门的应用程序，应用程序基本上都在服务器端。由于客户端没有程序，应用程序的升级和维护都可以在服务器端完成，升级维护方便。
* <font color=orange>Web服务器</font>
    * WebLogic: 是美国Oracle公司出品的一个application serve, 它是一个大型的、收费的和支持JavaEE所有规范的服务器.
    * WebSphere: 是IBM 的软件平台,  它是一个大型的、收费的和支持JavaEE所有规范的服务器.
    * <font color=red>Tomcat(使用最多的服务器)</font>, 是Apache 软件基金会（Apache Software Foundation）的Jakarta 项目中的一个核心项目，由Apache、Sun 和其他一些公司及个人共同开发而成, 它是中小型的、免费的和支持Servlet、JSP规范的服务器.

* Tomcat的下载
&emsp;&emsp;[Tomcat官网](http://tomcat.apache.org/), tar.gz(zip)文件是Linux操作系统下的安装版本, exe文件是Windows系统下的安装版本, zip文件是Windows系统下的压缩版本(Windows系统下必须配置`JAVA_HOME`环境变量), 一般是Tomcat6或者7版本.
* <font color=orange>Tomcat的安装</font>
    * Mac下安装
        * 1、下载`.tar`或者`.tar.gz`格式的压缩包
        * 2、将解压后的文件夹放入`/Users/你的用户名`, 为了方便使用可以命名为`Tomcat`, 不推荐放到`/Library`目录下面(权限问题很麻烦)
        * 3、打开终端, 运行 `sudo chmod 755 /Users/你的用户名/Tomcat/bin/*.sh `, 输入电脑密码后即可修改文件夹的权限
        * 4、终端运行 `sudo sh /Users/你的用户名/Tomcat/bin/startup.sh` 可以运行Tomcat
        * 5、浏览器打开网址:[本地测试地址http://localhost:8080/](http://localhost:8080/), 如果出现Apache Tomcat/xxxx 即代表安装成功.
        * 6、终端运行 `sudo sh /Users/你的用户名/Tomcat/bin/shutdown.sh`即可关闭Tomcat.
    * Windows下安装
        * 需要首先配置`JAVA_HOME`的环境变量, 它的值就是JDK的安装路径
        * 运行`tomact目录/bin/startup.bat`即可
        * 关闭的几种方式
            * 运行 `sudo sh /Users/你的用户名/Tomcat/bin/startup.sh`
            * 关闭cmd
* <font color=orange>Tomcat的常见问题</font>
    * 运行`startup.bat`一闪而过
        * 解决方法: 没有正确配置`JAVA_HOME`系统变量, 它的值必须是JDK的安装目录. 
    * `Address already in use`
        * 默认端口是8080, 该端口被占用了, 可以修改配置文件, 在`tomcat目录/conf/server.xml`, `port`改为其他端口, 一般实际开发中可以改为<font color=red>`80`</font>端口.
    * 配置了`CATALINA_HOME`系统变量后, tomcat运行后都是配置目录的项目.
* <font color=orange>Tomcat的目录</font>
    * `bin`: 存放可执行程序
    * `conf`: 存放配置文件
    * `lib`: 存放tomcat和项目运行所需的jar包
    * `logs`: 存放日志, 查看`catalina.log`可以查看运行异常日志
    * `temp`: 临时文件目录
    * <font color=red>`webapps`(★)</font>: 存放项目的目录, 一个项目单独一个目录
        * `WEB-INF`(无法通过浏览器直接访问)
            * `lib`: 项目使用的jar包  
            * `classes`: 存放自定义Java文件生成的.class文件
            * `web.xml`: 当前项目的核心配置文件
    * <font color=red>`work`(★)</font>: 存放jsp文件在运行时产生的java和class文件
* <font color=orange>虚拟目录的映射(项目发布)</font>
    * 方式1(★)、将项目放到`tomcat目录/webapps/`中
    * 方式2、修改配置文件`tomcat目录/conf/server.xml`, 在host标签下添加`<Context path="/项目名称" docBase="项目的磁盘目录">`
    * 方式3、在`tomcat目录/conf/catalina/localhost`目录中新建一个`项目名称.xml`文件, xml文件中内容是`<Context path="/项目名称" docBase="项目的磁盘目录">`
* <font color=orange>Eclipse和Tomcat的整合</font>
`Preferences`------`Server`------`Runtime Envirconments`------`Add`------选择`tomcat版本`------`Next`------选择`tomcat安装目录和自己安装的JRE`------`Finish`
<br />
`Window`------`Show view`------`Other`------`Servers`------`Create a new server`------选择`tomcat版本`------`Finish`
<br />
双击`Tomcat v7.0 Server at localhost [Stopped Republish]`------`Server  Locations`------选择`Use Tomcat installation(takes control of Tomcat installation)`------修改`Deploy path`------`webapps`------`保存`
* <font color=orange>通过Eclipse发布发布项目</font>
    * 方式1: 项目------`右键`------`Run on Server`
    * 方式2: 配置Servers 
        * 1、新建动态的Web项目`Dynamic Web Project`, 然后`Dynamic web module version`选择`2.5`版本(3.0版本下没有`web.xml`文件)
        * 2、Java文件放在`Java Resources`中, 而网页文件放在`WebContent`中.
        * 3、Server窗口中`Tomcat v7.0 Server at localhost`------`右键`------`Add and Remove`------`Add`------`Finish`
        * 4、在Server窗口选中服务器, 有一个绿色的`运行`按钮
            * 如果出现`Several ports (8005, 80, 8009) required by Tomcat v7.0 Server at localhost are already in use ......`, 需要关闭Tomcat服务器, 或者修改Tomcat端口号.
            * Mac下如果出现 `Servlet.service() for servlet [jsp] in context with path [] threw exception [java.lang.IllegalStateException: No output folder] with root cause java.lang.IllegalStateException: No output folder`, 是因为`Tomcat安装目录/work/Catalina/localhost/` 这个目录没有被读写的权限.
                * 终端运行`sudo chmod 777 Tomcat目录/work/Catalina/localhost`
            * Mac下如果出现 ` Failed to open access log file [/Library/Tomcat/logs/localhost_access_log.xxx.txt]` 是因为`Tomcat安装目录/logs`没有读写权限.
                * 终端运行`sudo chmod 777 Tomcat目录/logs`
