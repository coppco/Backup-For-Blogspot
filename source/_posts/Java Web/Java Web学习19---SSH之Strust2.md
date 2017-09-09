---
layout: post
title: Java Web学习19---SSH之Struts2
comments: true
date: 2017-03-31 11:42:54
tags:
	- Java
	- Struts2
---
SSH（Struts，Spring，Hibernate） Struts进行流程控制，Spring进行业务流转，Hibernate进行数据库操作的封装。Struts2是一个基于MVC设计模式的Web应用框架，它本质上相当于一个servlet，在MVC设计模式中，Struts2作为控制器(Controller)来建立模型与视图的数据交互。
<!--more-->

## <font color=orange> Struts2 </font>

&emsp;&emsp;Struts 2以WebWork为核心，采用拦截器的机制来处理用户的请求，这样的设计也使得业务逻辑控制器能够与ServletAPI完全脱离开，它是一个基于MVC设计模式的Web层框架.

* Struts2的使用
	* 导入jar包
	* 在`web.xml`中进行配置
```
<filter>
	<filter-name>struts2</filter-name>
	<filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
</filter>
<filter-mapping>
	<filter-name>struts2</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
```
	* 编写Action类
		* 必须是public
		* 必须有返回值(String类型)
		* 方法名称可以是任意的, 但是不能有参数列表
	* 配置src目录中的`struts.xml`文件
```
<?xml version="1.0" encoding="UTF-8"?>
<!--约束-->
<!DOCTYPE struts PUBLIC
        "-//Apache Software Foundation//DTD Struts Configuration 2.3//EN"
        "http://struts.apache.org/dtds/struts-2.3.dtd">

<struts>
    <!--包结构-->
    <package name="default" namespace="/" extends="struts-default">
        <!--配置action, name是访问的action名, class是action的全限类名, method是里面的方法-->
        <action name="hello" class="com.coppco.action.HelloAction" method="sayHello"></action>
    </package>
</struts>
```
	* 页面的跳转
		* 方法的返回值是字符串, 需要返回一个字符串, 注意不能重复
		* 在`struts.xml`中配置
```
<action name="hello" class="com.coppco.action.HelloAction" method="sayHello">
	<!--配置页面跳转, name: 方法返回的字符串, type: 类型, 值填写需要跳转的页面-->
	<result name="ok" type="redirect">/login.jsp</result>
</action>
```

### <font color=orange> Struts2加载的配置文件 </font>
* 1、加载`org/apache/struts2/default.properties`
	* Struts2的一些默认值, 一般不修改
* 2、加载`struts-default.xml,struts-plugin.xml,struts.xml`
	* `struts-default.xml`: Struts2核心功能的配置（Bean、拦截器、结果类型等）, 一般不修改
	* `struts.xml`, 一般开发修改这个文件
* 3、加载自定义的`struts.properties`
* 4、加载`web.xml`
	* struts2的配置是固定写法, 一般不变.
* 5、在`struts.xml`、`struts.properties`和`web.xml`中可以配置常量, 但是一般是在`struts.xml`中配置, 后加载的文件会覆盖之前加载的配置

### <font color=orange> struts.xml配置文件的配置</font>
	
* `<package>标签`: 如果要配置`<Action>`的标签，那么必须要先配置`<package>`标签，代表的包的概念
	* 包含的属性
		* name					-- 包的名称，要求是唯一的，管理action配置
		* extends				-- 继承，可以继承其他的包，只要继承了，那么该包就包含了其他包的功能，一般都是继承struts-default
		* namespace				-- 名称空间，一般与`<action>`标签中的name属性共同决定访问路径（通俗话：怎么来访问action），常见的配置如下
			* namespace="/"		-- 根名称空间
			* namespace="/aaa"	-- 带有名称的名称空间
		* abstract				-- 抽象的。这个属性基本很少使用，值如果是true，那么编写的包是被继承的
* `<action>标签`
	* 代表配置action类，包含的属性
		* name			-- 和<package>标签的namespace属性一起来决定访问路径的
		* class			-- 配置Action类的全路径（默认值是ActionSupport类）
		* method		-- Action类中执行的方法，如果不指定，默认值是execute	
* `<result>标签`
	* action类中方法执行，返回的结果跳转的页面
		* name		-- 结果页面逻辑视图名称, 方法的返回值
		* type		-- 结果类型（默认值是转发，也可以设置其他的值）


### <font color=orange> 常量的配置</font>
`struts.xml`、`struts.properties`和`web.xml`都可以配置常量, 但是一般在`struts.xml`中配置.并且后加载的配置文件会覆盖之前的常量.

```
<!--struts.xml中配置常量-->
<constant name="struts.action.extension" value="do,,"></constant> 
<struts>
    <!--包结构-->
    <package name="default" namespace="/" extends="struts-default">
        <!--配置action, name是访问的action名, class是action的全限类名, method是里面的方法-->
        <action name="hello" class="com.coppco.action.HelloAction" method="sayHello"></action>
    </package>
</struts>
```

### <font color=orange>多个struts的配置文件</font>
```
<struts>
	<!--file: 需要包含报名com/coppco/user/struts-part1.xml-->
	<include file="struts-part1.xml"/>
	<include file="struts-part2.xml"/>
</struts>
```

### <font color=orange>Action类的三种写法</font>
* Action类就是一个POJO类
	* 什么是POJO类，POJO（Plain Ordinary Java Object）简单的Java对象.简单记：没有继承某个类，没有实现接口，就是POJO的类。
	* Action类可以实现Action接口
		* Action接口中定义了5个常量，5个常量的值对应的是5个逻辑视图跳转页面（跳转的页面还是需要自己来配置），还定义了一个方法，execute方法。
		* 大家需要掌握5个逻辑视图的常量
			* SUCCESS		-- 成功.
			* INPUT			-- 用于数据表单校验.如果校验失败,跳转INPUT视图.
			* LOGIN			-- 登录.
			* ERROR			-- 错误.
			* NONE			-- 页面不转向.
	* Action类可以去继承ActionSupport类（<font color=red>开发中这种方式使用最多</font>）
		* 设置错误信息

### <font color=orange> Action的访问</font>
	
* 通过<action>标签中的method属性，访问到Action中的具体的方法。
	* 传统的配置方式，配置更清晰更好理解！但是扩展需要修改配置文件等！
	* 具体的实例如下：
		* 页面代码
			* `<a href="${pageContext.request.contextPath}/addBook.action">添加图书</a>`
			* `<a href="${pageContext.request.contextPath}/deleteBook.action">删除图书</a>`
		* 配置文件的代码
```
<package name="demo2" extends="struts-default" namespace="/">
    <action name="addBook" class="cn.itcast.demo2.BookAction" method="add"></action>
    <action name="deleteBook" class="cn.itcast.demo2.BookAction" method="delete">
    </action>
</package>
```
		* Action的代码
```
public String add(){
    System.out.println("添加图书");
    return NONE;
}
public String delete(){
    System.out.println("删除图书");
    return NONE;
}
```
* 通配符的访问方式:(访问的路径和方法的名称必须要有某种联系.)	通配符就是 `*` 代表任意的字符
	* 使用通配符的方式可以简化配置文件的代码编写，而且扩展和维护比较容易。
	* 具体实例如下：
		* 页面代码
```
<a href="${pageContext.request.contextPath}/order_add.action">添加订单</a>
<a href="${pageContext.request.contextPath}/order_delete.action">删除订单</a>
```
		* 配置文件代码
			* `<action name="order_*" class="cn.itcast.demo2.OrderAction" method="{1}"></action>`	
		* Action的代码
```
public String add(){
	System.out.println("添加订单");
	return NONE;
}
public String delete(){
	System.out.println("删除订单");
	return NONE;
}
```
	* 具体理解：在JSP页面发送请求，`http://localhost/struts2_01/order_add.action`，配置文件中的`order_*`可以匹配该请求，`*`就相当于变成了add，method属性的值使用{1}来代替，{1}就表示的是第一个`*`号的位置！！所以method的值就等于了add，那么就找到Action类中的add方法，那么add方法就执行了！
	
* 动态方法访问的方式（有的开发中也会使用这种方式）
	* 如果想完成动态方法访问的方式，需要开启一个常量，struts.enable.DynamicMethodInvocation = false，把值设置成true。
		* 注意：不同的Struts2框架的版本，该常量的值不一定是true或者false，需要自己来看一下。如果是false，需要自己开启。
		* 在struts.xml中开启该常量。
			* `<constant name="struts.enable.DynamicMethodInvocation" value="true"></constant>`
	* 具体代码如下
		* 页面的代码
			* `<a href="${pageContext.request.contextPath}/product!add.action">添加商品</a>`
			* `<a href="${pageContext.request.contextPath}/product!delete.action">删除商品</a>`
		* 配置文件代码
			* `<action name="product" class="cn.itcast.demo2.ProductAction"></action>`
		* Action的类的代码
```
public class ProductAction extends ActionSupport{
    public String add(){
        System.out.println("添加订单");
        return NONE;
    }
    public String delete(){
        System.out.println("删除订单");
        return NONE;
    }
}
```

### <font color=orange>Struts2框架中使用Servlet的API</font>
* 完全解耦合的方式
	* 如果使用该种方式，Struts2框架中提供了一个类，ActionContext类，该类中提供一些方法，通过方法获取Servlet的API
	* 一些常用的方法如下
		* static ActionContext getContext()  										-- 获取ActionContext对象实例
		* java.util.Map<java.lang.String,java.lang.Object> getParameters()  		-- 获取请求参数，相当于request.getParameterMap();Map中Object是一个数组.
		* java.util.Map<java.lang.String,java.lang.Object> getSession()  			-- 获取的代表session域的Map集合，就相当于操作session域。
		* java.util.Map<java.lang.String,java.lang.Object> getApplication() 		-- 获取代表application域的Map集合
		* void put(java.lang.String key, java.lang.Object value)  					-- 注意：向request域中存入值。
    
* 使用原生Servlet的API的方式
	* Struts2框架提供了一个类，ServletActionContext，该类中提供了一些静态的方法
	* 具体的方法如下
		* getPageContext()
		* getRequest()
		* getResponse()
		* getServletContext()
	
### <font color=orange>Struts2框架的数据封装</font>
* 第一种方式：属性驱动
	* 提供对应属性的set方法进行数据的封装。
		* 表单的哪些属性需要封装数据，那么在对应的Action类中提供该属性的set方法即可。
		* 表单中的数据提交，最终找到Action类中的setXxx的方法，最后赋值给全局变量。
				
		* 注意0：Struts2的框架采用的拦截器完成数据的封装。
		* 注意1：这种方式不是特别好:因为属性特别多,提供特别多的set方法,而且还需要手动将数据存入到对象中.
		* 注意2：这种情况下，Action类就相当于一个JavaBean，就没有体现出MVC的思想，Action类又封装数据，又接收请求处理，耦合性较高。
			
	* 在页面上，使用OGNL表达式进行数据封装。
		* 在页面中使用OGNL表达式进行数据的封装，就可以直接把属性封装到某一个JavaBean的对象中。
		* 在页面中定义一个JavaBean，并且提供set方法：例如：private User user;
		* 页面中的编写发生了变化，需要使用OGNL的方式，表单中的写法：`<input type="text" name="user.username">`
				
		* 注意：只提供一个set方法还不够，必须还需要提供user属性的get和set方法！！！
			* 先调用get方法，判断一下是否有user对象的实例对象，如果没有，调用set方法把拦截器创建的对象注入进来，
		
* 第二种方式：模型驱动
	* 使用模型驱动的方式，也可以把表单中的数据直接封装到一个JavaBean的对象中，并且表单的写法和之前的写法没有区别！
	* 编写的页面不需要任何变化，正常编写name属性的值
	*  模型驱动的编写步骤：
		* 手动实例化JavaBean，即：private User user = new User();
		* 必须实现ModelDriven<T>接口，实现getModel()的方法，在getModel()方法中返回user即可！！

### <font color=orange>Struts2的拦截器</font>
* 拦截器的概述
	* 拦截器就是AOP（Aspect-Oriented Programming）的一种实现。（AOP是指用于在某个方法或字段被访问之前，进行拦截然后在之前或之后加入某些操作。）
	* 过滤器:过滤从客服端发送到服务器端请求的	
	* 拦截器:拦截对目标Action中的某些方法进行拦截
		* 拦截器不能拦截JSP
		* 拦截到Action中某些方法
* 拦截器和过滤器的区别
	* 1）拦截器是基于JAVA反射机制的，而过滤器是基于函数回调的
	* 2）过滤器依赖于Servlet容器，而拦截器不依赖于Servlet容器
	* 3）拦截器只能对Action请求起作用（Action中的方法），而过滤器可以对几乎所有的请求起作用（CSS JSP JS）	
	* 拦截器 采用 责任链 模式
		* 在责任链模式里,很多对象由每一个对象对其下家的引用而连接起来形成一条链
		* 责任链每一个节点，都可以继续调用下一个节点，也可以阻止流程继续执行
	* 在struts2 中可以定义很多个拦截器，将多个拦截器按照特定顺序 组成拦截器栈 （顺序调用 栈中的每一个拦截器 ）
* 自定义拦截器
	* 编写拦截器，需要实现Interceptor接口，实现接口中的三个方法
```
protected String doIntercept(ActionInvocation invocation) throws Exception {
    // 获取session对象
    User user = (User)ServletActionContext.getRequest().getSession().getAttribute("existUser");
        if(user == null){
            // 说明，没有登录，后面就不会执行了
            return "login";
        }
        return invocation.invoke();
}
```
	* 需要在`struts.xml`中进行拦截器的配置，配置一共有两种方式
```
		<!-- 定义了拦截器 第一种方式
		<interceptors>
			<interceptor name="DemoInterceptor" class="com.itheima.interceptor.DemoInterceptor"/>
		</interceptors>
		-->
		
		<!-- 第二种方式：定义拦截器栈 -->
		<interceptors>
			<interceptor name="DemoInterceptor" class="com.itheima.interceptor.DemoInterceptor"/>
			<!-- 定义拦截器栈 -->
			<interceptor-stack name="myStack">
				<interceptor-ref name="DemoInterceptor"/>
				<interceptor-ref name="defaultStack"/>
			</interceptor-stack>
		</interceptors>
		
		<action name="userAction" class="com.itheima.demo3.UserAction">
			<!-- 只要是引用自己的拦截器，默认栈的拦截器就不执行了，必须要手动引入默认栈 
			<interceptor-ref name="DemoInterceptor"/>
			<interceptor-ref name="defaultStack"/>
			-->
			
			<!-- 引入拦截器栈就OK -->
			<interceptor-ref name="myStack"/>
		</action>
```