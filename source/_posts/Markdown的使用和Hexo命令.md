---
layout: post
title: Markdown的使用和Hexo命令
comments: true
date: 2015-10-14 18:37:52
tags:
    - First
    - 杂谈
    - 新尝试
    - Markdown
---

由于hexo使用的是markdown格式的文件,而我又不会使用markdown,所以第一篇微博就来学习一下markdown的使用和hexo的一些命令吧!
<!--more-->

# 目录
* [1、------ Markdown的基本语法](#1) 
	* [1.1、------ 兼容HTML代码](#1.1) 
	* [1.2、------ 区块元素](#1.2)   
		* [1.2.1、------ 段落](#1.2.1) 
		* [1.2.2、------ 换行](#1.2.2)　
		* [1.2.3、------ 标题](#1.2.3) 
		* [1.2.4、------ 区块引用](#1.2.4)
		* [1.2.5、------ 列表](#1.2.5)  
		* [1.2.6、------ 代码块](#1.2.6)
		* [1.2.7、------ 表格](#1.2.7)
	* [1.3、------ 区段元素](#1.3) 
		* [1.3.1、------ 强调](#1.3.1)  
		* [1.3.2、------ 链接](#1.3.2)  
		* [1.3.3、------ 图片](#1.3.3) 
		* [1.3.4、------ 代码](#1.3.4)  
		* [1.3.5、------ 反斜杠](#1.3.5) 
		* [1.3.6、------ 小型文字](#1.3.6)
		* [1.3.7、------ 博客列表页不显示全文](#1.3.7)
		* [1.3.8、------ 转义字符](#1.3.8)
	* [1.4、------ 其他](#1.4) 
		* [1.4.1、------ 分割线和删除线](#1.4.1) 
		* [1.4.2、------ 脚注](#1.4.2) 
		* [1.4.3、------ 目录](#1.4.3) 
* [2、------ hexo的一些命令](#2)
* [3、------ 后记: 备份博客](#3)

<h1 id='1' style='color: orange'> 1、 Markdown的基本语法 </h1>
<h2 id='1.1' style='color: green'> 1.1、 兼容HTML代码 </h2>

例如在Markdown文件中加入下面的HTML代码:

```html
<p>
<font color='green'>效果:</font>&emsp;&nbsp;&emsp;&nbsp;&emsp;&nbsp;<a href="https://www.baidu.com">国内只能百度一下</a>
</p>
```

<p>
<font color='green'>效果:</font>&emsp;&nbsp;&emsp;&nbsp;&emsp;&nbsp;<a href="https://www.baidu.com">国内只能百度一下</a>
</p>

+ <font color=red>注意事项</font>
	+ HTML区块标签如`<div>`、`<table>`、`<pre>`、`<p>`等必须在前后加上空行与其它内容区隔开，还要求它们的开始标签与结尾标签不能用制表符或空格来缩进。

<h2 id='1.2' style='color: green'> 1.2、 区块元素 </h2>

<h3 id='1.2.1' style='color: green'> 1.2.1、段落 </h3>

&emsp;&emsp;段落前后要有一个以上的空行（空行的定义是显示上看起来像是空的，便会被视为空行。比方说，若某一行只包含空格和制表符，则该行也会被视为空行）。

&emsp;&emsp;简单的方法就是使用</font color=red>回车</font>来进行段落分行, 在上一段落结束处连续按两次回车即可。

<h3 id='1.2.2' style='color: green'> 1.2.2、换行 </h3>

&emsp;&emsp;你可以使用html中的`<br />`标签来实现换行, 也可以在插入处<font color>先按入两个以上的空格然后回车</font>来实现。

<h3 id='1.2.3' style='color: green'> 1.2.3、标题 </h3>

标题Markdown除了可以使用HTML标签, 也可以使用另外两种方式。
* HTML标签
	* `<h1>`------`<h6>`标签来实现

```html
<p>
<font color='green'>效果:</font>
<h4>使用HTML标签来实现</h4>
</p>
```
<p>
<font color='green'>效果:</font>
<h4>使用HTML标签来实现</h4>
</p>

* Markdown中支持的方式
	* 使用`=`和`-`
		* `=`: 最高阶标题
		* `-`: 第二阶标题
	* 使用`# ~ ###### 标题`, 最多有6个`#`
		* 注意: <font color=red>`#`与标题中间至少要有一个空格</font>
		* 标题的行尾可以选择使用`#`进行闭合进行美化, 数量也可以不一样, 标题的大小只取决于前面的`#`数量
		
```
<p>
<font color='green'>效果:</font>
</p>
使用==
==
使用--
--

#   1级标题  #####
## 2级标题 
### 3级标题
#### 4级标题
##### 5级标题
###### 6级标题
```

<p>
<font color='green'>效果:</font>
</p>

使用=
==
使用--
--
#   1级标题  #####
## 2级标题 
### 3级标题
#### 4级标题
##### 5级标题
###### 6级标题 

<h3 id='1.2.4' style='color: green'> 1.2.4、区块引用 </h3>

你可以在每一行的前面添加`>`
```
> 大家好才是真的好。
> 他好我也好。
> 今年过节不收礼, 收礼只收脑白金。
> 一切皆有可能。
> 飘柔，就是这么自信
> 你是我的优乐美。
> 农夫山泉有点甜。
```
> 大家好才是真的好。
> 他好我也好。
> 今年过节不收礼, 收礼只收脑白金。
> 一切皆有可能。
> 飘柔，就是这么自信
> 你是我的优乐美。
> 农夫山泉有点甜。

当然, markdown允许你偷懒只在第一行前面添加`>`, 但是如果句子太长可能就不会很美观.
```
> 大家好才是真的好。
  他好我也好。
  今年过节不收礼, 收礼只收脑白金。
  一切皆有可能。
  飘柔，就是这么自信
  你是我的优乐美。
  农夫山泉有点甜。
```
> 大家好才是真的好。
  他好我也好。
  今年过节不收礼, 收礼只收脑白金。
  一切皆有可能。
  飘柔，就是这么自信
  你是我的优乐美。
  农夫山泉有点甜。

区块引用可以嵌套, 只要根据层次加上不同数量的`>`

```
> This is the first level of quoting.
>
> > This is nested blockquote.
>
> Back to the first level.
```

> This is the first level of quoting.
>
> > This is nested blockquote.
>
> Back to the first level.

引用的区块内也可以使用其他的 Markdown 语法，包括标题、列表、代码区块等：

```
> ## 这是一个标题。
> 
> 1.   这是第一行列表项。
> 2.   这是第二行列表项。
> 
> 给出一些例子代码：
> 
>     return shell_exec("echo $input | $markdown_script");
```

> ## 这是一个标题。
> 
> 1.   这是第一行列表项。
> 2.   这是第二行列表项。
> 
> 给出一些例子代码：
> 
>     return shell_exec("echo $input | $markdown_script");

<h3 id='1.2.5' style='color: green'> 1.2.5、列表 </h3>

Markdown 支持有序列表和无序列表。
无序列表使用星号`*`、加号`+`或是减号`-`作为列表标记。
有序列表则使用数字接着一个英文句点, <font color=red>但是你不必按照顺序书写, 但是推荐使用有序的顺序</font>。
列表项目可以包含多个段落，每个项目下的段落都必须缩进4个空格或是 1 个制表符。
<font color=red>列表项目标记通常是放在最左边，但是其实也可以缩进，最多3个空格(缩进4个空格是代码块)，项目标记后面则一定要接着至少一个空格或制表符。</font>


```
   * Red
 
+ Green
- Blue
1. 1
   3. 3
```
	
   * Red
   
+ Green
- Blue
1. 1
3. 3

列表飨项中可以嵌套使用引用、代码块和列表项, 它们都需要缩进4个空格或者一个制表符
```
* 列表
	* 列表
		1. 1
		> 引用
		2. 2
	* 列表
	> 引用
```

* 列表
	* 列表
		1. 1
		> 引用
		2. 2
	* 列表
	> 引用

<h3 id='1.2.6' style='color: green'> 1.2.6、代码块 </h3>

缩进 4 个空格或是 1 个制表符.

```
	缩进一个字表副
    
    缩进4个空格
```

	缩进一个字表副

    缩进4个空格

<font color=red>推荐使用\`\`\`把代码包裹起来, 这样写的好处是: 代码高亮显示</font>, 还有一个缺点就是不显示编程语言、没有拷贝按钮.

\`\`\`java
代码
\`\`\`

```java
pubilc static void main(String[] args) {
    System.out.println("Java");
}
```


<h3 id='1.2.7' style='color: green'> 1.2.7、表格 </h3>markdown中可以使用`<table>`标签, 也可以使用它自己的格式.
```
//其中第二行表示对齐方式,  默认、左对齐、居中、右对齐
|标题1|标题2|标题3|标题4|
|---|:---|:---:|---:|
|行1-1|行1-2|行1-3|行1-4|
|行2-1|行2-2|行2-3|行3-4|
```
<font color=green>__预览__:</font>

|标题1|标题2|标题3|标题4|
|---|:---|:---:|---:|
|行1-1|行1-2|行1-3|行1-4|
|行2-1|行2-2|行2-3|行3-4|

<h2 id='1.3' style='color: green'> 1.3、 区段元素 </h2><h3 id='1.3.1' style='color: green'> 1.3.1、强调 </h3>使用一个`_`或`*`包裹起来的是斜体, 两个包裹的是粗体.

```
  _斜体_
  __粗体__
  *斜体* 
  **粗体**
```

_斜体_
__粗体__
*斜体* 
**粗体**

<h3 id='1.3.2' style='color: green'> 1.3.2、链接 </h3>除了HTML中的`<a>`标签,Markdown 支持两种形式的链接语法： 行内式和参考式两种形式, 除了可以跳转到网页, 也可以实现页面内跳转,页面之间跳转。
<font></font>

* 内联方式   
`不会的问题请先[百度一下](https://www.baidu.com),再请教别人!`
  <font color=green>__预览__:</font>
  不会的问题请先[百度一下](https://www.baidu.com),再请教别人!

* 引用方式   
  `欢迎访问我的[github][1]和[简书][2]。`
  `[1]: https://github.com/coppco`
  `[2]: http://www.jianshu.com/users/04289820b712/timeline`
<font color=green>__预览__:</font>

欢迎访问我的[github][1]和[简书][2]。
[1]: https://github.com/coppco
[2]: http://www.jianshu.com/users/04289820b712/timeline

* 跳转到本页 
可以实现不同页面跳转, 也可以在[本页](#1)跳转, 可以通过路径或者添加id值跳转.
* 自动链接
`<http://www.baidu.com/>`
打开<http://www.baidu.com/>搜索!


<h3 id='1.3.3' style='color: green'> 1.3.3、图片 </h3>图片和链接类似,有内联方式和引用方式, 地址可以是本地文件也可以是网络URL。

* 方式1: 内联方式, 本地图片
    `![本地头像](/assets/blogImg/coppco.png)`
    <font color=green>__预览__:</font>
    ![本地头像](/assets/blogImg/coppco.png)

* 方式2: 引用方式,使用网络图片   
  `![网络图片][1]`
  `[1]: https://coppco.github.io/assets/blogImg/coppco.png` 

<font color=green>__预览__:</font>

  ![网络图片][1]
[1]: https://coppco.github.io/assets/blogImg/coppco.png

<h3 id='1.3.4' style='color: green'> 1.3.4、代码 </h3>如果要标记一小段行内代码，你可以用反引号把它包起来（\`).

`Swift`、`Objective-C`、`Java`、`c#`、`.NET`

如果要在代码区段内插入反引号，你可以用多个反引号来开启和结束代码区段：
``这有一个`, 可以看到吗``

<h3 id='1.3.5' style='color: green'> 1.3.5、反斜杠 </h3> 一些特殊字符如 \*  \[ \>等前面添加\\
Markdown 支持以下这些符号前面加上反斜杠来帮助插入普通的符号:

```
\   反斜线
`   反引号
*   星号
_   底线
{}  花括号
[]  方括号
()  括弧
#   井字号
+   加号
-   减号
.   英文句点
!   惊叹号
```
<font color=green>__预览__:</font>
\\
\`
\*
\_
\{\}
\[\]
\(\)
\#
\+
\-
\.
\!

<h3 id='1.3.6' style='color: green'> 1.3.6、小型文字 </h3>`<small>文本内容</small>`
<font color=green>__预览__:</font>
<small>文本内容</small>

<h3 id='1.3.7' style='color: green'> 1.3.7、博客列表不显示全文 </h3>

在合适的位置添加<!--more-->即可,在它前面的会显示,后面的只会在详情显示

<h3 id='1.3.8' style='color: green'> 1.3.8、转义字符 </h3>

|字符|十进制|转义字符|
|:---:|:---:|:---:|
|"|`&#34;`|`&quot;`|
|&	|`&#38;`|	`&amp;`|
|<|	`&#60;`|	`&lt;`|
|>|	`&#62;`|	`&gt;`|
|不断开空格(non-breaking space)|	`&#160;`|	`&nbsp;`|
|半方大的空白|`&ensp;`|`&#8194;`|
|全方大的空白|`&emsp;`|`&#8195;`|

<h2 id='1.4' style='color: green'> 1.4、其他 </h4><h3 id='1.4.1' style='color: green'> 1.4.1、分割线和删除线 </h3>可以使用三个以上表示分割线`-`、`*`和`_`, 使用`~`表示删除线.分割线不是标注的规范.
`---`
`***`
`~~文字删除线~~`
<font color=green>__预览__:</font>

~~文字删除线~~

<h3 id='1.4.2' style='color: green'> 1.4.2、脚注 </h3>脚注不是标准 MarkDown 的范畴.定义如下: 

```
比如Java[^1],  Swift[^2] 是这样的。

[^1]: 脚注1
[^2]: 脚注2
```

<h3 id='1.4.3' style='color: green'> 1.4.3、目录 </h3>目录也不是标准的Markdown规范, 但是可以通过其他方式实现.
例如本篇文章是通过HTML的方式实现, 

```
* [1.一级目录](#1)
    * [1.1二级目录](#1.1)
        * [1.1.1三级目录](#1.1.1)

<h2 id='1'> 一级目录 </h2>
<h4 id='1.1'> 二级目录 </h4>
<h5 id='1.1.1'> 三级目录 </h5>
```


<h1 id='2' style='color: orange'> 2、 hexo的一些命令 </h1>
<font color=red>__1、hexo new "文章名称"__</font>:    新建一个md文件,在 hexo/source/_posts/目录下, 里面的内如你也可以根据内容新建文件夹分类
<font color=red>__2、hexo clean__</font>:    当source文件夹中文件改变的时候,这个操作可以清除生成的public文件夹
<font color=red>__3、hexo g__</font>:    重新生成静态文件夹public
<font color=red>__4、hexo s__</font>:    开启本地服务打开[http://0.0.0.0:4000/](http://0.0.0.0:4000/)预览(如果本地端口冲突,可以在\node_modules\hexo-server\index.js里面修改默认ip和端口:{log: false,ip: '127.0.0.1',port: '4400'})
<font color=red>__5、hexo d__</font>:    部署到远程git上面, 主要是把生成的public文件夹上传到git

<h1 id='3' style='color: orange'> 3、 博客后记: 备份 </h1>
使用hexo+github搭建个人博客的时候,难免遇到使用不同的电脑和操作系统来写博客,我是这样解决的:把博客所在的目录也使用一个git仓库存储,这样一些资源、主题的配置可以得到备份. 以后使用不同的电脑就clone下来,当然环境node、hexo、git等都是需要安装的.  下面是备份整体目录后, 换电脑或者操作系统后的操作~~~
有些东西是不需要备份的, 可以添加忽略文件`.gitignore`, 内容如下: 
```
#过滤node_modules文件夹,过滤public文件夹
#node.js版本不同, 里面内容不同
/node_modules
#这个文件夹是可以生成的
/public
db.json
```
Windows下:
    1. 安装node.js , 可以去[官网](https://nodejs.org/en/)下载 Windows版本
    2. 安装git   [下载地址](https://git-scm.com/download/win)
    3. 进入微博备份目录 使用右键----> Git Bash,  输入命令:npm install -g hexo-cli  
    4. npm isntall(主要是.gitignore忽略了node_modules文件夹, 需要重新安装)  
Mac下:
    1. 安装node.js , 可以去[官网](https://nodejs.org/en/)下载Mac版本  
    2. 安装git   安装了Xcode, 默认会安装好了
    3. 然后进入博客文件夹使用sudo npm install hexo -server
    4. npm install(主要是.gitignore忽略了node_modules文件夹, 需要重新安装)  
温馨提示: 有时候执行npm install非常慢, 可以把npm换成淘宝镜像
`
1.通过config命令(永久)
>npm config set registry https://registry.npm.taobao.org 
>npm info express  或者 npm config get registry 来验证（如果上面配置正确这个命令会有字符串response）

2.命令行指定(临时)
>npm --registry https://registry.npm.taobao.org install express

3.编辑 ~/.npmrc 加入下面内容,  这个是写死的,下次打开配置还在
>registry = https://registry.npm.taobao.org

4.改回官方地址可以使用
>npm config set registry https://registry.npmjs.org/
>`

备注: 详情参考这里---->[国内优秀npm镜像推荐及使用](http://riny.net/2014/cnpm/)
第一次使用markdown写这个,还是比较生疏!
