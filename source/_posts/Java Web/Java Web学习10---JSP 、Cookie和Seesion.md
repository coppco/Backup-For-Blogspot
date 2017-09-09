---
layout: post
title: Java Web学习10---JSP、Cookie和Seesion
comments: true
date: 2017-01-23 14:27:06
tags:

	- Java
	- JSP
	- Cookie
	- Session
---

JSP最大的特点在于，写JSP就像在写html，但它相比html而言，html只能为用户提供静态数据，而JSP技术允许在页面中嵌套java代码，为用户提供动态数据。

## <font color=orange>JSP</font>

&emsp;&emsp;JSP全名为Java Server Pages，中文名叫java服务器页面，<font color=red>本质是一个简化的Servlet设计</font>，它是由Sun Microsystems公司倡导、许多公司参与一起建立的一种动态网页技术标准。JSP技术有点类似ASP技术，它是在传统的网页HTML文件(\*.htm,\*.html)中插入Java程序段(Scriptlet)和JSP标记(tag)，从而形成JSP文件，后缀名为(\*.jsp)。 用JSP开发的Web应用是跨平台的，既能在Linux下运行，也能在其他操作系统上运行。

&emsp;&emsp;它实现了Html语法中的java扩张（以 <%, %>形式）。JSP与Servlet一样，是在服务器端执行的。通常返回给客户端的就是一个HTML文本，因此客户端只要有浏览器就能浏览。

&emsp;&emsp;使用JSP技术，可以将内容的生成和显示进行分离,Web页面开发人员可以使用HTML或者XML标识来设计和格式化最终页面，并使用JSP标识或者小脚本来生成页面上的动态内容.

&emsp;&emsp;Tomcat中对应的Java和.class文件存放在<font color=orange>`work`</font>目录中, jsp文件后缀名是<font color=orange>`jsp`</font>.

* <font color=orange>JSP执行流程</font>
	* 1.在浏览器中输入: `http://localhost/项目名/xxx.jsp`
	* 2.服务器得到请求，会通过jsp的`servlet`查找到xxx.jsp页面.
	>	//Tomcat---conf---web.xml文件中.
	 <servlet-mapping&gt;
    &emsp;<servlet-name&gt;jsp</servlet-name&gt;
    &emsp;<url-pattern&gt;\*.jsp</url-pattern&gt;
    &emsp;<url-pattern&gt;\*.jspx</url-pattern&gt;
    </servlet-mapping&gt;
	* 3.服务器将查找到的xxx.jsp页面翻译成xxx_jsp.java(其本质就是一个servlet).
	* 4.jvm会将xxx_jsp.java文件编译成xxx_jsp.class.
	* 5.服务器运算xxx_jsp.class文件, 生成动态的内容.
	* 6.服务器生成响应结果.
	* 7.服务器组成响应信息, 发送给浏览器.
	* 8.浏览器接受数据, 解析展示.

* <font color=orange>JSP中的脚本, 它的作用是使Java代码可以直接插入到HTML代码中</font>
	* 声明标签
		* 作用: 声明的变量在类的成员位置上
		* 格式: `<%! ... %>`
	* 脚本片断
		* 作用: 内容会生成在_jspService()方法中 
		* 格式: `<% ... %>`
	* 脚本表达式
		* 作用: 它就相当于是out.println()将内容直接输出到页面中，注意表达式不能以分号结尾
		* 格式: `<%= ... %>`
* <font color=orange>JSP中的注释</font>
	* 使用html中的注释
		* 格式: `<--html的注释-->`
		* 只在页面上面看不见, 在Java代码和html源代码都有
	* 使用Java中的注释
		* 格式1: 单行注释 `//单行注释`
		* 格式2: 多行注释 `/*多行注释*/`
		* 只在Java代码中存在
	* 使用JSP中的注释
		* 格式: `<%--JSP中的注释--%>`
		* 只在JSP页面中存在, 翻译成Java文件后就没有了.
* <font color=orange>JSP指令, 一个JSP可以有多个指令, 可以放在任何位置, 但是通常放在jsp文件顶部</font>
	* 用于指示JSP执行某些步骤
	* 用于指示JSP表现特定行为
	* 格式, 
	>	<%@指令名 属性1=”” 属性2=””%>
	* JSP中有三大指令
		* <font color=orange>page</font>: 导入类,编码的设置
			* <font color=orange>import</font>属性: 在jsp页面上导包操作
			* <font color=orange>pageEncoding</font>属性: 指定当前jsp页面的编码,这个编码是给服务器看的，服务器需要知道当前页面的编码，否则服务器无法正确的把jsp翻译成Java文件.
			* <font color=orange>contentType</font>属性: 默认值是`text/html;charset=ISO-8859-1`, 设置了之后jsp对应的java文件就会出现`response.setContentType(“xxxx”)`.
			* pageEncoding与contentType都是page指令的属性，它们都是用来设置编码，有如下联系：
				* 如果这两个属性只提供了其中一个，那么没有提供的那个属性的编码值就是提供的这个属性的编码值，例如：在jsp页面中设置了contentType=”text/html;charset=utf-8”，那么没有设置的pageEncoding的值就为utf-8，反之亦然；
				* 如果两个属性都没有提供，那么两者的默认编码就是ISO-8859-1。 
			* pageEncoding与contentType的区别：
				* pageEncoding是设置当前页面的编码，该编码是给服务器看的，可以让服务器正确的将jsp文件翻译成Java文件；
				* contentType有两个作用：一是设置响应字符流的编码，二是设置Content-Type响应头，
			* language: 代表在jsp脚本中可以写的语言, 只有一个值 java
			* extends: 它用于设置jsp翻译后的java类的父类. 要求必须是HttpServlet或其子类.
			* session: 面上是否禁用session。可取值为true/false  如果值为false,在页面上不能使用session。
			* isELIgnored: 用是否忽略el表达式.可取值为true/false  如果值为true,那么页面上的el表达式就不会被解析.
			* autoFlush与buffer: 用于设置jsp中out流的默认缓冲区大小以及是否自动刷新.
			* errorPage: 设置错误页面，当jsp中如果出现了异常，会自动跳转到指定的错误页面
			* isErrorPage: 指示当前页面是一个错误页面，这时就可以使用一个内置对象 exception，通过这个内置对象就可以获取异常信息.
		* <font color=orange>include</font>: 静态包含,标文件的源码包含过来，一起编译, 只会生成一个.class、.java文件.
			* 格式: `<%@ include file=“filename” %> `
		* <font color=orange>taglib</font>: 导入标签库
			* 格式: `<%@taglib prefix="" uri="" %>`
				* uri: 标签文件的URI地址
				* prefix: 标签组的命名空间前缀
* <font color=orange>JSP中的内置对象, 这些对象我们可以在JSP中直接使用的对象.</font>
	* <font color=orange>out</font>: javax.servlet.jsp.JspWriter类型，代表输出流的对象。作用域为page（页面执行期）
	* <font color=orange>request</font>: javax.servlet.ServletRequest的子类型，此对象封装了由WEB浏览器或其它客户端生成地HTTP请求的细节（参数，属性，头标和数据）。作用域为request(用户请求期）。
	* <font color=orange>response</font>: javax.servlet.ServletResponse的子类型，此对象封装了返回到HTTP客户端的输出，向页面作者提供设置响应头标和状态码的方式。经常用来设置HTTP标题，添加Cookie，设置响应内容的类型和状态，发送HTTP重定向和编码URL。作用域为page（页面执行期）。
	* <font color=orange>pageContext</font>: javax.servlet.jsp.PageContext（抽象类）类型，作用域为page（页面执行期）。此对象提供所有四个作用域层次的属性查询和修改能力，它也提供了转发请求到其它资源和包含其他资源的方法：该对象的方法都是抽象方法
	* <font color=orange>session</font>: javax.servlet.http.HttpSession类型，主要用于跟踪对话。作用域session(会话期—）。HttpSession是一个类似哈希表的与单一WEB浏览器会话相关的对象，它存在于HTTP请求之间，可以存储任何类型的命名对象。如果不需要在请求之间跟踪会话对象，可以通过在page指令中指定session="false".需要记住的是pageContext对象也可以与session.getAttribute(),session.setAttribute()一样的方式取得并设置会话属性。
	* <font color=orange>application</font>: javax.servlet.ServletContext类型，servlet的环境通过调用getServletConfig().getContext()方法获得。作用域是application(整个程序运行期）。它提供了关于服务器版本，应用级初始化参数和应用内资源绝对路径，注册信息的方式
	* <font color=orange>config</font>: javax.servlet.ServletConfig,作用域为page（页面执行期）
	* <font color=orange>exception</font>: java.lang.Throwable,通过JSP错误页面中一个catch块已经益出但没有捕获的java.lang.Throwable的任意实例，传向了errorPage的URI。作用域为page（页面执行期）. 注意exception只有在page指令中具有属性isErrorPage="true"时才有效。
	* <font color=orange>page</font>: java.lang.Object类型，指向页面自身的方式。作用域为page（页面执行期)

|内置对象|代表内容|范围|
|:---:|:---:|:---:|
|request|触发服务调用的请求|request|
|response|对请求的应答|page|
|session|为请求的用户创建session对象|session|
|application|从servlet配置对象获得的servlet上下文(如果在getServletConfig(), getContext()中调用中)| application |
|out|向输出流写入内容的对象|page|
|pageContext|本JSP的页面上下文|page|
|page|实现处理本页当前请求的类的实例|page|
|config|本JSP的ServletConfig|page|
|exception|表示JSP页面运行时产生的异常|page|

* <font color=orange>JSP动作标签</font>
	* `<jsp:forward page="跳转页"></jsp:forward>`: 请求转发
	* `<jsp:include page="包含页"></jsp: include>`: 动态包含, 将被包含页面或者servlet的运行结果包含到当前页面.会生成对个.class、.java文件
	* `<jsp:param value="" name=""/>`: 它作为<jsp:forward>和<jsp:include>的子标签, 作用是往请求转发和包含的网页传递参数.

### <font color=orange>EL</font>
&emsp;&emsp;EL是Expression Language的缩写，它是jsp内置的表达式语言，从jsp2.0开始，就不让再使用java脚本，而是使用el表达式或动态标签来代替java脚本, 代替的是java脚本中的`<%= ... %>`。
* EL表达式的格式: `${表达式} `
* EL表达式的作用
	* 获取域中数据
		* 例如: `${pageScope|requestScope|sessionScope|applicationScope.属性名}`, 其中前面的Scope可以省略, 
		* 简洁写法: `${属性名}` , 依次从pageContext、request、session、application查找属性值, 若查找到返回值, 结束该次查找, 若找不到, 则返回"".
		* 获取数组中的数据: `${域中的名称[index]}`
		* 获取数组中的数据: `${域中的名称[index]}`
		* 获取数组中的数据: `${域中的名称.键名}`
		* 如果属性名中出现`.`、`+`、`-`等特殊符号, 需要使用`["属性名"]`来获取, 例如: `${requestScope["属性名"]}`
		* JavaBean导航
			* 如果在域中保存的是javaBean对象，那么也可以使用EL表达式来访问javaBean的属性，因为EL表达式只做读操作，所以javaBean一定要为属性提供get方法，而对set方法没有要求。使用EL表达式获取javaBean属性就是javaBean导航。
	* 执行运算
		* 可以进行四则运算, 逻辑运算, 但是 `+`只能做加法运算.
		* `${empty arr}`可以判断 数组、set、list、map的长度是否为0, 也可以判断对象是否为空
		* JSP中获取项目名: `${pageContext.request.contextPath}`
	* 获取web开发常用对象
	* 调用Java方法

### <font color=orange>jstl</font>
&emsp;&emsp;JSTL(JSP Standard Tag Library，jsp标准标签库)是Apache对EL表达式的扩展，也就是说JSTL依赖EL表达式。JSTL是标签语言，使用起来非常方便。但是它不是jsp内置的标签，所以用的时候需要我们自己导包，以及指定标签库。使用Myeclipse开发JavaWeb项目会自动导入包, 但是Eclipse不会自动导入包.

* 标签库
	* <font color=red>core</font>: 核心标签
	* fmt: 格式化标签库
	* sql: 数据库标签库
	* xml: xml标签库
	
* 使用jstl步骤
	* 导入`jstl-1.2.jar`包
	* 在JSP页面中`<%@ taglib prefix=”xxx” uri=”http://java.sun.com/jsp/jstl/core”%> `
		* prefix="c"：指定标签库的前缀，这个前缀可以赋任意的值，但大家都会在使用core标签库时指定前缀为c；
		* uri="http://java.sun.com/jsp/jstl/core"：指定标签库的uri，它不一定是真实存在的网址，但它可以让JSP找到标签库的描述文件。
	* 核心标签
		* c:if
			* 格式:
`<c:if test="判断的内容(一般是el表达式)" var="给前面表达式的结果起个名称" [scope="page|request|session|application"] />`				
				* scope 用来表达式结果存放的域
				* 若指定了标签的scope属性，则必须指定var属性
			* 例如: `<c:if test="${3>4 }" var="flag"> 三大于四 </c:if> `
		* c:forEach
			* begin属性: 设置循环变量从几开始
			* end属性: 设置循环变量到几结束；
			* step属性: 设置循环变量的步长
			* var属性: 定义一个变量，用于接收循环或把数组或集合中遍历的每一个元素赋值给var指定的变量
			* varStatus属性: varStatus属性就是用来记录循环状态的，它可以创建一个循环变量vs，该循环变量有如下属性：
				* count：用来记录循环元素的个数；
				* index：用来记录所循环元素的下标；
				* first：判断当前循环的元素是否是第一个元素；
				* last：判断当前循环的元素是否是最后一个元素；
				* current：代表当前循环的元素。
			* items:属性: 指定要循环的变量，可以是一个数组也可以是一个集合，默认是支持EL表达式
			
### <font color=orange>会话技术</font>
* 会话的作用:
&emsp;&emsp;每个用户与服务器进行交互的过程中，各自会有一些数据，程序要想办法保存每个用户的数据。
&emsp;&emsp;一般用户登录、验证码、购物车、访问记录等通过绘画技术保存起来.
* 会话技术
	* Session: 服务器会话技术
	* Cookie: 客户端会话技术


## <font color=orange> Cookie </font>
&emsp;&emsp;Cookie是客户端技术，程序把每个用户的数据以Cookie的形式写给用户各自的浏览器。当用户使用浏览器再去访问服务器中的web资源时，就会带着各自的数据去。这样，web资源处理的就是用户各自的数据了。
&emsp;&emsp;Cookie是由服务器端生成，发送给User-Agent（一般是浏览器），浏览器会将Cookie的key/value保存到某个目录下的文本文件内，下次请求同一网站时就发送该Cookie给服务器（前提是浏览器设置为启用Cookie）。Cookie名称和值可以由服务器端开发自己定义，对于JSP而言也可以直接写入jsessionid，这样服务器可以知道该用户是否合法用户以及是否需要重新登录等，服务器可以设置或读取Cookies中包含信息，借此维护用户跟服务器会话中的状态。

&emsp;&emsp;<font color=orange>Cookie是Http协议制定的</font>, 所有使用HTTP技术的都可以使用Cookie技术.需要注意的是: <font color=red>Cookie是不能跨浏览器的, 并且不支持中文</font>

* Http协议对Cookie做了一些规定,但是实际浏览器可能不会严格按照此规定.
	* 一个Cookie的大小，最大为4KB；
	* 一个服务器最多向一个浏览器保存20个Cookie；
	* 一个浏览器最多可以保存300个Cookie。

* 服务端生成Cookie, 保存到浏览器, 可以添加多个Cookie
	* 方式1: 通过`javax.servlet.http.Cookie`实现
		* 1.1、创建`javax.servlet.http.Cookie`对象
		>	public Cookie(String name, String value)
		* 1.2、通过Response发送给浏览器
		>	public void addCookie(Cookie Cookie);
	* 方式2: 直接通过给Response添加`Set-Cookie`响应头来实现
	>	response.addHeader(“Set-Cookie”,”key1=value1”)；
	response.addHeader(“Set-Cookie”,”key2=value2”)；
* 浏览器再次请求时会通过`Cookie`的请求头将Cookie发送给服务器
	* 请求头Cookie与响应头Set-Cookie有区别，多个Cookie对应多个Set-Cookie响应头，但是只对应一个Cookie请求头，格式为：key1=value1； key2=key2。即多个Cookie之间用分号和空格隔开。
	
* <font color=orange>javax.servlet.http.Cookie</font>
&emsp;&emsp;javax.servlet.http.Cookie类用于创建一个Cookie，response接口也中定义了一个addCookie方法，它用于在其响应头中增加一个相应的Set-Cookie头字段。 同样，request接口中也定义了一个getCookies方法，它用于获取客户端提交的Cookie。
	* 常用方法
		* 构造方法: `public Cookie(String name, String value):构造带指定名称和值的 Cookie`
		* 获取Cookie的名称: `public String getName():返回 Cookie 的名称`	
		* 获取Cookie的值: `public String getValue():返回 Cookie 的值。 `
		* 设置Cookie的生命周期: `public void setMaxAge(int expiry):设置 Cookie 的最大生存时间，以秒为单位`
			* 0代表的是删除持久Cookie, 注意，删除Cookie时，path必须一致，否则不会删除
			* -1代表的是浏览器关闭后失效.
		* 设置返回Cookie的路径: `public void setPath(String uri):指定客户端应该返回 Cookie 的路径。`
			* 默认路径是: 访问Servlet路径从`/项目名`开始到最后一个`/`结束, 如访问 `/test/a/b/hello`, 那么默认路径是 `/test/a/b`
			* 设置了path之后, 当浏览器访问路径包含设置的路径就会把对应的Cookie传到服务器, 访问其他路径则不会传入.
	* <font color=orange>Response相关的Cookie方法</font>
		* 添加Cookie: `public void addCookie(Cookie Cookie);`
	* <font color=orange>Request相关的Cookie方法</font>
		* 获取所有Cookie: `public Cookie[] getCookies();`
	* <font color=orange>清空Cookie</font>
		* 需要设置setMaxAge(0), 并且保持路径(path)和名称(name)一致.
		>	Cookie cookie = new Cookie("name", "");
		cookie.setPath(request.getContextPath() + "/");
		cookie.setMaxAge(0);
		response.addCookie(cookie);
		
## <font color=orange> Session </font>
&emsp;&emsp;Session是服务器端技术，利用这个技术，服务器在运行时可以为每一个用户的浏览器创建一个其独享的session对象，由于session为用户浏览器独享，所以用户在访问服务器的web资源时，可以把各自的数据放在各自的session中，当用户再去访问服务器中的其它web资源时，其它web资源再从用户各自的session中取出数据为用户服务。它由服务器创建, 并且保存在服务端.

&emsp;&emsp;HttpSession对象是Servlet的三大域对象之一，其他两个域对象是HttpServletRequest和ServletContext。这三个域中，request的域范围最小，它的域范围是整个请求链，并且只在请求转发和包含时存在；session域对象的域范围是一次会话，而在一次会话中会产生多次请求，因此session的域范围要比request大；application的域范围是最大的，因为一个web应用只有唯一的一个application对象，只有当web应用被移出服务器或服务器关闭它才死亡，它的域范围是整个应用。

* 三个域对象都具有的方法
	* void setAttribute(String name,Object value)：向域中添加域属性；
	* Object getAttribute(String name)：从域中获取指定名称的属性值；
	* void removeAttribute(String name)：移出域中指定名称的域属性

&emsp;&emsp;<font color=orange>Session底层是依赖Cookie的</font>，如果浏览器禁用Cookie则Session会依赖URL重写。详情我们会在后面介绍。如何获取HttpSession对象?在服务器端，例如在Servlet中，我们通过request对象的getSession()方法获取服务器为当前用户创建的session对象，即：HttpSession session=request.getSession()。而在jsp中，session是jsp的内置对象，不用获取就可以直接使用。

* <font color=orange>javax.servlet.http. HttpSession </font>
	* 常用方法
		* 域对象具有的方法都有
	* Request相关的方法
		* 从Request中获取: `HttpSession session=request.getSession();`, 如果没找session, 那么会返回一个新的session.
	* 销毁方式
		* 创建: 第一次调用request.getSession()时
		* 销毁
			* 服务器关闭
			* 请求超时
			* 设置session超时时间(以秒为单位)
				* Tomcat中在`web.xml`中有默认设置30分钟 
				* 手动设置超时时间: `void setMaxInactiveInterval(int interval)`
			* 销毁session: 调用`invalidate();`方法
* <font color=orange>url重写</font>
&emsp;&emsp;当客户机不接受cookie时，server就使用URL重写作为会话跟踪的基本方式.URL重写，添加了附加数据(会话ID)到请求的URL路径上. 
会话ID必须被编码作为该URL字符串中的路径参数。该参数的名称为jsessionid,
简单说就是cookie禁用了jsessionid就不能携带，那么每次请求，都是一个新的session对象。
	* url重写实现
		* response. encodeRedirectURL(java.lang.String url)用于对sendRedirect方法后的url地址进行重写。
		* response. encodeURL(java.lang.String url)用于对表单action和超链接的url地址进行重写