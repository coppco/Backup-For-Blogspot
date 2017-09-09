---
layout: post
title: Java Web学习09---Request和Response
comments: true
date: 2017-01-15 17:16:48
tags:
	- Java
	- HTTP
	- Request
	- Response
---
&emsp;&emsp;Request 和 Response 对象起到了服务器与客户机之间的信息传递作用。Request 对象用于接收客户端浏览器提交的数据，而 Response 对象的功能则是将服务器端的数据发送到客户端浏览器。
  
<!--more-->
  
  
## <font color=orange> Response </font>
&emsp;&emsp;Response对象用于动态响应客户端请示，控制发送给用户的信息，并将动态生成响应。Response对象只提供了一个数据集合cookie，它用于在客户端写入cookie值。若指定的cookie不存在，则创建它。若存在，则将自动进行更新。结果返回给客户端浏览器。

* Response组成
	* <font color=orange>响应行</font>
		* 格式: `协议/版本 状态码 状态码说明`
		* <font color=orange>常见状态码</font>
			* 1xx: 已发送请求
			* 2xx: 已完成响应
				* 200: 正常响应
			* 3xx: 还需要浏览器进一步操作
				* <font color=red>302</font>: 重定向(用的多), 需要配置响应头:location
				* 304: 读缓存 
			* 4xx: 用户操作错误
				* 404: 资源不存在
				* 405: 访问的方法不存在
			* 5xx: 服务器错误
				* 500: 内部异常
		* 设置状态码
			* setStatus(int 状态码): 主要用来设置1xx、2xx、3xx错误码
			* sendError(int 状态码): 主要用来设置4xx、5xx
	* <font color=orange>响应头</font>
		* 设置响应头格式: `set/addHeader(String key, String value)`、`set/addIntHeader(String key, int value)`、`set/addDateHeader(String key, long value)`等, 其中<font color=orange>一个key可以对应多个value.set是设置, 会覆盖之前设置的响应头; add是添加响应头, 不会覆盖.</font>
		* 判断是否包含某个响应头: `boolean	containsHeader(String name)`
		* 常用的响应头
			* <font color=orange>location</font>: 重定向
				* 方式1
					* 1、设置状态码
	
					>	response.setStatus(302);
	
					* 2、设置location
	
					>	response.addHeader("location", "https://www.baidu.com");
				* 方式２, 常用
	
				>	response.sendRedirect("https://www.baidu.com");
			* <font color=orange>refresh</font>: 定时刷新
				* 方式1
					* 通过html的meta标签实现
					>	<meta http-equiv="refresh" content="3;url=/"&gt;
				* 方式2
					* 通过response设置header实现
					>	response.add(/set)Header("refresh", "3;url=/");
			* <font color=orange>content-type</font>: 设置文件mime类型, 设置响应流的编码方式,以及浏览器打开是的编码方式.
				* 方式1
				>	response.setContentType("text/html;charset=utf-8");
				* 方式2
				>	response.setHeader("content-type", "text/html;charset=utf-8");
			* <font color=orange>content-disposition</font>: 文件下载使用 
				* 文件下载的方式
					* 1、<font color=orange>超链接下载</font>(如果浏览器能处理会直接打开, 不能处理才会下载)
					>	<a href="/ProjectName/download/测试.txt"&gt;超链接下载文本</a&gt;
					* 2、<font color=orange>编码下载, 通过servlet完成</font>
					>	//获取参数
					String fileName = request.getParameter("name");
					//获取文件的mime类型
					ServletContext servletContext = this.getServletContext();
					String type = servletContext.getMimeType(fileName);
					//设置mime类型
					response.setContentType(type);
					//设置下载头信息
					response.setHeader("content-disposition", "attachment;filename=" + fileName);
					//输入流和输出流读写
					InputStream resourceAsStream = servletContext.getResourceAsStream("/download/" + fileName);
					ServletOutputStream outputStream = response.getOutputStream();
					//这里也可以使用<font color=red>commons-io.jar</font>进行拷贝`IOUtils.copy(input, output)`
					byte[] arr = new byte[1024 * 8];
					int length = -1;
					while ((length = resourceAsStream.read(arr)) != -1) {
					&emsp;&emsp;outputStream.write(arr, 0, length);
					}
					outputStream.close();
					resourceAsStream.close();
	* <font color=orange>响应体</font>
		* 操作响应体(<font color=red>字符流和字节流不可以同时使用, 并且服务器会自动关流</font>)
			* 获取字符流(测试使用)
				* `PrintWriter	getWriter()`
			* 获取字节流(常用)
				* `ServletOutputStream	getOutputStream()`
				
## <font color=orange> Request </font>
&emsp;&emsp;Request对象的作用是与客户端交互，收集客户端的Form、Cookies、超链接，或者收集服务器端的环境变量。
* Request组成
	* <font color=orange>请求行</font>
		* 格式: `请求方式 请求资源 协议/版本`
		* 常用方法
			* <font color=orange>获取请求方式</font>: `String method = request.getMethod();`
			* <font color=orange>获取请求者的IP地址</font>: `String getRemoteAddr()`
			* <font color=orange>根据key获取一个请求参数值(★)</font>: `String	getParameter(String name)` 
			* <font color=orange>根据key获取多个请求参数值(★)</font>: `String[] getParameterValues(String name)` 
			* <font color=orange>获取所有请求参数的键和值, 返回集合(★)</font>: `Map(String, String[]) getParameterMap()` 
			* <font color=orange>在Java中获取项目名称(★)</font>: `String getContextPath()`
			* 获取完整的请求路径(不带参数): `StringBuffer getRequestURL()`
			* 获取从项目名开始到参数之前的内容: `String getRequestURI()`
			* 获取get请求的所有参数: `String	getQueryString()`
			* 获取协议: `String getProtocol()`
	* <font color=orange>请求头</font>
		* 格式: `key/value`的形式,(value可以是多个)
		* 常用请求头
			* user-agent: 浏览器内核
				* msie: IE浏览器
				* firefox: 火狐浏览器
				* chrome: 谷歌浏览器
			* referer: 页面从哪里来(防止倒链) 
		* 常用方法
			* 获取指定key的header值(一个): `String getHeader(String key);`
			* 获取指定key的所有header值(多个): `Enumeration getHeaders(String name)`
	* <font color=orange>请求体</font>
		* 只有post请求才会有请求体
* <font color=orange>中文乱码问题</font>
	* 获取请求参数时中文乱码
		* get请求: 参数会拼接到地址栏, 使用`utf-8`编码, 服务器收到参数后使用`iso-8859-1`编码, 所有会出现乱码
		* post请求: 参数放在请求体中, 服务器收到请求体后使用`iso-8859-1`编码, 也会出现乱码
		* <font color=orange>解决方法</font>
			* 通用方法: `new String(参数.getBytes("iso-8859-1"), "utf-8");`, 服务器获取到参数后先使用`iso-8859-1`解码转为Byte数组, 然后再使用`utf-8`编码成字符串.
			>	String username = new String(request.getParameter("username").getBytes("iso-8859-1"), "utf-8")
			* 对于post请求, 只需要将请求流的编码设置为`utf-8`即可.
			>	request.setCharacterEncoding("utf-8");
	* 文件下载时, 文件名中文乱码问题
		* 常见浏览需要使用`utf-8`编码的文件名, IE浏览器文件名如果有空格它会使用`+`代替
		>	response.setHeader("content-disposition", "attachment;filename=" + URLEncoder.encode(fileName, "utf-8"));
		* 火狐浏览器需要使用`base64`编码的文件名
		* 其他方式
			* 使用封装的工具类
			* 大部分浏览器都OK: `response.setHeader("content-disposition", "attachment;filename=" + new String(filename.getBytes("gbk"), "iso-8859-1"));`
			* 如果使用Tomcat、JBoos也可以设置`URLEncoding="UTF-8"`
	* 编码
	>	String URLEncoder.encode(String, 编码方式);
	* 解码
	>	String URLDecoder.decode(String, 编码方式);
* <font color=orange>请求转发</font>
	* request对象也是`域对象`, 它在一个请求来的时候创建, 在响应生成时销毁.
	* 请求转发方法
	>	request.getRequestDispatcher("`web.xml中配置的url-pattern路径`").forward(request, response); 
	* 设置参数
	>	public void setAttribute(String name, Object o);
	* 获取参数
	>	public Object getAttribute(String name);
	* 获取所有参数
	>	public Enumeration<String> getAttributeNames();
	* 移除参数
	>	public void removeAttribute(String name);
* <font color=orange>请求转发如果是表单时, 重复提交问题</font>
	* 请求转发之后刷新网页会提示重复提交, 解决办法有
		* 使用重定向
		* 使用令牌机制
* <font color=orange>BeanUtils的使用</font>
	* BeanUtils是Apache提供的一个工具类, 它可以快速封装一个对象
	* 使用步骤
		* 导入`commons-beanutils-xxx.jar`和`commons-logging-xxx.jar`包
		* 使用`BeanUtils.populate(Object bean, Map<> map)`即可.
* <font color=orange>请求转发和重定向的区别</font>
	* 重定向发送两次请求, 请求转发只发送一次请求
	* 重定向地址发起变化, 请求转发地址不会变化
	* 重定向是客户端发送, 请求转发是服务器内部发送
	* 重定向不存在域对象, 请求转发可以使用request域对象
	* 重定向是Response的方法, 请求转发是Request的方法.
	* 重定向可以访问站外资源, 请求转发只能访问服务器内部资源