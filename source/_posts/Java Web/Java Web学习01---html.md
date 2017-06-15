layout: java
title: Java web学习01---HTML
date: 2016-09-20 08:46:42
tags:
	- Java
---

## <font color=orange>HTML</font>

HTML指的是超文本标记语言 (Hyper Text Markup Language),不是一种编程语言，而是一种标记语言, 是使用标签来描述网页的一种语言。
* 文件后缀为.html或.htm
* html标签是以尖括号包裹关键字成对出现的，有开始标签和结束标签，支持正确的嵌套.
* 大部分标签有属性 格式：属性=“属性值”（多个属性之间用空格隔开, 属性值也可以不使用双引号括起来, 但是推荐使用双引号括起来.
* 标签
    * 围堵标签: 有开始、结束标签的标签, 例如: `<html></html>`
    * 空标签：没有结束标签,功能比较单一 ，例如：`<br></br> == <br/>`
* html不区分大小写，建议使用小写
* 网上学习HTML的网站:[w3c](http://www.w3school.com.cn)
<!--more-->

### HTML基本标签
* 文件标签(结构标签)
	* `<html></html>`: 根标签
	* `<head></head>`: 头部标签
		* `<title></title>`: 页面标题标签
	* `<body></body>`: 内容
		* text: 文本的颜色
			* 值可以为一些英文如:red、blue
			* 也可以为rgb值,如:ff,ff,ff
			* 也可以是rgb16进制数如: #ffffff
		* bgcolor: 背景色
		* background: 背景图
* 排版标签
	* `<!--注释-->`: 注释
	* `<br/>`: 换行
	* `<p></p>`: 段落
		* align: 对齐方式
	* `<hr/>`: 水平线标签
		* width: 长度
			* 值可以是px: 10px固定数值
			* 也可以是百分比: 50%, 相对父视图的比例
		* size: 粗度
		* color: 颜色
		* align: 对齐方式
* 块标签, 容器
	* `<div></div>`: 行级块标签, 自动换行
	* `<span></span>`: 行内块标签, 都在一行
* 文本标签
	* `<font></font>`: 基本文字标签 
		* color: 颜色
            * 可以是英文单词
            * 可以是#xxxxxx(16进制数表示的RGB值)
		* size: 大小(1 ~ 7, 默认3)
		* face: 字体类型, 写文字即可
	* `<h1></h1>`: 标题标签, 从h1到h6, 字体越来越小
* 清单标签
	* `<ul></ul>`: 无序列表
		* type: square  circle disc 列表项样式
	* `<li></li>`: 列表项
	* `<ol></ol>`: 有序标签
* 图形标签
	* `<img/>`: 图片
		* src: 图片地址
            * 路径的写法
                * 相对路径
                    * ./或者空:表示当前路径
                    * ../:表示上级路径
                * 绝对路径
                    * 网络URL路径, 如`https://ss0.bdstatic.com/5aV1bjqh_Q23odCf/static/superman/img/logo/bd_logo1_31bdc765.png`
                    * 本地绝对路径, 如 `c:/1.png`
		* width: 宽度
		* height: 高度
		* border: 边框
		* align: 对齐方式
		* alt: 图片的文字说明
* 链接标签
	* `<a></a>`: 超链接
		* href: 跳转的页面地址
			* 网络地址
			* #锚点, 本页的地址
		* name: 名称
		* target: _self(默认是当前窗口)、_blank(空白页), 还可以在已经设置了name的frmae中打开.
* 表格标签
	* `<table></table>`: 表格
		* border: 边框
		* width: 宽度
		* align: 对齐方式
	* `<tr></tr>`: 代表行
	* `<caption></caption>`: 表格标题
	* `<th></th>`: 表格标头
	* `<td></td>`：代表单元格
		* colspan: 列合并
		* rowspan: 行合并


### 表单标签
* form标签
	* name: 表单名称, 一般需要写
	* action: 提交的路径
	* method: 提交的方式, 默认get
		* get
			* 提交的数据放在地址栏后面, ?name=value&name=value形式
			* get提交数据不安全, get提交有大小限制(根据浏览器不同而不同)
		* post
			* 提交的数据封装在请求体中
			* post提交相对安全点, post提交大小无限制
* input标签
	* type: 类型
		* text: 文本输入框
			* value: 默认文字
            * placeholder 占位符
		* file: 文件选择框
		* password: 密码输入框
            * value: 默认文字
            * placeholder 占位符
		* radio: 单选按钮
			* name: name一样才是一组,选中一个会取消其他的选项.
			* checked: 添加属性值为checked, 那么它默认会选中.
			* value: 提交时候的值, 不能少
		* checkbox: 复选框
			* name: name一样才是一组
			* checked: 添加属性值为checked, 那么它默认会选中.
			* value: 提交时候的值, 不能少
		* submit: 提交内容到action地址里面
		* reset: 重置, 表单清空
		* image: 图片按钮, 也会提交
			* src: 图片地址
		* button: 普通按钮, 没有任何功能
		* hidden: 隐藏表单
			* 某些不想可见的表单
* select标签:下拉选择框
	* name: 表单名称
    * multiple: 可以让select选项同时显示, 多选
    * size: 可以设置一次显示几个
* option标签:下拉选择项
	* selected属性: 默认选择项
	* value: 提交值
* textarea标签: 显示文本
	* cols: 列数控制宽度
	* rows: 行数控制高度
	* 注意: 默认值放在标签体中.


### HTML框架标签及其他标签
* 框架标签, 和body标签不兼容, frameset可以相互嵌套,  但是frame标签里面不能嵌套frameset.
	* frameset标签: 
		* rows: 按行划分
		* cols: 按列划分
	* frame标签: 
		* src: 加载网页地址
		* name: 方便target根据name定位
* 其他标签
	* meta标签, 可以用来指定网页关键字、描述和编码方式等.
        * charset属性可以指定浏览器打开网页的编码方式
	* link标签, 引入网页的css文件
	* title标签, 网页的标题
	* script标签: 引入JavaScript地址
* 特殊字符
	* `&nbsp;`  -->	空格
    * `&gt;`	-->		大于号&gt;
    * `&lt;`	-->		小于号&lt;
    * `&amp;`   -->    and符&amp;
    * `&copy;`	-->		版权符号&copy;
    * `&reg;`	-->		注册符号&reg;
