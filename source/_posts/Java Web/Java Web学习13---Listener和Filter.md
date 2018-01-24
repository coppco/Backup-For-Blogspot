---
layout: post
title: Java Web学习13---Listener和Filter
comments: true
date: 2016-10-18 11:43:10
tags:
	- Java
	- Listener
	- Filter
---
Listener是监听者, 可以用来监听HttpServletRequest,HttpSession,ServletContext对象.
Filter是过滤器, 可以拦截访问web资源的请求与响应操.

## <font color=orange> Listener </font>
* 监听web对象创建与销毁的监听器
	* ServletContextListener: 监听ServletContext对象的创建与销毁
		* 创建
			* 服务器开启时创建
		* 销毁
			* 服务器关闭时销毁。
	* HttpSessionListener
		* 创建
			* 取决于请求中是否有jsessinid，如果有，可能会获取一个已经存在的session对象。如果没有，会创建一个新的session对象.
		* 销毁
			* 超时(默认超时30分钟或者通过setMaxInactiveInterval(int)手动设置超时时间)
			* 关闭服务器
			* invalidate()方法
	* ServletRequestListener	
		* 创建
			* 发送请求时创建
		* 销毁
			* 当响应产生时，销毁.
	* <font color=orange>使用步骤</font>
		* 1、创建一个类，实现指定的监听器接口
		* 2、重写接口中的方法.
		* 3、在web.xml文件中配置监听
		>	<listener&gt;
		&emsp;&emsp;<listener-class&gt;全限定名</listener-class&gt;
		</listener&gt;		
* 监听web对象属性变化
	* ServletContextAttributeListener
	* HttpSessionAttributeListener
	* ServletRequestAttributeListener	
	* <font color=orange>通用方法</font>
		* void attributeAdded(...)
		* void attributeRemoved(...)
		* void attributeReplaced(...)
	* <font color=orange>使用步骤</font>
		* 1、创建一个类，实现指定的监听器接口
		* 2、重写接口中的方法.
		* 3、在web.xml文件中配置监听
		>	<listener&gt;
		&emsp;&emsp;<listener-class&gt;全限定名</listener-class&gt;
		</listener&gt;	
* 监听session绑定javaBean, 它们是由JavaBean实现接口, 无需在web.xml中配置
	* HttpSessionBindingListener: 使javaBean对象在被绑定到会话或从会话中取消对它的绑定时得到通知
	>	public void valueBound(HttpSessionBindingEvent event) {
	}
	public void valueUnbound(HttpSessionBindingEvent event) {
	}
	* HttpSessionActivationListener(活化或者钝化): 绑定到会话的对象可以侦听通知它们会话将被钝化和会话将被激活的容器事件
		* 钝化: 当我们正常关闭服务器时，session对象会被保存到服务器文件中, 可以节省服务器的内存.
		* 活化: 如果之前进行了钝化操作，当正常启动服务器时会从文件中将session读取出来使用
		* <font color=orange>使用步骤</font>
			* 1、创建一个类，实现指定的监听器接口
			* 2、重写HttpSessionActivationListener接口中的方法.
			* 3、JavaBean类实现Serializable接口
			* 4、可以在`项目名/WebContent/META-INF`下新建一个`context.xml`配置文件
			>	<Context&gt;
			&emsp;&emsp;<Manager className="org.apache.catalina.session.PersistentManager" maxIdleSwap="1"&gt;
			&emsp;&emsp;&emsp;&emsp;<Store className="org.apache.catalina.session.FileStore" directory="home"/&gt;
		&emsp;&emsp;</Manager&gt;
		</Context&gt;
		//maxIdleSwap: 单位分钟, 多久不用就保存到磁盘
		//directory: 序列化的硬盘路径
		//通过以上配置会在tomcat的work目录下新建一个名称为home的文件夹.
		
## <font color=orange> Filter </font>
* 作用
	* 通过Filter可以拦截访问web资源的请求与响应操作.
	* 通过Filter技术，对web服务器管理的所有web资源：例如Jsp, Servlet, 静态图片文件或静态 html 文件等进行拦截，从而实现一些特殊的功能。例如实现URL级别的权限访问控制、过滤敏感词汇、压缩响应信息等一些高级功能。
* 实际应用
	* 自动登录
	* 统一编码
	* 内容过滤等
* <font color=orange>常用方法</font>
	* 销毁操作: `void	destroy()`
	* 初始化操作: `void	init(FilterConfig filterConfig)` 
	* 处理请求: `void	doFilter(ServletRequest request, ServletResponse response, FilterChain chain)`
* <font color=orange>使用步骤</font>
	* 1、创建一个类, 实现接口`javax.servlet.Filter`
	* 2、重写接口方法
		* doFilter(...)
			* <font color=red>在Filter的doFilter方法内如果没有执行`chain.doFilter(request,response)`,那么资源是不会被访问到的。</font>
	* 3、注册filter, 在`web.xml`中添加
	>	<filter&gt;
	<filter-name&gt;demo1Filter</filter-name&gt;
	<filter-class&gt;全限定名</filter-class&gt;
	</filter&gt;
	* 4、绑定路径, <font color=red>`<filter-mapping>`里面的子标签可以通过`url-pattern`标签来配置, 也可以使用`servlet-name`标签来配置</font>
	>	<filter-mapping&gt;
	<filter-name&gt;demo1Filter</filter-name&gt;
	//通过`url-pattern`来拦截
	<url-pattern&gt;/demo1</url-pattern&gt;
	//也可以通过`sevelt-name`标签来指定拦截哪个servlet
	<servlet-name&gt;</servlet-name&gt;
	//<dispatcher&gt;可以配置拦截方式, 取值有 REQUEST  FORWARD  ERROR  INCLUDE, 默认是REQUEST.
	<dispatcher&gt;REQUEST</dispatcher&gt;
	</filter-mapping&gt;
	* dispatcher的取值
		* REQUEST: 当是从浏览器直接访问资源，或是重定向到某个资源时进行拦截方式配置的 它也是默认值
		* FORWARD: 它描述的是请求转发的拦截方式配置
		* ERROR: 如果目标资源是通过声明式异常处理机制调用时，那么该过滤器将被调用。除此之外，过滤器不会被调用。
		* INCLUDE: 如果目标资源是通过RequestDispatcher的include()方法访问时，那么该过滤器将被调用。除此之外，该过滤器不会被调用
* <font color=orange>Filter链与生命周期</font>
	* Filter链
		* 多个Filter对同一个资源进行了拦截，那么当我们在开始的Filter中执行	chain.doFilter(request,response)时，是访问下一下Filter,直到最后一个Filter执行时，它后面没有了Filter,才会访问web资源。
	* Filter链的执行顺序
		* 它们的执行顺序取决于<filter-mapping>在web.xml文件中配置的先后顺序。
	* Filter生命周期
		* 创建: 当服务器启动，会创建Filter对象，并调用init方法，只调用一次.
		* 当访问资源时: 路径与Filter的拦截路径匹配，会执行Filter中的doFilter方法，这个方法是真正拦截操作的方法.
		* 销毁: 当服务器关闭时，会调用Filter的destroy方法来进行销毁操作.