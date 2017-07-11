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
	* 响应头
		* 设置响应头格式: `set/addHeader(String key, String value)`、`set/addIntHeader(String key, int value)`、`set/addDateHeader(String key, long value)`等, 其中<font color=orange>一个key可以对应多个value.set是设置, 会覆盖之前设置的响应头; add是添加响应头, 不会覆盖.</font>
		* 判断是否包含某个响应头: `boolean	containsHeader(String name)`
		* 常用的响应头
			* location: 重定向
			* refresh: 定时刷新
			* content-type: 设置文件mime类型, 设置响应流的编码方式,以及浏览器打开是的编码方式.
			* content-disposition: 文件下载使用 
	* 响应体
* 重定向实现
	* 方式1
		* 1、设置状态码
	
		>	response.setStatus(302);
	
		* 2、设置location
	
		>	response.addHeader("location", "https://www.baidu.com");
			
	* 方式２
	
	>	response.sendRedirect("https://www.baidu.com");
* 定时刷新实现
	* 方式1
		* 通过html的meta标签实现
		>    <meta http-equiv="refresh" content="3;url=/">
	* 方式2
		* 通过response设置header实现
		>    response.addHeader("refresh", "3;url=/");