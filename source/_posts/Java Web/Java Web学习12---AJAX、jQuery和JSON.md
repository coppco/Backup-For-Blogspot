---
layout: post
title: Java Web学习12---AJAX、jQuery中的AJAX和JSON
comments: true
date: 2016-10-11 14:11:37
tags:
	- AJAX
	- Java
	- jQuery
	- JSON
---

AJAX即“Asynchronous Javascript And XML”（异步JavaScript和XML），是指一种创建交互式网页应用的网页开发技术。
JQuery中对于AJAX进行了封装, 使用起来更加简便.
JOSN现在主要用于服务端和客户端之间的数据传输.

<!--more-->

## <font color=orange> AJAX </font>

AJAX 是一种用于创建快速动态网页的技术。
通过在后台与服务器进行少量数据交换，AJAX 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。传统的网页（不使用 AJAX）如果需要更新内容，必须重载整个网页页面。

* <font color=orange>AJAX作用</font>:
	* AJAX不是一种新的编程语言，而是一种用于创建更好更快以及交互性更强的Web应用程序的技术。 
	* 使用Javascript向服务器提出请求并处理响应而不阻塞用户！核心对象XMLHTTPRequest。通过这个对象，您的 JavaScript 可在不重载页面的情况与Web服务器交换数据。
	* AJAX 在浏览器与 Web 服务器之间使用异步数据传输（HTTP 请求），这样就可使网页从服务器请求少量的信息，而不是整个页面。
	* AJAX 可使因特网应用程序更小、更快，更友好。
* <font color=orange>使用步骤</font>
	* 1、获取XMLHttpRequest
		* 不同浏览器获取的方式不同: 
		
		>	function getXMLHttpRequest() {
		&emsp;&emsp;var xmlhttp;
		&emsp;&emsp;if (window.XMLHttpRequest) {// code for all new browsers
		&emsp;&emsp;&emsp;&emsp;xmlhttp = new XMLHttpRequest();
		&emsp;&emsp;} else if (window.ActiveXObject) {// code for IE5 and IE6
		&emsp;&emsp;&emsp;&emsp;xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
		&emsp;&emsp;}
   		&emsp;&emsp;return xmlhttp;
		}	
	* 2、设置回调函数: onreadystatechange, 目的是服务器端响应完成后，浏览器端可以知道，并完成后续工作。
	>	xmlHttp.onreadystatechange=function(){};
	* 3、open操作: 设置访问的资源路径以及请求方式
		* 3.1、如果使用post请求, 想使用send方法传递参数, 需要设置请求头的参数, `content-type`
	* 4、send操作: 发送请求
* <font color=orange>AJAX常用属性</font>
	* <font color=orange>onreadystatechange</font>: 指定当readyState属性改变时的事件处理句柄, 只写.
	* <font color=orange>readyState</font>: 返回当前请求的状态，只读.
		* 0: 核心对象已经创建
		* 1: 核心对象已经调用了open方法
		* 2: 核心对象已经调用了send方法
		* 3: 部分响应已经生成, 没有意义
		* 4: 响应已经完成
	* status: 返回当前请求的http状态码.只读
		* 一般我们会判断`status == 200 && readyState == 4`来处理请求完成时的业务逻辑.
	* <font color=orange>responseText</font>: 将响应信息作为字符串返回.只读
	* responseBody: 将回应信息正文以unsigned byte数组形式返回.只读
	* responseStream: 以Ado Stream对象的形式返回响应信息。只读
	* responseXML: 将响应信息格式化为Xml Document对象并返回，只读
	* statusText: 返回当前请求的响应行状态，只读

* <font color=orange>AJAX常用方法</font>
	* 设置请求方式、请求路径等: `oXMLHttpRequest.open(bstrMethod, bstrUrl, varAsync, bstrUser, bstrPassword);`
		* bstrMethod: http方法，例如：POST、GET、PUT及PROPFIND。大小写不敏感。
		* bstrUrl: 请求的URL地址，可以为绝对地址也可以为相对地址。
		* varAsync[可选]: 布尔类型, 指定此请求是否为异步方式，默认为true。
		* bstrUser[可选]: 如果服务器需要验证，此处指定用户名，如果未指定，当服务器需要验证时，会弹出验证窗口。
		* bstrPassword[可选]: 验证信息中的密码部分，如果用户名为空，则此值将被忽略。
	* 发送请求到http服务器并接收回应: oXMLHttpRequest.send(varBody);
		* varBody: post请求时需要发送的参数
		>	xmlhttp.send("username=张三&password=123456");
	* setRequestHeader: 单独指定请求的某个http头
		* post请求时通过send发送参数时需要设置content-type
		>	xmlhttp.setRequestHeader("content-type", "application/x-www-form-urlencoded"); //值是form表单的enctype属性的值
	* abort: 取消当前请求
	* getAllResponseHeaders	: 获取响应的所有http头
	* getResponseHeader: 从响应信息中获取指定的http头

	
## <font color=orange> jQuery中的AJAX </font>

JQuery对AJAX进行了封装, 有如下几种方式: 

* `JQuery对象.load(url, [data], [callback])`: 载入远程 HTML 文件代码并插入至 DOM 中。
	* 参数
		* url: 待装入 HTML 网页网址。
		* data: 发送至服务器的 key/value 数据。在jQuery 1.3中也可以接受一个字符串了。name=123或者{"name": "123"}
		* callback: 载入成功时回调函数。
	* 默认使用 GET 方式, 传递附加参数时自动转换为 POST 方式
		* POST模式传递参数
		>	$("#feeds").load("feeds.php", {limit: 25}, function(){
   		&emsp;&emsp;alert("The last 25 entries in the feed have been loaded");
 		});
 		
 * 可以指定选择符，来筛选载入的 HTML 文档，DOM 中将仅插入筛选出的 HTML 代码。语法形如 "url #some &gt; selector"
	* 例如
	>	$("#links").load("/Main_Page #p-Getting-Started li");
* `jQuery.ajax(url,[settings])`或`$.ajax(url,[settings])`: 通过 HTTP 请求加载远程数据。
	* 参数
		* url: 一个用来包含发送请求的URL字符串。
		* settings: AJAX 请求设置。所有选项都是可选的。
			* accepts: Map类型, 默认取决于数据类型。如果accepts设置需要修改, 推荐在$.ajaxSetup()方法中做一次。
			* async: 默认是true, 既是所有请求默认都是异步的.如果想使用同步, 改为false即可, 注意，同步请求将锁住浏览器，用户其它操作必须等待请求完成才可以执行。
			* cache: (默认: true,dataType为script和jsonp时默认为false) jQuery 1.2 新功能，设置为 false 将不缓存此页面。
			* contents: 一个以"{字符串:正则表达式}"配对的对象，用来确定jQuery将如何解析响应，给定其内容类型。
			* contentType: (默认: "application/x-www-form-urlencoded") 发送信息至服务器时内容编码类型。默认值适合大多数情况。如果你明确地传递了一个content-type给 $.ajax() 那么他必定会发送给服务器（即使没有数据要发送）
			* data: Object或String类型, 发送到服务器的数据。将自动转换为请求字符串格式。GET 请求中将附加在 URL 后。查看 processData 选项说明以禁止此自动转换。必须为 Key/Value 格式。如果为数组，jQuery 将自动为不同值对应同一个名称。如 {foo:["bar1", "bar2"]} 转换为 "&foo=bar1&foo=bar2"。
			* dataType: String类型,预期服务器返回的数据类型。如果不指定，jQuery 将自动根据 HTTP 包 MIME 信息来智能判断，比如XML MIME类型就被识别为XML。在1.4中，JSON就会生成一个JavaScript对象，而script则会执行这个脚本。随后服务器端返回的数据会根据这个值解析后，传递给回调函数。
				* 可用值
					* xml: 返回 XML 文档，可用 jQuery 处理。
					* html: 返回纯文本 HTML 信息；包含的script标签会在插入dom时执行。
					* script: 返回纯文本 JavaScript 代码。不会自动缓存结果。除非设置了"cache"参数。'''注意：'''在远程请求时(不在同一个域下)，所有POST请求都将转为GET请求。(因为将使用DOM的script标签来加载)
					* json: 返回 JSON 数据 。
					* jsonp: JSONP 格式。使用 JSONP 形式调用函数时，如 "myurl?callback=?" jQuery 将自动替换 ? 为正确的函数名，以执行回调函数。
					* text: 返回纯文本字符串
			* timeout: Number类型, 设置请求超时时间（毫秒）。此设置将覆盖全局设置。
			* type: String类型, (默认: "GET") 请求方式 ("POST" 或 "GET")， 默认为 "GET"。注意：其它 HTTP 请求方法，如 PUT 和 DELETE 也可以使用，但仅部分浏览器支持。
			* url: String(默认: 当前页地址) 发送请求的地址。
		* 示例
			* 加载并执行js文件
			>	$.ajax({
  			&emsp;&emsp;type: "GET",
  			&emsp;&emsp;url: "test.js",
  			&emsp;&emsp;dataType: "script"
			});
* `jQuery.get(url, [data], [callback], [type])`或`$.get(url, [data], [callback], [type])`: 这是一个简单的 GET 请求功能以取代复杂 `$.ajax`, 请求成功时可调用回调函数。如果需要在出错时执行函数，请使用 `$.ajax`.
	* 参数
		* url: 待载入页面的URL地址
		* data: 待发送 Key/value 参数。
		* callback: 载入成功时回调函数。
		* type: 返回内容格式，xml, html, script, json, text, _default。
* `jQuery.post(url, [data], [callback], [type])`或者`$.post(url, [data], [callback], [type])`: 通过远程 HTTP POST 请求载入信息。这是一个简单的 POST 请求功能以取代复杂 `$.ajax` 。请求成功时可调用回调函数。如果需要在出错时执行函数，请使用 `$.ajax`。
	* 参数
		* url: 待载入页面的URL地址
		* data: 待发送 Key/value 参数。
		* callback: 载入成功时回调函数。
		* type: 返回内容格式，xml, html, script, json, text, _default。
* `jQuery.ajaxSetup([options])`: 设置全局 AJAX 默认选项。
* `serialize()`: 序列表表格内容为字符串。
* `serializeArray()`: 序列化表格元素 (类似 '.serialize()' 方法) 返回 JSON 数据结构数据。此方法返回的是JSON对象而非JSON字符串。需要使用插件或者第三方库进行字符串化操作。
* 回调函数,如果要处理$.ajax()得到的数据，则需要使用回调函数	* beforeSend 在发送请求之前调用，并且传入一个XMLHttpRequest作为参数, 返回false则取消本次请求.
	* error 在请求出错时调用。传入XMLHttpRequest对象，描述错误类型的字符串以及一个异常对象（如果有的话）
	* dataFilter 在请求成功之后调用。传入返回的数据以及"dataType"参数的值。并且必须返回新的数据（可能是处理过的）传递给success回调函数。
	* success 当请求之后调用。传入返回后的数据，以及包含成功代码的字符串。
	* complete 当请求完成之后调用这个函数，无论成功或失败。传入XMLHttpRequest对象，以及一个包含成功或错误代码的字符串。

## <font color=orange> JSON </font>
&emsp;&emsp;JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式。它基于JavaScript（Standard ECMA-262 3rd Edition - December 1999）的一个子集。 JSON采用完全独立于语言的文本格式，但是也使用了类似于C语言家族的习惯（包括C, C++, C#, Java, JavaScript, Perl, Python等）。这些特性使JSON成为理想的数据交换语言。 易于人阅读和编写，同时也易于机器解析和生成(网络传输速度)。

* <font color=orange>JSON的格式</font>
	* 对象: 使用`{}`括起来, 数据结构为 {"key": value,"key": value,...}的键值对的结构, 其中key一定是字符串, value可以是数字、字符串、数组、对象几种。取值方法为 `对象.key` 获取属性值.
	* 数组: 使用`[]`括起来, 数据结构为[value1, value2, value3,...], 字段值的类型可以是 数字、字符串、数组、对象几种。取值方法为 `使用索引获取`.
* <font color=orange>Java中常见的解析JSON的jar包</font>
	* jackson: spring默认的解析库
	* fastjson: 阿里巴巴出品, 号称最快
	* jsonlib: 现在已经停止更新
	* gson: google出品