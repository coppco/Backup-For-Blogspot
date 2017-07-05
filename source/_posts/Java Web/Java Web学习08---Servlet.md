---
layout: post
title: Java Web学习08---Servlet
comments: true
date: 2017-01-10 06:20:53
tags:
	- Java
	- Servlet
---
HTTP协议：超文本传输协议（HTTP，HyperText Transfer Protocol)是互联网上应用最为广泛的一种网络协议。所有的WWW文件都必须遵守这个标准。

<!--more-->

## <font color=orange>HTTP</font>
HTTP协议：超文本传输协议（HTTP，HyperText Transfer Protocol)是互联网上应用最为广泛的一种网络协议。所有的WWW文件都必须遵守这个标准。
HTTP协议规定 浏览器(客户端)向服务器发送 何种格式的数据. 服务器 会处理数据. 向浏览器(客户端)作出响应.(向客户端发送何种格式的数据)
* HTTP协议的特点:
	* HTTP协议遵守一个请求响应模型.
  	  * 请求和响应必须成对出现.
  	  * 必须先有请求后有响应.
	* HTTP协议默认的端口:80
* HTTP请求
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
				* Accept: text/html,image/*		--支持数据类型
					* text/html、text/css等mime类型
				* Accept-Charset: ISO-8859-1	--字符集
				* Accept-Encoding: gzip		--支持压缩
				* Accept-Language:zh-cn 		--语言环境
				* Host: www.baidu.com:80		--访问主机
				* If-Modified-Since: Tue, 11 Jul 2000 18:23:51 GMT	  --缓存文件的最后修改时间
				* Referer(★): http://www.baidu.com/index.jsp	 --来自哪个页面、防盗链
				* User-Agent(★): Mozilla/4.0 (compatible; MSIE 5.5; Windows NT 5.0)  --发出请求的用户信息
				* Cookie(★)
				* Connection: close/Keep-Alive   	--链接状态
				* Date: Tue, 11 Jul 2000 18:23:51 GMT	--时间
		* 请求体
			* 只有post请求才有请求体， get请求参数直接拼接到url后