layout: java
title: Java Web学习03---jQuery
date: 2016-10-03 09:19:29
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
* jQuery中的选择器
    * 通过id值
        * $("#id值");
    * 通过元素名称
        * $("div");
    * 通过class   
        * $(".class名");
    * 通配符*, 获取所有元素
        * $("*")
    * 多个选择器一起
        * $("div,span,p.myClass")
    * 后代选择器
        * $("父选择器 子选择器")
    * 子选择器
        * $("父选择器>子选择器")
    * 匹配所有紧接在 prev 元素后的 next 元素
        * $("父选择器 + 子选择器")
    * 匹配 prev 元素之后的所有 siblings 元素
        * $("父选择器 ~ 子选择器")
    * 获取匹配的第一个元素
        * $('li:first');
    * 获取最后个元素
        * $('li:last')
    * 去除所有与给定选择器匹配的元素
        * $("input:not(:checked)") input标签中checked标签
    * 匹配所有索引值为偶数的元素，从 0 开始计数
        * $("tr:even")
* jQuery获取元素
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
* jQuery事件
    * jQuery里面的事件是DOM事件名去掉前面的on即可, 而且是链式编程.
* jQuery效果
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
