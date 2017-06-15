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
JavaScript一种直译式脚本语言，是一种动态类型、弱类型、基于原型的语言，内置支持类型。它的解释器被称为JavaScript引擎，为浏览器的一部分，广泛用于客户端的脚本语言，最早是在HTML（标准通用标记语言下的一个应用）网页上使用，用来给HTML网页增加动态功能。[百度百科](http://baike.baidu.com/item/javascript)
* 组成部分
    * ECMAScript, 描述了该语言的语法和基本对象.
    * 文档对象模型(DOM), 描述了处理网页内容的方法和接口.
    * 浏览器对象模型(BOM), 描述了与浏览器进行交互的方法和接口.
* 作用
    * 嵌入动态文本于HTML页面.
    * 对浏览器事件做出响应. 
    * 读写HTML元素.
    * 在数据被提交到服务器之前验证数据。[4] 
    * 检测访客的浏览器信息.
    * 控制cookies，包括创建和修改等.
    * 基于Node.js技术进行服务器端编程.
* <font size=3 color=orange>HTML引入JS的方式</font>
    * 方式1: 在单独文件中定义JS代码. 使用`<script></script>`的src属性来引入.
        * <font color=red>一但使用了src属性, 那么script标签体中代码将不会执行</font>
    * 方式2: 可以在`<script></script>标签里面书写JS`, 一般放在head中.
    >   <script type="text/javascript"&gt;
    //JScode
    </script&gt;
* <font size=3 color=orange>EMCAScript里面的语法</font>
    * 区分大小写
        * 与 Java 一样，变量、函数名、运算符以及其他一切东西都是区分大小写的。
        * var Test;和var test;是完全两个变量
    * 变量是弱类型的
        * 与 Java 和 C 不同，ECMAScript 中的变量无特定的类型，定义变量时只用 var 运算符，可以将它初始化为任意值, 可以随时改变变量所存数据的类型(但是尽量避免这样做).
    * 每行结尾的分号可有可无
        * 最好的代码编写习惯是总加入分号，因为没有分号，有些浏览器就不能正确运行.
    * 注释与 Java、C 和 PHP 语言的注释相同
        * 单行注释使用 //
        * 多行注释使用/\* 注释部分 \*/
    * 括号表示代码块, 使用{}括起来
        * 例如
        > if (test1 == "red") {
            &emsp;test1 = "blue";
            &emsp;alert(test1);
            }
* <font size=3 color=orange>EMCAScript里面的变量</font>
    * 声明
        * 使用var 声明, 无需明确的类型声明.
            > var text = 123;
        * <font size=3 color=red>var也可以省略, 此时会自动创建一个全局变量,初始化为指定值. 但是不推荐这么做, 这样不利于变量的跟踪.</font>
            >   title = "你好么?";
            alert(title); 
        * 可以先声明, 再赋值
            > var text;  text = 123;
        * 一个var可以声明多个变量, 并且可以是不同类型
            > var test = "hi", age = 25;
        * <font size=3 color=red>使用变量是可以改变存放数据的类型, 但是不推荐这么做.</font>
            > var text = "hello";
            text = 25;
    * 命名
        * 第一个字符必须是字母、下划线（_）或美元符号（$）
        * 余下的字符可以是下划线、美元符号或任何字母或数字字符
        * 在开发中还遵守其他命名规则
            * Camel 标记法: 首字母是小写的，接下来的字母都以大写字符开头.
            * Pascal 标记法: 首字母是大写的，接下来的字母都以大写字符开头.
            * 匈牙利类型标记法: 在以 Pascal 标记法命名的变量前附加一个小写字母（或小写字母序列），说明该变量的类型.
* EMCAScript关键字
    * 关键字是保留的，不能用作变量名或函数名.下面是ECMA-262的一些关键字. 
    * break、case、catch、continue、default、delete、do、else、finally、for、function、if、in、instanceof、new、return、switch、this、throw、try、typeof、var、void、while、with
* EMCAScript保留字
    * 留字在某种意思上是为将来的关键字而保留的单词。因此保留字不能被用作变量名或函数名.下面是ECMA-262的一些保留字.
    * abstract、boolean、byte、char、class、const、debugger、double、enum、export、extends、final、float、goto、implements、import、int、interface、long、native、package、private、protected、public、short、static、super、synchronized、throws、transient、volatile 
* <font size=3 color=orange>EMCAScript里面的数据类型</font>
    * 原始类型
        * 存储在栈（stack）中的简单数据段，也就是说，它们的值直接存储在变量访问的位置。
        * ECMAScript 有 5 种原始类型(primitive type)，即 Undefined(未初始化)、Null(空)、Boolean(布尔)、Number(数字) 和 String(字符串. 使用单/双引号括起来的都是).
        * 通过<font size=3 color=red>typeof</font>运算符可以判断一个值或者变量是否属于原始类型.
            >   var sTemp = "test string";
            alert (typeof sTemp);    //输出 "string"
            alert (typeof 86);    //输出 "number"
        * undefined - 如果变量是 Undefined 类型的
            &emsp;boolean - 如果变量是 Boolean 类型的
            &emsp;number - 如果变量是 Number 类型的
            &emsp;string - 如果变量是 String 类型的
            &emsp;object - 如果变量是一种引用类型或 Null 类型的, 现在，null 被认为是对象的占位符.
        * <font color=orange>原始类型中的String也是伪对象, 可以调用String对象里面的方法.</font>
    * 引用类型
        * 存储在堆（heap）中的对象，也就是说，存储在变量处的值是一个指针（point），指向存储对象的内存处。
        * var o = new Object(); 这种语法与 Java 语言的相似，不过当有不止一个参数时，ECMAScript 要求使用括号。如果没有参数，如以下代码所示，括号可以省略var o = new Object;<font size=4 color=red>尽管括号不是必需的，但是为了避免混乱，最好使用括号.</fong>
        * <font color=orange>Object 对象</font>, ECMAScript 中的所有对象都由这个对象继承而来.
            * Object 对象具有下列属性：
                * constructor 对创建对象的函数的引用（指针）。对于 Object 对象，该指针指向原始的 Object() 函数。
                * Prototype 对该对象的对象原型的引用。对于所有的对象，它默认返回 Object 对象的一个实例。
            * Object 对象还具有几个方法：
                * hasOwnProperty(property) 判断对象是否有某个特定的属性。必须用字符串指定该属性。（例如，o.hasOwnProperty("name")）
                * IsPrototypeOf(object) 判断该对象是否为另一个对象的原型。
                * PropertyIsEnumerable 判断给定的属性是否可以用 for...in 语句进行枚举。
                * ToString() 返回对象的原始字符串表示。对于 Object 对象，ECMA-262 没有定义这个值，所以不同的 ECMAScript 实现具有不同的值。
                * ValueOf() 返回最适合该对象的原始值。对于许多对象，该方法返回的值都与 ToString() 的返回值相同。
        * <font color=orange>Boolean 对象</font>, 是Boolean 原始类型的引用类型
            * Boolean 对象将覆盖 Object 对象的 ValueOf() 方法，返回原始值，即 true 和 false。ToString() 方法也会被覆盖，返回字符串 "true" 或 "false"
            * 最好还是使用 Boolean 原始值，避免发生Boolen对象和Boolen原始值比较时出现错误.
            >   var oFalseObject = new Boolean(false);
            var bResult = oFalseObject && true;	//输出 true
        * <font color=orange>Number 对象</font>
            * toFixed() 方法返回的是具有指定位数小数的数字的字符串表示
            >   var oNumberObject = new Number(68);
            alert(oNumberObject.toFixed(2));  //输出 "68.00"
            * toExponential() 方法返回的是用科学计数法表示的数字的字符串形式
            >   var oNumberObject = new Number(68);
            alert(oNumberObject.toExponential(1));  //输出 "6.8e+1"
            * toPrecision() 方法根据最有意义的形式来返回数字的预定形式或指数形式。
            >   var oNumberObject = new Number(68);
            alert(oNumberObject.toPrecision(1));  //输出 "7e+1"
            * toFixed()、toExponential() 和 toPrecision() 方法都会进行舍入操作，以便用正确的小数位数正确地表示一个数。
            * 与 Boolean 对象相似，Number 对象也很重要，不过应该少用这种对象，以避免潜在的问题。只要可能，都使用数字的原始表示法。
        * <font color=orange>String 对象</font>
            * 常用方法
                * charAt()	返回在指定位置的字符。
                * charCodeAt()	返回在指定的位置的字符的 Unicode 编码。
                * concat()	连接字符串。
                * indexOf()	检索字符串。
                * lastIndexOf()	从后向前搜索字符串。
                * link()	将字符串显示为链接。
                * localeCompare()	用本地特定的顺序来比较两个字符串。
                * match()	找到一个或多个正在表达式的匹配。
                * replace()	替换与正则表达式匹配的子串。
                * search()	检索与正则表达式相匹配的值。
                * slice()	提取字符串的片断，并在新的字符串中返回被提取的部分。
                * substr()	从起始索引号提取字符串中指定数目的字符.
                * substring()	提取字符串中两个指定的索引号之间的字符。
                *  toLocaleLowerCase()	把字符串转换为小写。	
                * toLocaleUpperCase()	把字符串转换为大写。	
                * toLowerCase()	把字符串转换为小写。	
                * toUpperCase()	把字符串转换为大写。
        * <font color=orange>Array 对象</font>用于在单个的变量中存储多个值, 数组是可变的, 可以存放任意值.
            * 创建 Array 对象的语法：
            >   var arr1 = new Array();//长度为0
            var arr2 = new Array(size); //指定长度
            var arr3 = new Array(element0, element0, ..., elementn);//指定元素
            var arr4 = ["a", "b", "c"];//非官方写法
            * 常用属性
                * length	设置或返回数组中元素的数目。
                * 访问大于或者等于它长度的会是undefined
            * 常用方法
                * 
* <font size=3 color=orange>ECMAScript 类型转换</font>
    * 转换成字符串
        * Boolean 值、Number值和字符串的原始值都有toString() 方法, 会把对应的值转为字符串.
            * Boolen类型
                * >   var bFound = false;
                alert(bFound.toString());	//输出 "false"
            * Number类型toString() 方法比较特殊，它有两种模式，即默认模式和基模式。
                * 默认模式:无论最初采用什么表示法声明数字，Number 类型的 toString() 方法返回的都是数字的十进制表示。因此，以八进制或十六进制字面量形式声明的数字输出的都是十进制形式的。
                    >   var iNum1 = 10;
                    var iNum2 = 10.0;
                    alert(iNum1.toString());	//输出 "10"
                    alert(iNum2.toString());	//输出 "10"
                * 基模式:可以用不同的基输出数字，例如二进制的基是 2，八进制的基是 8，十六进制的基是 16
                    >   var iNum = 10;
                    alert(iNum1.toString(2));	//输出 "1010"
                    alert(iNum1.toString(8));	//输出 "12"
                    alert(iNum1.toString(16));	//输出 "A"
                    //对数字调用 toString(10) 与调用 toString() 相同，它们返回的都是该数字的十进制形式。
        * 转换成数字
            * 只有对 String 类型调用这些方法，它们才能正确运行；对其他类型返回的都是 NaN。
            * ECMAScript 提供了两种把非数字的原始值转换成数字的方法，即 parseInt() 和 parseFloat()。前者把值转换成整数，后者把值转换成浮点数
                * parseInt()
                    * <font color=orange>parseInt() 方法首先查看位置 0 处的字符，判断它是否是个有效数字；如果不是，该方法将返回 NaN，不再继续执行其他操作。但如果该字符是有效数字，该方法将查看位置 1 处的字符，进行同样的测试。这一过程将持续到发现非有效数字的字符为止，此时 parseInt() 将把该字符之前的字符串转换成数字。
                        例如，如果要把字符串 "12345red" 转换成整数，那么 parseInt() 将返回 12345，因为当它检查到字符 r 时，就会停止检测过程。</font>
                    * <font color=orange>字符串中包含的数字字面量会被正确转换为数字，比如 "0xA" 会被正确转换为数字 10。不过，字符串 "22.5" 将被转换成 22，因为对于整数来说，小数点是无效字符。</font>
                        >   ar iNum1 = parseInt("12345red");	//返回 12345
                        var iNum1 = parseInt("0xA");	//返回 10
                        var iNum1 = parseInt("56.9");	//返回 56
                        var iNum1 = parseInt("red");	//返回 NaN
                    * parseInt() 方法还有基模式，可以把二进制、八进制、十六进制或其他任何进制的字符串转换成整数
                        >   var iNum1 = parseInt("AF", 16);	//返回 175
                        var iNum1 = parseInt("10", 2);	//返回 2
                        var iNum2 = parseInt("10", 8);	//返回 8
                        var iNum3 = parseInt("10", 10);	//返回 10
                    * 十进制中前面包含了0,  那么默认得到8进制数的值.
                        > var iNum1 = parseInt("010");	//返回 8
                        var iNum2 = parseInt("010", 8);	//返回 8
                        var iNum3 = parseInt("010", 10);	//返回 10
                * parseFloat()
                    * parseFloat() 方法与 parseInt() 方法的处理方式相似，从位置 0 开始查看每个字符，直到找到第一个非有效的字符为止，然后把该字符之前的字符串转换成整数。
                        不过，对于这个方法来说，第一个出现的小数点是有效字符。如果有两个小数点，第二个小数点将被看作无效的。parseFloat() 会把这个小数点之前的字符转换成数字。这意味着字符串 "11.22.33" 将被解析成 11.22。
                        使用 parseFloat() 方法的另一不同之处在于，字符串必须以十进制形式表示浮点数，而不是用八进制或十六进制。该方法会忽略前导 0，所以八进制数 0102 将被解析为 102。对于十六进制数 0xA，该方法将返回 NaN，因为在浮点数中，x 不是有效字符。
                    * parseFloat() 方法也没有基模式。
                        >   var fNum1 = parseFloat("12345red");	//返回 12345
                        var fNum2 = parseFloat("0xA");	//返回 NaN
                        var fNum3 = parseFloat("11.2");	//返回 11.2
                        var fNum4 = parseFloat("11.22.33");	//返回 11.22
                        var fNum5 = parseFloat("0102");	//返回 102
                        var fNum1 = parseFloat("red");	//返回 NaN
        * 强制类型转换
            * 可以使用强制类型转换（type casting）来处理转换值的类型。使用强制类型转换可以访问特定的值，即使它是另一种类型的。
            * ECMAScript 中可用的 3 种强制类型转换如下：
                * Boolean(value) - 把给定的值转换成 Boolean 型；
                    * 当要转换的值是至少有一个字符的字符串、非 0 数字或对象时，Boolean() 函数将返回 true。如果该值是空字符串、数字 0、undefined 或 null，它将返回 false
                        >   var b1 = Boolean("");		//false - 空字符串
                        var b2 = Boolean("hello");		//true - 非空字符串
                        var b1 = Boolean(50);		//true - 非零数字
                        var b1 = Boolean(null);		//false - null
                        var b1 = Boolean(0);		//false - 零
                        var b1 = Boolean(new object());	//true - 对象
                *  String(value) - 把给定的值转换成字符串；
                    * <font color=orange>强制转换成字符串和调用 toString() 方法的唯一不同之处在于，对 null 和 undefined 值强制类型转换可以生成字符串而不引发错误：</font>
                    >   var s1 = String(null);	//"null"
                    var oNull = null;
                    var s2 = oNull.toString();	//会引发错误
                * Number(value) - 把给定的值转换成数字（可以是整数或浮点数）；
                    * Number() 函数的强制类型转换与 parseInt() 和 parseFloat() 方法的处理方式相似，只是它转换的是整个值，而不是部分值。
                    * 1.2.3 使用parseInt() 和 parseFloat() 分别得到1和1.2,  而使用Number("1.2.3")将返回 NaN，因为整个字符串值不能转换成数字.
                    
|用法|结果|
|:---:|:---:|
|Number(false)|0|
|Number(true)|1|
|Number(undefined)|NaN|
|Number(null)|0|
|Number("1.2")|1.2|
|Number("12")	|12|
|Number("1.2.3")|NaN|
|Number(new object())|NaN|
|Number(50)|50|

* <font size=3 color=orange>ECMAScript 运算符</font>
    * instanceof 运算符
        * 在使用 typeof 运算符时采用引用类型存储值会出现一个问题，无论引用的是什么类型的对象，它都返回 "object"。ECMAScript 引入了另一个 Java 运算符 instanceof 来解决这个问题。
        * instanceof 运算符与 typeof 运算符相似，用于识别正在处理的对象的类型。与 typeof 方法不同的是，instanceof 方法要求开发者明确地确认对象为某特定类型。
        >   var oStringObject = new String("hello world");
        alert(oStringObject instanceof String);	//输出 "true"
    * 一元运算符
        * <font color=orange>delete</font>
            * delete运算符: 删除对以前定义的对象属性或方法的引用.
            > var o = new Object;
            o.name = "David";
            alert(o.name);	//输出 "David"
            delete o.name;
            alert(o.name);	//输出 "undefined"
            * delete 运算符不能删除开发者未定义的属性和方法
                > 例如: delete o.toString;
        * <font color=orange>void</font> 
            * void运算符: 对任何值返回 undefined.
            * 没有返回值的函数真正返回的都是 undefined。
            >   从 HTML 的 <a&gt;元素调用 JavaScript 函数时。要正确做到这一点，函数不能返回有效值，否则浏览器将清空页面，只显示函数的结果。例如: <a href="javascript:window.open('about:blank')"&gt;Click me</a&gt;显示object.要避免这种效果，可以用 void 运算符调用 window.open() 函数：<a href="javascript:void(window.open('about:blank'))"&gt;Click me</a&gt;这使 window.open() 调用返回 undefined，它不是有效值，不会显示在浏览器窗口中。
        * 前增量++/前减量--运算符、后增量++/后减量--运算符
            * 直接从 C（和 Java）借用的两个运算符是前增量运算符和前减量运算符。
            * 单独使用时结果会自增/自减, 当参与运算时, 在前就先自增/自减, 取运算后的值. 在后就先取值, 然后再自增/自减.
            >   var num = 10;
            num++;
            alert(num); //11
            alert(num++); //11
            alert(num); //12
            alert(++num); //13
            alert(num) //13
        * 一元+和一元-
            * 一元 + 对于数字无本质影响
            * 一元 + 会对字符串产生影响, 会把字符串类型变为number类型.当一元加法运算符对字符串进行操作时，它计算字符串的方式与 parseInt() 相似，主要的不同是只有对以 "0x" 开头的字符串（表示十六进制数字），一元运算符才能把它转换成十进制的值。因此，用一元加法转换 "010"，得到的总是 10，而 "0xB" 将被转换成 11。
                >   var sNum = "20";
                alert(typeof sNum);	//输出 "string"
                var iNum = +sNum;
                alert(typeof iNum);	//输出 "number"
            * 一元 - 和 一元 + 对字符串的影响类似, 同时会对值取负.
    * 位运算符
        * ~(not非) 一个数的二进制每位取反
        * &(and与) 无符号右移运算两个数的二进制数同一个位置的两个数位都为1才为1
        * |(or或) 两个数的二进制数同一个位置的两个数位都为0才为0
        * ^(XOR异或) 两个数的二进制数同一个位置的两个数位相同为0, 否则为1.
        * <<(左移运算符)
            * 它把数字中的所有数位向左移动指定的数量。例如，把数字 2（等于二进制中的 10）左移 5 位，结果为 64（等于二进制中的 1000000）
            * 在左移数位时，数字右边多出 5 个空位。左移运算用 0 填充这些空位
            * 左移运算保留数字的符号位。例如，如果把 -2 左移 5 位，得到的是 -64，而不是 64。
        * &gt;&gt;(有符号右移运算)
            * 移动数位后会造成空位。这次，空位位于数字的左侧，但位于符号位之后。ECMAScript 用符号位的值填充这些空位，创建完整的数字.
        * &gt;&gt;&gt;(无符号右移运算)
            * 对于正数，无符号右移运算的结果与有符号右移运算一样
            * 负数的无符号右移运算得到的总是一个非常大的数字.
    * 逻辑运算符
        * && 逻辑与
            * 当两边都是boolen类型, 都为true才为true, 否则为false
            * <font color=orange>如果一个运算数是对象，另一个是 Boolean 值，返回该对象。
                &emsp;如果两个运算数都是对象，返回第二个对象。
                &emsp;如果某个运算数是 null，返回 null。
                &emsp;如果某个运算数是 NaN，返回 NaN。
                &emsp;如果某个运算数是 undefined，发生错误。</font>
        * || 逻辑或
            * 当两边都是boolen类型, 都为false才为false
            * <font color=orange>如果一个运算数是对象，另一个是 Boolean 值，返回该对象。
                &emsp;如果两个运算数都是对象，返回第一个对象。
                &emsp;如果某个运算数是 null，返回 null。
                &emsp;如果某个运算数是 NaN，返回 NaN。
                &emsp;如果某个运算数是 undefined，发生错误。</font>
        * ! 逻辑非
            * <font color=orange>如果运算数是对象，返回 false
            &emsp;如果运算数是数字 0，返回 true
            &emsp;如果运算数是 0 以外的任何数字，返回 false
            &emsp;如果运算数是 null，返回 true
            &emsp;如果运算数是 NaN，返回 true
            &emsp;如果运算数是 undefined，发生错误。</font>
        * || 和 && 都会出现和Java中的"短路现象"
    * 乘性运算符
        * \*用于两数相乘
            >   var iResult = 12 * 34
            
            * 处理特殊值时
            >   如果结果太大或太小，那么生成的结果是 Infinity 或 -Infinity。
            如果某个运算数是 NaN，结果为 NaN。
            Infinity 乘以 0，结果为 NaN。
            Infinity 乘以 0 以外的任何数字，结果为 Infinity 或 -Infinity。
            Infinity 乘以 Infinity，结果为 Infinity。
        * /用第二个运算数除第一个运算数
            >   var iResult = 88 /11;
              
            * 处理特殊值时
            >   如果结果太大或太小，那么生成的结果是 Infinity 或 -Infinity。
            如果某个运算数是 NaN，结果为 NaN。
            Infinity 被 Infinity 除，结果为 NaN。
            Infinity 被任何数字除，结果为 Infinity。
            0 除一个任何非无穷大的数字，结果为 NaN。
            Infinity 被 0 以外的任何数字除，结果为 Infinity 或 -Infinity。
        * %求余运算符
            >   var iResult = 26%5; //等于 1
            
            * 处理特殊值时
            >   如果被除数是 Infinity，或除数是 0，结果为 NaN。
            Infinity 被 Infinity 除，结果为 NaN。
            如果除数是无穷大的数，结果为被除数。
            如果被除数为 0，结果为 0。
    * 加性运算符
        * \+ 加性运算符
        * 处理特殊值时
        >   某个运算数是 NaN，那么结果为 NaN。
        -Infinity 加 -Infinity，结果为 -Infinity。
        Infinity 加 -Infinity，结果为 NaN。
        +0 加 +0，结果为 +0。
        -0 加 +0，结果为 +0。
        -0 加 -0，结果为 -0。
        不过，如果某个运算数是字符串，那么采用下列规则：
        如果两个运算数都是字符串，把第二个字符串连接到第一个上。
        如果只有一个运算数是字符串，把另一个运算数转换成字符串，结果是两个字符串连接成的字符串。
    
        * \- 减法运算符
        >   某个运算数是 NaN，那么结果为 NaN。
        -Infinity 减 Infinity，结果为 NaN。
        -Infinity 减 -Infinity，结果为 NaN。
        Infinity 减 -Infinity，结果为 Infinity。
        -Infinity 减 Infinity，结果为 -Infinity。
        +0 减 +0，结果为 +0。
        -0 减 -0，结果为 -0。
        +0 减 -0，结果为 +0。
        某个运算符不是数字，那么结果为 NaN。
    * 关系运算符
        * \>(大于)、<(小于)、\>=(大于等于)、<=(小于大于), 都返回一个boolen类型
        * 常规比较方式, 比较两个Number类型 或者两个String类型
            * 都是Number类型时 根据值大小判断
            * 都是String类型时,  通过对应位置上面字符的数值来判断
            >   var bResult = "25" < "3";
            //两个运算数都是字符串，所以比较的是它们的字符代码（"2" 的字符代码是 50，"3" 的字符代码是 51）。
            alert(bResult);	//输出 "true"
        * 比较数字和字符串
            * 当比较数字和字符串形式的数字时, 会把字符串转为数值进行比较.
            >   var bResult = "25" < 3;
            //字符串 "25" 将被转换成数字 25，然后与数字 3 进行比较
            alert(bResult);	//输出 "false"
        * 字符串不能转换成数字时
        >   var bResult = "a" < 3;
            //a不能转为数字, 如果对它调用 parseInt() 方法，返回的是 NaN。根据规则，任何包含 NaN 的关系运算符都要返回 false，因此这段代码也输出 false：
            alert(bResult);

    * 条件运算符 表达式 : value1 ? value2
        >   var iMax = (iNum1 > iNum2) ? iNum1 : iNum2;
    * 赋值运算符
        * = 只是把等号右边的值赋予等号左边的变量。
        * 每种主要的算术运算以及其他几个运算都有复合赋值运算符
            >   乘法/赋值（*=）
            除法/赋值（/=）
            取模/赋值（%=）
            加法/赋值（+=）
            减法/赋值（-=）
            左移/赋值（<<=）
            有符号右移/赋值（>>=）
            无符号右移/赋值（>>>=）
    * 逗号运算符
        * 用逗号运算符可以在一条语句中执行多个运算。
        * 逗号运算符常用变量声明中。
            >   var iNum1 = 1, iNum = 2, iNum3 = 3;
    * 等性运算符
        * 全等号(===)和非全等号(!==)
            * 这两个运算符所做的与等号和非等号相同，只是它们在检查相等性前，不执行类型转换。
                >   ar sNum = "66";
                var iNum = 66;
                alert(sNum == iNum);	//输出 "true"
                alert(sNum === iNum);	//输出 "false"
        * 等号和非等号
            * ==等号 当且仅当两个运算数相等时，它返回 true
            * !=不等号 当且仅当两个运算数不相等时，它返回 true
            * 这两个运算符都会进行类型转换
            * 执行类型转换的规则
                >   如果一个运算数是 Boolean 值，在检查相等性之前，把它转换成数字值。false 转换成 0，true 为 1。
                如果一个运算数是字符串，另一个是数字，在检查相等性之前，要尝试把字符串转换成数字。
                如果一个运算数是对象，另一个是字符串，在检查相等性之前，要尝试把对象转换成字符串。
                如果一个运算数是对象，另一个是数字，在检查相等性之前，要尝试把对象转换成数字。
            * 运算符还遵守下列规则
                >   值 null 和 undefined 相等。
                在检查相等性时，不能把 null 和 undefined 转换成其他值。
                如果某个运算数是 NaN，等号将返回 false，非等号将返回 true。
                如果两个运算数都是对象，那么比较的是它们的引用值。如果两个运算数指向同一对象，那么等号返回 true，否则两个运算数不等。
                即使两个数都是 NaN，等号仍然返回 false，因为根据规则，NaN 不等于 NaN

|表达式|值|
|:---:|:---:|        
|null == undefined	|true|
|"NaN" == NaN	|false|
|5 == NaN	|false|
|NaN == NaN|	false|
|NaN != NaN	|true|
|false == 0	|true|
|true == 1|	true|
|true == 2	|false|
|undefined == 0	|false|
|null == 0|	false|
|"5" == 5|true|   

* <font size=3 color=orange>ECMAScript 语句</font>	
	* if语句， 和Java中一样
	>    if (i > 30) {
  	&emsp;alert("大于 30");
	} else if (i < 0) {
  	&emsp;alert("小于 0");
	} else {
  	&emsp;alert("在 0 到 30 之间");
	}
	* 迭代语句又叫循环语句
		* do-while 语句
		>   var i = 0;
		do {
		&emsp;i += 2;
		} while (i < 10);

		* while语句
		>   var i = 0;
		while (i < 10) {
	 	&emsp;i += 2;
		}

		* for语句
		>    iCount = 6;
		for (var i = 0; i < iCount; i++) {
  		&emsp;alert(i);
		}

		* for-in语句
		>    for (sProp in window) {
  		&emsp;alert(sProp);
		}
	* 标签语句，以便以后调用
	>    start : i = 5;
	//标签 start 可以被之后的 break 或 continue 语句引用
	
	* break 和 continue 语句
		*  break 语句可以立即退出循环，阻止再次反复执行任何代码
		*  continue 语句只是退出当前循环，根据控制表达式还允许继续进行下一次循环
		*  与有标签的语句一起使用
			* break 语句和 continue 语句都可以与有标签的语句联合使用，返回代码中的特定位置。
			>    //break
			var iNum = 0;
			outermost:
			for (var i=0; i<10; i++) {
 			&emsp;for (var j=0; j<10; j++) {
    		&emsp;&emsp;if (i == 5 && j == 5) {
    		&emsp;&emsp;&emsp;break outermost;
  			&emsp;&emsp;}
  			&emsp;&emsp;iNum++;
  			&emsp;}
			}
			alert(iNum);	//输出 "55"
			//continus
			var iNum = 0;
			outermost:
			for (var i=0; i<10; i++) {
  			&emsp;for (var j=0; j<10; j++) {
    		&emsp;&emsp;if (i == 5 && j == 5) {
    		&emsp;&emsp;&emsp;continue outermost;
  			&emsp;&emsp;}
  			&emsp;&emsp;iNum++;
  			&emsp;}
			}
			alert(iNum);	//输出 "95"
	* with语句， 用于设置代码在特定对象中的作用域。
	>    var sMessage = "hello";
	with(sMessage) {
  	&emsp;alert(toUpperCase());	//输出 "HELLO"
	}
	//在这个例子中，with 语句用于字符串，所以在调用 toUpperCase() 方法时，解释程序将检查该方法是否是本地函数。如果不是，它将检查伪对象 sMessage，看它是否为该对象的方法。然后，alert 输出 "HELLO"，因为解释程序找到了字符串 "hello" 的 toUpperCase() 方法。
	with 语句是运行缓慢的代码块，尤其是在已设置了属性值时。大多数情况下，如果可能，最好避免使用它
	* switch语句， 可以用来比较字符串，而Java是在JDK1.7才可以,还能用不是常量的值说明情况。
	>    var BLUE = "blue", RED = "red", GREEN  = "green";
	switch (sColor) {
  	&emsp;case BLUE: alert("Blue");
    &emsp;break;
  	&emsp;case RED: alert("Red");
    &emsp;break;
  	&emsp;case GREEN: alert("Green");
    &emsp;break;
 	&emsp;default: alert("Other");
	}
* <font size=3 color=orange>ECMAScript 函数</font>
    * 函数概述
        * 声明：关键字 function、函数名、一组参数，以及置于括号中的待执行代码, 即使函数确实有值，也不必明确地声明它的返回值类型。
        >    function sayHi(sName, sMessage) {
        &emsp;alert("Hello " + sName + sMessage);
        }
        * 函数在执行过 return 语句后立即停止代码。因此，return 语句后的代码都不会被执行。
    * arguments对象
        * 在函数代码中，使用特殊对象 arguments，开发者无需明确指出参数名，就能访问它们
        >   function sayHi() {
        &emsp;if (arguments[0] == "bye") {
        &emsp;&emsp;return;
        &emsp;}
        &emsp;alert(arguments[0]);
        }
        //也可以这样定义
        var sayHi = function() {
        &emsp;if (arguments[0] == "bye") {
        &emsp;&emsp;return;
        &emsp;}
        &emsp;alert(arguments[0]);
        }
        * 检测参数个数
            * 引用属性 arguments.length 即可
    * Function函数
        * 函数实际上是功能完整的对象
        >   function sayHi(sName, sMessage) {
        &emsp;alert("Hello " + sName + sMessage);
        }
        //上面等价于
        var sayHi = 
        new Function("sName", "sMessage", "alert(\"Hello \" + sName + sMessage);");
        * 尽管可以使用 Function 构造函数创建函数，但最好不要使用它，因为用它定义函数比用传统方式要慢得多。不过，所有函数都应看作 Function 类的实例。
        * Function 对象的 length 属性, 可以获取函数期望的参数个数.
        * 函数的参数也可以传入this或this.属性值, 表示当前的元素(当前DOM对象), 那么会把该元素作为参数传递进入, 这样可以不用通过document.getElementByxxx获取元素了
        * Function对象的方法
            * Function 对象也有与所有对象共享的 valueOf() 方法和 toString() 方法。这两个方法返回的都是函数的源代码，在调试时尤其有用。
* <font size=3 color=orange>HTML DOM 对象(Document 对象, 文档对象)</font>
    * <font color=orange>Document 对象</font>,每个载入浏览器的 HTML 文档都会成为 Document 对象。Document 对象使我们可以从脚本中对 HTML 页面中的所有元素进行访问, 每个元素标签组成了一个树形结构.
        * 节点
            * 文档节点: documen
            * 元素节点: element
                * appendChild() 可以添加子元素
            * 属性节点; attribute
            * 文本节点: text
        * Document 对象常用属性
            * cookie 设置或返回与当前文档有关的所有 cookie。
            * domain 返回当前文档的域名。
            * title 返回当前文档的标题。
            * URL 返回当前文档的 URL。
         * Document 对象方法
            * getElementById()	返回对拥有指定 id 的第一个对象的引用。
            * getElementsByName()	返回带有指定名称的对象集合, 需要给元素添加name属性
            * getElementsByClassName()	返回带有指定名称的对象集合,需要给元素添加class属性
            * getElementsByTagName()	返回带有指定标签名的对象集合, 如获取所有的div
            * write() 向文档写 HTML 表达式 或 JavaScript 代码。
            * writeln()	等同于 write() 方法，不同的是在每个表达式之后写一个换行符。
            * 可以获取/设置标签体的属性
                * innerHTML 获取HTML 元素的标签体中的内容.
                * innerText 获取/设置元素的文本值
                * value 获取/设置value值
                * style 获取/设置标签体的样式
    * <font color=orange>Event 对象</font>, 代表事件的状态，比如事件在其中发生的元素、键盘按键的状态、鼠标的位置、鼠标按钮的状态。事件通常与函数结合使用，函数不会在事件发生前被执行！
        * 常用事件
            * onclick	当用户点击某个对象时调用的事件句柄。
            * ondblclick	当用户双击某个对象时调用的事件句柄。
            * onblur	元素失去焦点。
            * onfocus	元素获得焦点。
            * onsubmit	确认按钮被点击, 加在form表单上面, onsubmit = "return 函数名" , 这个函数必须是返回true或者false.
            * onload	一张页面或一幅图像完成加载。
            * onchange 当改变的时候
            * onunload 用户退出事件
            * onbeforeunload 用户即将退出事件
        * 了解的方法
            * preventDefault()	通知浏览器不要执行与事件关联的默认动作, 事件不会执行
            * 对于IE浏览器设置returnValue属性也可以, 如果设置了该属性，它的值比事件句柄的返回值优先级高。把这个属性设置为 fasle，可以取消发生事件的源元素的默认动作。
            >   function showAlert(event) {
            &emsp;var event = event || window.event; //兼容性问题, IE浏览器有自己的event, 通过window.event获取, 那么参数里面的就是空
            &emsp;event. preventDefault();
            }
            * stopPropagation()	 不再派发事件。
                * 当父元素有点击事件, 而子事件也有点击事件, 如果不阻止, 那么点击子元素, 父元素的点击事件也会触发. 这是可以在子元素的点击事件上面使用此方法, 阻止事件继续派发.
                * 对于IE浏览器设置cancelBubble属性也可以,	如果事件句柄想阻止事件传播到包容对象，必须把该属性设为 true。
        * <font color=orange>JS事件和函数的绑定</font>
            * 方式1: 通过标签的事件属性 `<xxx onclick="函数名(参数)"></xxx>`
            >   //定义js函数
            function show(message) {
            &emsp;alert(message);
            }
            //通过onclick事件绑定
            `<input type="button" value="点击试试" onclick="show('哈哈')" />`	
            * 方式2: 给元素派发事件
                * 先获取元素, 然后通过.onclick = function 函数名(参数) {...}或者函数名
                * <font color=red>这种方式需要注意: 因为HTML是注释型语言, 会逐条执行, 如果执行获取元素标签无法获取元素, 就会报错 </font>
                    * 可以把获取元素的代码放在html的结尾
                    * 也可以在body的onload事件中执行该代码.
                >   document.getElementById('haha').onclick = function login() {
                &emsp;alert('登录');
                }
                //等价于 先定义函数, 后赋值
                var login = function() {
                &emsp;alert('登录');
                }
                document.getElementById('haha').onclick = login;
    * <font color=orange>Style 对象</font>
        * 使用 Style 对象属性的语法：
        >   document.getElementById("id").style.property="值"
        * 可以修改背景(Background)、边框(Border)和边距(Margin)、布局(Layout)、列表(List)、杂项()、定位(Positioning)、打印(Printing)、滚动条(Scrollbar)、表格(Table)、文本(Text)、规范
        * style对象的属性是css的属性去掉-, 后面的首首字母大写
* <font size=3 color=orange>BOM 对象(Browser 对象, 浏览器对象)</font>
    * 一般每个浏览器都5个对象
        * window: 窗口
        * location: 定位信息(地址栏)
        * history: 历史记录
        * navigator: 浏览器的信息
        * screen: 客户端屏幕的信息
    * <font color=orange>Window 对象</font>表示浏览器中打开的窗口, <font color=red>如果文档包含框架（frame 或 iframe 标签），浏览器会为 HTML 文档创建一个 window 对象，并为每个框架创建一个额外的 window 对象。</font>
        * 通过window可以获取其他几个BOM对象.
        * 常用方法, window方法可以省略window，直接使用方法。
            * alert()	显示带有一段消息和一个确认按钮的警告框。
            * clearInterval()	取消由 setInterval() 设置的 timeout。
            * clearTimeout()	取消由 setTimeout() 方法设置的 timeout。
            * open() 打开浏览器窗口
            * close()	关闭浏览器窗口。
            * confirm()	显示带有一段消息以及确认按钮和取消按钮的对话框。
            * prompt() 带输入框的弹出框
            * print()	打印当前窗口的内容。
            * setInterval()	按照指定的周期（以毫秒计）来调用函数或计算表达式。(重复执行)
	        	* code可以放函数名， 不加引号
	        	* 函数名加引号， 必须带（）
	        	* 也可以放新创建的function， 有点像Java中匿名内部类
            * setTimeout()	在指定的毫秒数后调用函数或计算表达式。(只执行一次)
	* <font color=orange>Location 对象</font>
		* Location 对象包含有关当前 URL 的信息。
		* Location 对象是 Window 对象的一个部分，可通过 window.location 属性来访问。
		* 常用方法
			* assign() 加载新的文档。 
			* reload() 重新加载当前文档。 
			* replace() 用新的文档替换当前文档。 
		* 常用属性
			* href 设置或返回完整的 URL。 
				* 修改href可以实现页面跳转,也可以省略href直接修改location
			* host 设置或返回主机名和当前 URL 的端口号。 
			* port 设置或返回当前 URL 的端口号。 
	* <font color=orange>Location 对象</font>， 它包含用户（在浏览器窗口中）访问过的 URL
		* History 对象属性
			* length 返回浏览器历史列表中的 URL 数量。 
		* History 对象方法
			* back() 加载 history 列表中的前一个 URL。 
			* forward() 加载 history 列表中的下一个 URL。 
			* go() 加载 history 列表中的某个具体页面。 
			>   history.go(-2) //后退两次
