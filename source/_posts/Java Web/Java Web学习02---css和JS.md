layout: java
title: Java Web学习02---CSS和JS
date: 2016-09-25 10:19:29
tags:
	- Java
---
css是层叠样式表, 是对html进行样式修饰的语言, 多个css对同一个html修饰, 使用优先级别高的.JavaScript可以使网页动起来.可以在[w3c](http://www.w3school.com.cn/)上面学习.
<!--more-->
在实际开发中很少使用标签的属性来装饰页面, 通常是使用html + css + js来完成一个网页.

# <font size=5 color=orange>CSS</font>
CSS: 层叠样式表, 用来渲染网页, 提高工作效率.
* 后缀名
    * .css
* 格式
>   选择器 {
    &emsp;属性: 值;
    &emsp;属性: 值;
    }
* html引用css文件的方式
    * 方式1: 内联样式表, 通过标签的style属性来设置
        * 如: `<div style="background-color: blue; color: red;">天佑中华</div>`
    * 方式2: 内部样式表, 通过当前页面中使用的样式
        * 通过在html中使用`<style></style>`标签来实现
>   <style&gt;
    &emsp;#div2 {
    &emsp;&emsp;background-color:#bcff00 ;
    &emsp;}
    </style&gt;
    * 方式3: 外部样式表, 使用独立的css文件
        * 通过`<link rel="stylesheet" type="text/css" href="css文件路径"/>`

* 选择器
    * id选择器
        * 要求html元素必须要有id属性且有值.
        * 如`<div id="password"></div>`
        * css中通过`#id值{...}`来使用
    * class选择器
        * 要求html元素必须要有class属性且有值.
        * 如`<div class="name"></div>`
        * css中通过`.class值{...}`来使用
    * 元素选择器
        * 直接使用`标签名{...}`来使用
    * 属性选择器
        * html必须有一个属性不论属性是什么且有值`<div  nihao="nihao">`
        * css通过`元素名[属性="属性值"]{...}`来使用
    * 后代选择器
        * 满足父选择器中, 所有子选择器的元素
        * css通过`选择器 后代选择器{...}`来使用
    * 子选择器
        * 满足父选择器中, 所有子选择器的元素
        * css通过`选择器>后代选择器{...}`来使用
    * 子选择器和后代选择器的区别
        * 子选择器使用>表示, 而后代选择器使用空格表示
        * 子选择器只是仅仅是它的直接后代, 而后代包含间接后代.
    * 伪类选择器
        * 有时候还会需要用文档以外的其他条件来应用元素的样式，比如鼠标悬停等。这时候我们就需要用到伪类了。以下是链接应用的伪类定义。
        * 例如`<a></a>`的一些状态, 注意这个顺序不能乱了.
>   a:link{/\*未访问状态\*/
&emsp;color:#999999;
}
a:visited{/\*已访问状态\*/
&emsp;color:#FFFF00;
}
a:hover{/\*鼠标悬停在链接时状态\*/
&emsp;color:#006600;
}
a:hover{/\*选定是状态\*/
&emsp;color:#0000FF;
}
    * 通用选择器 
        * `* {font-size: 12px;}`设置所有字体12px
    * 群组选择器:当几个元素样式属性一样时，可以共同调用一个声明，元素之间用逗号分隔。
        * 例如`p, td, li {...}`
    * 相邻同胞选择器
    * 伪元素选择器
    * 结构性伪类选择器
* css的属性
    * CSS字体属性允许设置字体系列 (font-family) 和字体加粗 (font-weight)，还可以设置字体的大小、字体风格（如斜体）和字体变形（如小型大写字母).
        * font-family: 设置字体或者字体家族
        * font-size: 字体大小
        * font-style: 字体风格
    * CSS背景属性允许应用纯色作为背景，也允许使用背景图像创建相当复杂的效果.
        * background-color:背景颜色 
        * background-image: 背景图片
    * CSS文本属性可定义文本的外观.通过文本属性，可以改变文本的颜色、字符间距，对齐文本，装饰文本，对文本进行缩进、下划线( text-decoratio="none")等.
        * color: 文本颜色
        * line-height: 设置行高
        * text-decoration: 向文本添加装饰, 如下划线, 无
        * text-align: 对齐文本
    * CSS列表属性允许你放置、改变列表项标志，或者将图像作为列表项标志。
        * list-style-type: 设置列表项类型, 如1、a、实心圆等
        * list-style-image: 设置图片为列表项的类型, 使用url函数`url("../image/1.png")`
    *   CSS尺寸 (Dimension) 属性允许你控制元素的高度和宽度。同样，它允许你增加行间距。
        * width: 宽度
        * height: 高度
    * 浮动的框可以向左或向右移动，直到它的外边缘碰到包含框或另一个浮动框的边框为止。
        * float; left/right 向左/右浮动
        * clear: 设置元素两边是否允许其他元素浮动
    * CSS分类属性允许你规定如何以及在何处显示元素.
        * display: 设置是否和如何显示元素, 
            * 值为none时, 不显示该元素
            * 值为block时: 此元素将显示为块级元素, 此元素前后带换行符
            * 值为inline时: 此元素将会被显示为内联元素, 元素前后没有换行符
    * CSS框模型 (Box Model) 规定了元素框处理元素内容、内边距、边框 和 外边距 的方式。
        * 内边距(padding)、边框(border)和外边距(margin)都是可选的，默认值是零.
        * padding、border、margin可以一次设置4个值, 使用空格分隔,  表示上右下左的值.(设置值的时候可以设置1~4个值, 设置值的时候按照上右下左的顺序来设置, 如果后面没有值就取对面的值, 设置为对应方向的值).
        * border可以设置大小, 样式, 颜色等, 可以分开写, 也可以一起写`border: 5px solid red`

<font size=5 color=red>如果多个样式作用于同一个元素时, 不同的样式会叠加, 相同的样式会遵循就近原则: 一般内联样式表最高, 内部样式表和外部样式由他们在html中引入的先后位置决定, 使用后面的</font>
<font size=5 color=red>如果多个选择器作用于同一个元素时, 越特殊优先级越高.</font>




# <font size=5 color=orange>JavaScript</font>
