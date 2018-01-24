---
layout: post
title: Java Web学习08---HTTP、Servlet
comments: true
date: 2016-09-01 06:20:53
tags:

	- Java
	- HTTP
	- Servlet
---
HTTP协议：超文本传输协议（HTTP，HyperText Transfer Protocol)是互联网上应用最为广泛的一种网络协议。所有的WWW文件都必须遵守这个标准。
Servlet 是在服务器上运行的小程序。Servlet 的主要功能在于交互式地浏览和修改数据，生成动态 Web 内容。
<!--more-->

## <font color=orange>HTTP</font>
HTTP协议：超文本传输协议（HTTP，HyperText Transfer Protocol)是互联网上应用最为广泛的一种网络协议。所有的WWW文件都必须遵守这个标准。
HTTP协议规定 浏览器(客户端)向服务器发送 何种格式的数据. 服务器 会处理数据. 向浏览器(客户端)作出响应.(向客户端发送何种格式的数据)

* HTTP协议的特点:
	* HTTP协议遵守一个请求响应模型.
  	  * 请求和响应必须成对出现.
  	  * 必须先有请求后有响应.
	* HTTP协议默认的端口:80
* <font color=orange>HTTP请求</font>
	* 组成部分
		* 请求行，请求信息的第一行
			* 格式： 空格 请求方式 访问url 协议/版本
			* 例如：  `get https://www.baidu.com HTTP/1.1`
			* 请求方式：常用的有GET、POST
				* get会把参数放到url后； 而post放在请求体中。
				* get参数大小有限制； post请求却没有限制。
				* get请求没有请求体； post请求有请求体，请求参数放在请求体中。
		* 请求头，请求信息的第二行到空行结束
			* 格式： key/value（一个key可以对应多个value）
			* 常见的请求头
				* <font color=orange>Accept(★)</font>: text/html,image/*    --支持数据类型
					* text/html、text/css等mime类型
				* Accept-Charset: ISO-8859-1   --字符集
				* Accept-Encoding: gzip		--支持压缩
				* Accept-Language:zh-cn 	--语言环境
				* Host: www.baidu.com:80	--访问主机
				* If-Modified-Since: Tue, 11 Jul 2000 18:23:51 GMT  --缓存文件的最后修改时间
				* <font color=orange>Referer(★)</font>: http://www.baidu.com/index.jsp  --来自哪个页面、防盗链
				* <font color=orange>User-Agent(★)</font>: Mozilla/4.0 (compatible; MSIE 5.5; Windows NT 5.0)  --发出请求的用户信息
				* <font color=orange>Cookie(★)</font> --缓存
				* Connection: close/Keep-Alive   --链接状态
				* Date: Tue, 11 Jul 2000 18:23:51 GMT--时间
		* 请求体
			* 只有post请求才有请求体， get请求参数直接拼接到url后
* <font color=orange>HTTP响应</font>
    * 组成部分
        * 响应行，请求信息的第一行
        	* 格式: `协议/版本 状态码 状态说明`
        	* 例如: `HTTP/1.1 200 OK`
        		* 常见状态码
        			* 200 ---> 正常响应成功
        			* 302 ---> 重定向
        			* 304 ---> 没有修改,读缓存
        			* 404 ---> 用户操作资源不存在
        			* 500 ---> 服务器内部异常
        * 响应头，请求信息的第二行到空行结束
        	* 格式： key/value（一个key可以对应多个value）
        	* 常见的响应头
        		* <font color=orange>Location(★)</font>: --跳转方向
        		* Server:apache tomcat	 --服务器型号
				* Content-Encoding: gzip 	--数据压缩
				* Content-Length: 80 		--数据长度
				* Content-Language: zh-cn 	--语言环境
				* <font color=orange>Content-Type(★)</font>: text/html; charset=GB2312 --响应数据类型
				* <font color=orange>Last-Modified(★)</font>: Tue, 11 Jul 2000 18:23:51 GMT --最后修改时间
				* <font color=orange>Refresh(★)</font>: 1;url=http://www.baidu.com --定时刷新, 后面是跳转的url地址.
				* <font color=orange>Content-Disposition(★)</font>:attachment;filename=aaa.zip   --下载
				* <font color=orange>Set-Cookie(★)</font>:SS=Q0=5Lb_nQ; path=/search
				* Expires: -1					--缓存(适配不同浏览器)
				* Cache-Control: no-cache  --缓存(适配不同浏览器)
				* Pragma: no-cache   		--缓存(适配不同浏览器)
				* Connection: close/Keep-Alive   --是否保持连接
				* Date: Tue, 11 Jul 2000 18:23:51 GMT --时间
        * 响应体
        
## <font color=orange>Servlet</font>
&emsp;&emsp;Servlet 是在服务器上运行的小程序。Servlet 的主要功能在于交互式地浏览和修改数据，生成动态 Web 内容。

* Servlet、客户端和服务器通讯过程
	* 1、客户端发送请求至服务器端；
	* 2、服务器将请求信息发送至 Servlet；
	* 3、Servlet 生成响应内容并将其传给服务器。响应内容动态生成，通常取决于客户端的请求；
	* 4、服务器将响应返回给客户端。
	
&emsp;&emsp;一个 Servlet 就是 Java语言中的一个类，它被用来扩展服务器的性能，服务器上驻留着可以通过“请求-响应”编程模型来访问的应用程序。虽然 Servlet 可以对任何类型的请求产生响应，但通常只用来扩展 Web 服务器的应用程序。
&emsp;&emsp;servlet其实就是一个java类，这个类继承了HttpServlet，注意，Javax.servlet.http.HttpServlet类，是一个抽象类是,它不在我们的jdk中，所以使用时我们需要单独导入这个jar包，我们可以在tomcat中的lib下找到这个包, 路径是`/Tomcat安装目录/lib/servlet-api.jar`,而对于HttpServlet的子类，一般需要重写以下方法。

* 继承HttpServlet需要重写的方法
	* doGet，如果 servlet 支持 HTTP GET 请求 
	* doPost，用于 HTTP POST 请求 
	* init 和 destroy，用于管理 servlet 的生命周期内保存的资源 
	* getServletInfo，servlet 使用它提供有关其自身的信息 
	* getServletConfig, 获得任何启动信息
* <font color=orange>Servlet的基本使用</font>
	* 1、新建动态Web项目(Dynamic Web Project)
	* 2、使用MyEclipse会自动导入相关的lib, 而Eclipse需要导入`servlet-api.jar`包
	* 3、新建一个Java类继承于HttpServlet(权限类名是javax.servlet.http.HttpServlet)
		* 使用Eclipse新建类型为`Servlet`的类时, 可以自动实现下面4、5两步, 但是它不会随着Servlet删除、修改而自动配置.
	* 4、重写doGet或者doPost函数
		* doGet函数, 使用GET方法请求时响应
		
		>	@Override
		protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		&emsp;&emsp;// TODO Auto-generated method stub
		&emsp;&emsp;System.out.println("Hello Servlet!");
		}
		
		* doPost函数, 使用POST方法请求时响应
		
		>	@Override
		protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		&emsp;&emsp;// TODO Auto-generated method stub
		&emsp;&emsp;super.doPost(req, resp);
		}
		
	* 5、配置`web.xml`文件
		* 5.1、注册servlet
		
		>	servlet标签: 注册servlet使用
		servlet-name标签: 给servlet起个名字, 全局唯一
		servlet-class标签: 存放servlet的权限类名
		load-on-startup标签: 修改servlet初始化时机, 正整数, 值越小越先初始化.
		//例如
		<servlet&gt;
 	 	&emsp;&emsp;<servlet-name&gt;HelloServlet</servlet-name&gt;
  		&emsp;&emsp;<servlet-class&gt;com.coppco.test.HelloServlet</servlet-class&gt;
  		</servlet&gt;
  		
  	* 5.2、绑定路径, 一个servlet可以绑定多个`url-pattern`
		>	servlet-mapping标签: 绑定路径
		servlet-name标签: 使用上面已经起好的名字
		url-pattern标签: 访问路径, 目前要求必须以`/`开头, 全局唯一
		//例如	  
  		<servlet-mapping&gt;
  		&emsp;&emsp;<servlet-name&gt;HelloServlet</servlet-name&gt;
  		&emsp;&emsp;<url-pattern&gt;/hello</url-pattern&gt;
  		</servlet-mapping&gt;
  		<servlet-mapping&gt;
  		&emsp;&emsp;<servlet-name&gt;HelloServlet</servlet-name&gt;
  		&emsp;&emsp;<url-pattern&gt;/hello123</url-pattern&gt;
  		</servlet-mapping&gt;
  		
  		* <font color=orange>url-pattern几种匹配方式</font>:
  			* 完全匹配,要求必须以"/"开始
  			* 目录匹配,要求必须以"/"开始，以"\*"结束
  			* 扩展名匹配, 要求不能以"/"开始, 要以\*.xxx结束,xxx代表的是后缀名
  			* 优先级: 完全匹配>目录匹配>扩展名匹配
  			* Tomcat有一个默认的servlet, 用以处理匹配不到的servelt路径.
  * 6、访问
  		* `项目`右键----`Run on Server`或者`Servers窗口`中`Add and Remove`中添加项目
  		* 使用`http://主机:端口/项目名/访问路径`即可访问.
  
* <font color=orange>HttpServletRequest</font>
	* doGet和doPost函数有一个参数是`HttpServletRequest`类, 它用于服务端接受参数
		* HttpServletRequest类常用方法
			* 根据key获取valye: `String parameter = req.getParameter("key");`

* <font color=orange>HttpServletResponse</font>
	* doGet和doPost函数有一个参数是`HttpServletResponse`类, 它用于服务端发送响应到客户端
		* HttpServletResponse类常用方法
			* 设置响应数据类型是text/html, 编码为utf-8:  `resp.setContentType("text/html;charset=utf-8");`
			* 回写数据 : `resp.getWriter().println("返回数据" + username + ":" + password);`
				* 回写中文必须设置ContentType中的`charset=utf-8`, 不然会乱码
* <font color=orange>Servlet生命周期</font>
	* void init(ServletConfig config): 初始化
		* 在第一次请求访问时只执行一次
	* void service(ServletRequest request, ServletResponse response): 处理业务逻辑
		* 请求一次执行一次
	* void destory(): 销毁
		* 当servlet被移除或服务器正常关闭时只执行一次
	* Servlet是一个单实例多线程, 默认第一次访问的时候, 服务器创建servlet, 调用init()方法实现初始化,并调用service(), 每当请求来的时候, 服务器都会创建一个线程, 调用service()处理逻辑.当servlet被移除或服务器正常关闭时, 服务器调用destory()方法实现销毁操作.
* <font color=orange>路径的写法</font>
	* 相对路径
		* 当前路径: `./`
		* 上级路径: `../`
	* 绝对路径
		* 带主机和协议的绝对路径(访问站外资源时使用): `https://www.baidu.com`
		* 不带主机和协议的绝对路径(经常使用): `/Service/login`
* <font color=orange> ServletContext </font>
WEB容器在启动时，它会为每个WEB应用程序都创建一个对应的ServletContext对象，它代表当前web应用。ServletConfig对象中维护了ServletContext对象的引用，开发人员在编写servlet时，可以通过ServletConfig.getServletContext方法获得ServletContext对象。由于一个WEB应用中的所有Servlet共享同一个ServletContext对象，因此Servlet对象之间可以通过ServletContext对象来实现通讯。ServletContext对象通常也被称之为context域对象。
通过`ServletContext context=this.getServletContext();`获取
* ServlectContext作用
	* 获取全局初始化参数
		* String getInitParameter(String name)
		* Enumeration getInitParameterNames()
	* 实现servlet共享资源
		* Object getAttribute(String name),返回具有给定名称的 servlet 容器属性，如果不具有该名称的属性，则返回 null。
		* void setAttribute(String name,Object object) ,将对象绑定到此 servlet 上下文中的给定属性名称。如果已将指定名称用于某个属性，则此方法将使用新属性替换具有该名称的属性。 
		* void removeAttribute(String name) ,从 servlet 上下文中移除具有给定名称的属性。 
	* 获取web资源
		* String getRealPath(String path)  为给定虚拟路径返回包含实际路径的 String(返回的路径到项目名称)
		* InputStream getResourceAsStream(String path) 以 InputStream 对象的形式返回位于指定路径上的资源, 可以用于文件下载
	* 其它操作
		* String getMimeType(String file)   可以获取一个文件的mimeType类型.
		* URL getResource(String path) 它返回的是一个资源的URL
		* 还提供 log(String msg),getRequestDispatcher(String path) 等这样的方法，可以做
* <font color=orange> 获取文件路径 </font>
	* Class获取
		* Class.getResource("/").getPath();获取classes目录的绝对磁盘路径
		* Class.getResource("").getPath();获取的是当前Class对象代表的类所在的包的路径。
	* ClassLoader获取
		* Class.getClassLoader().getResource("/文件名").getPath(); 获取的是classes目录的绝对磁盘路径
		* Class.getClassLoader().getResource("文件名").getPath(); 获取的是classes目录的绝对磁盘路径