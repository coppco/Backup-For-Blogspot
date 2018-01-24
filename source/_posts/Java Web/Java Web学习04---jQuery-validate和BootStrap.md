layout: java
title: Java Web学习04---jQuery_validate和BootStrap
date: 2016-07-18 16:30:38
tags:
    - JavaScript
    - Java
    - jQuery
    - BootStrap
---

jQuery-validate是jQuery的一个插件, 有助于我们进行表单校验, 而BootStrap是一个HTML+CSS+JavaScript的类库.

<!--more-->

## <font color=orange>jQuery-validate</font>
jQuery-validate是jQuery的一个插件, 它可以用来进行表单校验, 它必须在jQuery的基础上面进行运行. 所以首先需要导入jQuery.js, 然后需要在页面加载成功之后校验.
[官网下载地址](http://jqueryvalidation.org/files/jquery-validation-1.15.0.zip)
[帮助文档位置](http://jqueryvalidation.org/documentation/)

* 检验器查询表

|校验类型	|取值|	描述|
|:---:|:---:|:---:|
|required	|true或false|	必填字段|
|email	|"@"或者"email"|	邮件地址|
|url	||	路径|
|date	 |数字|	日期|
|dateISO	|字符串|	日期（YYYY-MM-dd）|
|number		||数字（负数，小数）|
|digits		||整数|
|minlength	|数字|	最小长度|
|maxlength	|数字|	最大长度|
|rangelength	|[minL,maxL]	|长度范围|
|min		||最小值|
|max		||最大值|
|range	|[min,max]	|值范围|
|equalTo|	jQuery表达式|	两个值相同|
|remote	 |url路径|	ajax校验|

* 格式

>    <script type="text/javascript"&gt;
    &emsp;&emsp;$(function(){
    &emsp;&emsp;&emsp;&emsp;$("form元素的选择器").validate({
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;rules:{
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;//下面只能写一个校验器
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;name字段名:"校验器",
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;//下面可以写多个校验器
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;name字段名:{
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;校验器:"取值",
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;校验器:"取值"
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;},
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;name字段名:{
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;校验器:"取值",
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;校验器:"取值"
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;}
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;},
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;//messages是文本描述可以去掉
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;messages:{
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;name字段名:{
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;校验器:"提示",
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;校验器:"提示"
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;},
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;name字段名:{
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;校验器:"提示",
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;校验器:"提示"
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;}
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;}
    &emsp;&emsp;&emsp;&emsp;});
    &emsp;&emsp;});
    </script&gt;
* 使用说明和API
    * [查看](http://www.runoob.com)
* 使用示例
>   //导入jQuery库
<script type="text/javascript" src="../js/jquery-1.11.0.js" &gt;</script&gt;
//导入validate校验库
<script type="text/javascript" src="../js/jquery.validate.js" &gt;</script&gt;
//国际化库, 中文提示
<script type="text/javascript" src="../js/messages_zh.js" &gt;</script&gt;
<script type="text/javascript"&gt;
&emsp;&emsp;//在页面加载完成之后验证
&emsp;&emsp;$(function(){
&emsp;&emsp;&emsp;&emsp;//获取元素
&emsp;&emsp;&emsp;&emsp;$("#formId").validate({
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;//这里是规则
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;rules:{
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;debug: true //只验证不提交, 在调试时很方便
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;},
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;rules:{
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;username:required,
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;password:{
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;required: true,//必填
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;required: "#aa:checked",//表达式为真必须校验
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;required: function(){},//函数返回值为真必须校验
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;rangelength:[6, 20]//长度范围
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;},
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;repassword:{
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;required: true,
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;rangelength:[6, 20],
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;equalTo:"[name='password']"//相等
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;}
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;minValue:{
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;min: 5//最小值5
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;}
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;},
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;//这里是消息, 也可以不写
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;messages:{
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;password:{
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;required: "密码不能为空",
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;rangelength:"密码长度在6-20位"
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;},
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;repassword:{
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;required: "重复密码不能为空",
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;rangelength:"重复密码长度在6-20位",
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;equalTo: "两次密码不一致"
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;},
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;minValue:{
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;min: "最小值不能小于{0}"//{index}可以动态获取rules里面的对象校验器的值
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;}
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;}
&emsp;&emsp;&emsp;&emsp;});
&emsp;&emsp;});
</script&gt;

## <font color=orange>BootStrap</font>
Bootstrap是由动态CSS语言Less写成, 是基于HTML5和CSS3开发的，它在jQuery的基础上进行了更为个性化和人性化的完善，形成一套自己独有的网站风格，并兼容大部分jQuery插件, 它是响应式设计.
* 下载
    * [BootStrap中文网](http://www.bootcss.com)
* 使用
    * <font color=red>在导入BootStrap之前需要导入jQuery.js</font>
    >   <link rel="stylesheet" type="text/css" href="../css/bootstrap.min.css"/&gt;
    <script src="../js/jquery-1.11.0.js"&gt;</script&gt;//先导入jQuery.js
    <script src="../js/bootstrap.min.js"&gt;</script&gt;
    * <font color=orange>移动设备优先</font>, 对于移动设备有了很好的支持.
    >   <meta name="viewport" content="width=device-width, initial-scale=1"/&gt;

        * width:可视区域的宽度，值可为数字或关键词device-width
        * height:同width
        * intial-scale:页面首次被显示是可视区域的缩放级别，取值1.0则页面按实际尺寸显示，无任何缩放
        * maximum-scale=1.0, minimum-scale=1.0;可视区域的缩放级别，
            * maximum-scale用户可将页面放大的程序，1.0将禁止用户放大到实际尺寸之上。
        * user-scalable:是否可对页面进行缩放，no 禁止缩放
    * Boostrap需要把所有标签都放到布局容器中
        * <font color=orange>.container类</font> 用于固定宽度并支持响应式布局的容器
        >   <div class="container"&gt;</div&gt;
        * <font color=orange>.container-fluid类</font> 用于100%宽度, 占据全部视口(viewport)的容器
        >   <div class="container-fluid"&gt;</div&gt;
        * <font color=red>.container和.container-fluid不同嵌套.</font>
* 栅格系统
Bootstrap 提供了一套响应式、移动设备优先的流式栅格系统，随着屏幕或视口（viewport）尺寸的增加，系统会自动分为最多12列。它包含了易于使用的预定义类，还有强大的mixin 用于生成更具语义的布局。
    * 媒体查询, BootStrap把屏幕成为四种
        * 超小屏幕(手机, 小于768px), 没有相关代码, 在Boostrap中是默认的
            * 类前缀 <font color=orange>.col-xs-(需要占的列数, 小于12)</font>, 如col-xs-6(每行显示2个)
        * 小屏幕(平板, 大于大于768px)
        >   @media (min-width: @screen-sm-min) { ... }

            * 类前缀 <font color=orange>.col-sm-(需要占的列数, 小于12)</font>, 如col-sm-4(每行显示3个)
        * 中等屏幕(桌面显示器, 大于等于992px)
        >   @media (min-width: @screen-md-min) { ... }

            * 类前缀 <font color=orange>.col-md-(需要占的列数, 小于12)</font>, 如col-md-3(每行显示4个)
        * 大屏幕(大桌面显示器, 大于等于1200px)
        >   @media (min-width: @screen-lg-min) { ... }

            * 类前缀 <font color=orange>.col-lg-(需要占的列数, 小于12)</font>, 如col-lg-2(每行显示6个)
* 示例
>   <script type="text/javascript"&gt;
&emsp;&emsp;//在页面加载完成之后
&emsp;&emsp;$(function() {
&emsp;&emsp;&emsp;&emsp;//布局容器中的元素添加class值, 使其在大屏幕下显示6个(col-lg-2----gt;12 / 2),  中等屏幕下显示4个
&emsp;&emsp;&emsp;&emsp;(col-md-3----&gt;12 / 3), 小屏幕下显示3个(col-sm-4----gt; 12 / 4), 超小屏幕下显示2个(col-xs-6----gt; 12 / 6).
&emsp;&emsp;&emsp;&emsp;$("#container-fluid>div").addClass("col-lg-2").addClass("col-md-3").addClass("col-sm-4")
&emsp;&emsp;&emsp;&emsp;.addClass("col-xs-6");
&emsp;&emsp;});
</script&gt;
* 组件, 一些可复用的组件, 如字体图标、下拉菜单、导航、警告框、弹出框等,具体可以查看官网或者
    * 字体图标
        * 使用注意 
            * 不要和其他组件混合使用, 图标类不能和其它组件直接联合使用。它们不能在同一个元素上与其他类共同存在。应该创建一个嵌套的 <span> 标签，并将图标类应用到这个 <span> 标签上。
            * 只对内容为空的元素起作用, 图标类只能应用在不包含任何文本内容或子元素的元素上。
    >   <button type="button" class="btn btn-default" aria-label="Left Align"&gt;
    &emsp;<span class="glyphicon glyphicon-align-left" aria-hidden="true"&gt;</span&gt;
    </button&gt;
    <button type="button" class="btn btn-default btn-lg"&gt;
    &emsp;<span class="glyphicon glyphicon-star" aria-hidden="true"&gt;</span&gt; Star
    </button&gt;

    * 下拉菜单, 用于显示链接列表的可切换、有上下文的菜单。
        * 将下拉菜单触发器和下拉菜单都包裹在 .dropdown 里，或者另一个声明了 position: relative; 的元素。
>   <div class="dropdown"&gt;
    &emsp;<button class="btn btn-default dropdown-toggle" type="button" id="dropdownMenu1" data-toggle="dropdown" aria-haspopup="true" aria-expanded="true"&gt;
    &emsp;&emsp;Dropdown
    &emsp;&emsp;<span class="caret"&gt;</span&gt;
&emsp;</button&gt;
&emsp;<ul class="dropdown-menu" aria-labelledby="dropdownMenu1"&gt;
&emsp;&emsp;<li&gt;<a href="#"&gt;Action</a&gt;</li&gt;
&emsp;&emsp;<li&gt;<a href="#"&gt;Another action</a&gt;</li&gt;
&emsp;&emsp;<li&gt;<a href="#"&gt;Something else here</a&gt;</li&gt;
&emsp;&emsp;<li role="separator" class="divider"&gt;</li&gt;
&emsp;&emsp;<li&gt;<a href="#"&gt;Separated link</a&gt;</li&gt;
&emsp;</ul&gt;
</div&gt;
    * 导航条
        * 在您的应用或网站中作为导航页头的响应式基础组件。它们在移动设备上可以折叠（并且可开可关），且在视口（viewport）宽度增加时逐渐变为水平展开模式。
        * 务必使用 <nav&gt;元素，或者，如果使用的是通用的 <div&gt; 元素的话，务必为导航条设置 role="navigation" 属性，这样能够让使用辅助设备的用户明确知道这是一个导航区域。
        * 通过添加 .navbar-inverse 类可以改变导航条的外观。
* JavaScript 插件, 提供了过渡效果、模态框、下拉菜单、滚动监听、标签页、工具提示、弹出框、警告框、按钮、轮播图等.
