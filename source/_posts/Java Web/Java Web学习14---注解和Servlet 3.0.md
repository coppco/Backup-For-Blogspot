---
layout: post
title: Java Web学习14---注解和Servlet 3.0
comments: true
date: 2017-02-15 17:12:28
tags:
	- Java
	- 注解
	- Servlet3.0
---

&emsp;&emsp;Java5.0版本引入注解之后，它就成为了Java平台中非常重要的一部分。开发过程中，我们也时常在应用代码中会看到诸如@Override，@Deprecated这样的注解。

<!--more-->

## <font color=orange> 注解 </font>
&emsp;&emsp;注解是元数据，即一种描述数据的数据, 所以，可以说注解就是源代码的元数据。
例如: 下面使用了`@Override`注解

```
@Override
public String toString() {
    return "This is String Representation of current object.";
}
```

* <font color=orange>JDK中提供的注解</font>
	* 描述方法的重写: `@Override`
	* 消除警告: `@SuppressWarnings`
		* 消除未使用警告: `@SuppressWarnings("unused")`
		* 消除两个警告(未使用和没有泛型): `@SuppressWarnings({"unused", "rawtypes"})`
		* <font color=red>消除所有警告</font>: `@SuppressWarnings("all")`
	* 标记过时: `@Deprecated`
* <font color=orange>自定义注解, 它本质上是一个接口, 继承于`java.lang.annotation.Annotation`</font>
	* 格式: 
```
@interface TODO {
  public enum Priority {LOW, MEDIUM, HIGH}
  public enum Status {STARTED, NOT_STARTED} 
  String author() default "xxxx";
  Priority priority() default Priority.LOW;
  Status status() default Status.NOT_STARTED;
}
```

	* 自定义注解中可以有常量和抽象方法, Annotations只支持<font color=orange>基本类型、String、Class类型、注解类型(Annotation)、枚举类型以及以上类型的一维数组.</font>
		* 抽象方法在注解中称之为注解属性
	* 注解使用时如: `@TODO(priority = TODO.Priority.MEDIUM, author = "xxxx", status = TODO.Status.STARTED)`
		* 一旦注解有了属性, 使用时必须赋值, 有默认值的属性可以不赋值.
		* 数组的写法例如: `{"unused","rawtypes"}`
	* 如果注解中只有一个属性，可以直接命名为“value”，使用时无需再标明属性名, 如: `@Author("xxxx")`
```
@interface Author{
  String value();
}
@Author("xxxx")
public void someMethod() {
}
```
* <font color=orange>元注解, 专门注解其他的注解</font>
	* `@Documented`: –注解是否将包含在JavaDoc中
		* 表示是否将注解信息添加在java文档中。
	* <font color=orange>`@Retention`</font>: 定义该注解的生命周期。
		* RetentionPolicy.SOURCE: 在编译阶段丢弃。这些注解在编译结束之后就不再有任何意义，所以它们不会写入字节码。@Override, @SuppressWarnings都属于这类注解。
		* RetentionPolicy.CLASS: 在类加载的时候丢弃。在字节码文件的处理中有用。注解默认使用这种方式。
		* <font color=orange>RetentionPolicy.RUNTIME</font>: 始终不会丢弃，运行期也保留该注解，因此可以使用反射机制读取该注解的信息。我们自定义的注解通常使用这种方式。
	* <font color=orange>`@Target`</font>: 注解用于什么地方,如果不明确指出，该注解可以放在任何地方.
		* ElementType.TYPE: 用于描述类、接口或enum声明
		* ElementType.FIELD: 用于描述实例变量
		* ElementType.METHOD: 用于方法上
		* ElementType.PARAMETER: 
		* ElementType.CONSTRUCTOR
		* ElementType.LOCAL_VARIABLE
		* ElementType.ANNOTATION_TYPE 另一个注释
		* ElementType.PACKAGE 用于记录java文件的package信息
	* `@Inherited`: 是否允许子类继承该注解

## <font color=orange> Servlet3.0 </font>

* Servlet3.0 与 Servlet2.5的区别：
	* Servlet3.0需要运行在tomcat7以上的服务器中.
	* Servlet3.0以后web.xml就不是必须的, 使用注解开发.
	* Servlet3.0支持注解开发.
		* 例如servlet: `@WebServlet("/testServlet")` 
	* Servlet3.0支持文件上传.
* Servlet 3.0文件上传
	* 客户端要求
		* 使用post请求
		* 设置`enctype="multipart/form-data"`
	* 服务端要求
		* 设置注解: `@MultipartConfig`
		* 普通参数: `public String getParameter(String name);`
		* 文件上传的参数: `public Part getPart(String name);`
	* javax.servlet.http.Part
		* 获取文件上传的输入流: `public InputStream getInputStream() throws IOException;`
		* 获取上传文件类型: `public String getContentType();`
		* 获取上传的请求头: `public String getHeader(String name);`、`public Collection<String> getHeaders(String name);`等, 用来获取`content-disposition`头里面的文件名称
		* 删除临时文件: `public void delete() throws IOException;`
## <font color=orange> 静态代理和动态代理 </font>
* 静态代理
	* 要求装饰者和被装饰者实现同一个接口或继承于同一个类
	* 在装饰者中要有被装饰者的引用
	* 对需要加强的方法中进行加强
	* 对不需要加强的方法调用原来的方法
* 动态代理
	* 在项目运行时才会创建一个代理对象, 对方法进行增强
	* 方式1: JDK中的Proxy类, 前提: 实现接口
		* 动态的在内存中创建一个代理对象: `public static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h)`
			* 参数说明
				* ClassLoader loader: 代理对象类加载器, 一般使用被代理对象的类加载器
				* Class<?>[] interfaces: 代理对象需要实现接口, 一般使用被代理对象所有实现的接口.
				* InvocationHandler h: 执行处理类, 在这里对方法进行增强	
					* InvocationHandler中只有一个方法
						* `Object invoke(Ojbect proxy, Method method, Object[] args)`
							* Object proxy: 代理对象
							* Method method: 当前执行的方法
							* args: 当前方法执行方法所需的参数
							* 返回值: 当前方法执行后的返回值
		* 例如: 使用Filter实现统一编码

		>	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)  throws IOException, ServletException {
		&emsp;&emsp;//强转
  		&emsp;&emsp;final HttpServletRequest req = (HttpServletRequest) request;
  		&emsp;&emsp;HttpServletResponse res = (HttpServletResponse) response;
  		&emsp;&emsp;//使用动态代理
  		&emsp;&emsp;HttpServletRequest reqProxy = (HttpServletRequest) Proxy.newProxyInstance(HttpServletRequest.class.getClassLoader(), req.getClass().getInterfaces(), new InvocationHandler() {
    	&emsp;&emsp;&emsp;&emsp;@Override
    	&emsp;&emsp;&emsp;&emsp;public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
      	&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;if ("getParameter".equals(method.getName())) {
      	&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;if ("get".equalsIgnoreCase(req.getMethod())) {
       &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;String s = (String) method.invoke(req, args);
       &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;return new String(s.getBytes("iso-8859-1"), "utf-8");
       &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;} else if ("post".equalsIgnoreCase(req.getMethod())) {
       &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;req.setCharacterEncoding("utf-8");
       &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;return method.invoke(req, args);
       &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;}
       &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;}
      	&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;return method.invoke(req, args);
       &emsp;&emsp;&emsp;&emsp;}
  		&emsp;&emsp;});
  		&emsp;&emsp;//放行
  		&emsp;&emsp;chain.doFilter(reqProxy, res);
		}

	* 方式2: spring中的cglib, 前提: 继承类