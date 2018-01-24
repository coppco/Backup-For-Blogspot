layout: java
title: Java Web学习03---jQuery
date: 2016-07-03 09:19:29
tags:
    - Java
    - jQuery
    - JavaScript
---
jQuery是一个优秀的JavaScript库, 它兼容CSS3, jQuery兼容各种主流浏览器，如IE 6.0+、FF 1.5+、Safari 2.0+、Opera 9.0+等.jQuery能够使html页面保持代码和html内容分离, 也就是说不用在html里面插入一堆JavaScript来调用命令了, 只需定义id即可.

<!--more-->

## <font color=orange>jQuery</font>
* 版本
    * jQuery1.x
    * jQuery2.x 不再支持IE6,7,8
    * min版本是对应版本的压缩版, 大小更小, 加载速度快, 一般线上环境使用.
* 作用
    * 大大简化的JS的代码编码
    * 将页面和JS分离
    * 链式编程
* 导入jQuery
    * 将jquery-xx.js导入到项目中, 使用`<script src="jquery-xx.js"></script>`
* <font color=red>jQuery使用注意</font>
    * <font color=orange>jQuery可以使用简写 $</font>
    * 使用JS写的代码只能调用JS中的属性和方法.
    * 使用jQuery写的代码只能调用jQuery中的属性和方法.
    * JQ和DOM对象可以相互转换
        * DOM对象--->jQuery对象
        >   var s1 = document.getElementById("s1");
        // 将JS对象转成JQuery的对象
        jQuery(s1).html("Hello 王超杰");
        //jQuery 也可以使用简写   $
        $(s1).html("Hello 王超杰");
        * jQuery对象--->DOM对象
        >   // 将JQ的对象转成JS的对象。
        // $("#s1")[0].innerHTML="Hello 王守义";
        // $("#s1").get(0).innerHTML = "Hello 王守义";
* <font color=orange>jQuery中的选择器</font>
    * 基本选择器
        * 通过id值
            * 如:$("#id值");
        * 通过元素名称
            * 如:$("div");
        * 通过class   
            * 如:$(".class名");
        * 通配符*, 获取所有元素
            * 如:$("*")
        * 多个选择器一起
            * 如:$("div,span,p.myClass")
    * 层次选择器
        * 后代选择器
            * 如:$("父选择器 子选择器")
        * 子选择器
            * 如:$("父选择器>子选择器")
        * 匹配所有紧接在 prev 元素后的 next 元素
            * 如:$("父选择器 + 子选择器")
        * 匹配 prev 元素之后的所有 siblings 元素
            * 如:$("父选择器 ~ 子选择器")
    * 基本过滤选择器
        * 获取匹配的第一个元素:first
            * 如: $('li:first');
        * 获取最后个元素:last
            * 如: $('li:last')
        * 去除所有与给定选择器匹配的元素
            * 如: $("input:not(:checked)") input标签中checked标签
        * 匹配所有索引值为偶数的元素，从 0 开始计数:even
            * 如: $("tr:even") 获取偶数行
        * 匹配所有索引值为奇数的元素，从 0 开始计数:odd
            * 如:$("tr:odd") 获取奇数行
        * 匹配索引位置的元素:eq
            * 如:$('tr:eq(index)') tr里面索引等于index的元素
        * 匹配大于索引位置的元素:gt()
            * 如:$('tr:gt(index)') tr里面索引大于index的元素
        * 匹配小于索引位置的元素:lt()
            * 如:$('tr:lt(index)') tr里面索引小于index的元素
    * 内容过滤器
        * 包含指定选择器的元素:has()
            * 如:$("tr:has('.name'") tr里面包含class为name的元素
    * 可见性过滤器
        * 获取隐藏的元素, 一般指display为none, 和input type='hidden'两种情况:hidden
            * 如: $("div:hidden") 所有隐藏的div
        * 获取可见的元素:visible
            * 如: $("div:visible") 所有隐藏的div
    * 表单选择过滤器
        * 对于单选框和多选框 获取选择的元素:selected
        * 对于下拉选 获取选中的元素:checked
    * 属性过滤器
        * [属性名]
            * 如:$("div[title]") div中包含属性为title的元素
        * [属性名=值]
            * 如:$("div[title='小兵张嘎']") div中包含属性为title, 值为'小兵张嘎'的元素
    * 表单过滤器
        * 获取表单的子标签(包含input、textArea、select、button) :input
            * 如: $("form :input")
        
* <font color=orange>jQuery获取元素</font>
    * $("选择器")或jQuery("选择器")
    * 当返回值有多个时 通过[index]或者get(index)获取元素
* 页面加载完成之后onload事件写法.
    * JavaScript写法
    >   window.onload = function() {
    //code
    }
    //该方法在一个页面中只能出现一次, 出现多次只会执行一次
    * jQuery的写法, 可以有多个, 都会执行
        * 方式1
        >   $(document).ready(function() {//code});
        * 方式2, 其实是对方式1的简写
        >   $(function(){//code});
* <font color=orange>jQuery事件</font>
    * jQuery里面的事件是DOM事件名去掉前面的on即可, 而且是链式编程.
* <font color=orange>jQuery效果</font>
    * show(); --显示某个元素
        * 参数
            * 毫秒值, 持续事件
            * function 完成后的执行的函数
            * 切换效果默认是:  "swing"，可用参数"linear"
    * hide(); --隐藏某个元素
    * slideDown();	--向下滑动
    * slideUp();	--向上滑动
    * fadeOut();	--淡出
    * fadeIn();		--淡入
    * animate();	--自定义动画
    * toggle();		--单击事件的切换
        * 可以在show()、hide()之间切换
    * slideToggle();		--单击事件的切换
        * 可以在slideDown()、slideUp()之间切换
    * fadeToggle();		--单击事件的切换
        * 可以在fadeIn()、fadeOut()之间切换
* <font color=orange>jQuery修改属性</font>
    * 使用attr设置/修改属性
        * attr("属性名") 获取属性值
        * attr("属性名", "value")  设置一个属性值
        * attr({"属性1":"value1", "属性2":"value2"})设置多个属性
        * removeAttr("属性名") 移除指定属性
    * 使用addClass("class值")和removeClass("class值")来添加和移除指定值
    * jQuery1.6版本以后使用prop()和removeProp()方法类设置/修改移除有的属性, 如attr无法获取checked属性值
* <font color=orange>jQuery修改CSS</font>, 1.8以后版本才有这个函数, 实际会添加内联style属性
    * $("选择器").css("key","value"); 设置/修改一个css属性
    * ("选择器").css({"key1":"value1", "key2":"value2"}); 设置/修改多个css属性
* <font color=orange>jQuery遍历数组</font>
    * 数组名.each(function() {//code});
    * $.each(数组名, function() {//code});
* jQuery的html()方法、text()方法和val()方法
    * html() 设置和获取html标签体内容
    * text() 设置和获取html标签体内容
    * val() 设置和获取html标签体value值
    * 创建一个元素 $("<option>请选择</option>")
* jQuery的文档操作
    * 内部插入
        * a.append(c) 将c插入到a内部标签体的后面
        * a.prepend(c) 将c插入到a内部标签体的前面
        * a.appendTo(c) 将a插入到c内部标签体的后面
        * a.prependTo(c)  将a插入到c内部标签体的前面
    * 外部插入
        * a.after(c) 将c插入到a的后面
        * a.before(c) 将c插入到a的前面
        * a.insertAfter(c) 将a插入到c的后面
        * a.insertBefore(c) 将a插入到c的前面
    * 删除
        * empty() 清空元素
        * remove() 删除元素
