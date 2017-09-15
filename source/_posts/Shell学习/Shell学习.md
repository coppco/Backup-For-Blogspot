---
layout: post
title: Shell脚本学习
comments: true
toc: true
date: 2017-09-15 14:37:55
tags:
	- Shell
---

## <font color=orange>前言</font>

最近希望可以一键自动打包iOS App Store上传的ipa包, 所有来学习一下Shell脚本.

<img src="http://oak4eha4y.bkt.clouddn.com/shell_top.jpg" alt="hello" style="width: 50%; text-align: center; display: block; margin-top:30px; margin-bottom:30px"/>

<!--more-->

## <font color=orange> Shell </font>

Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计语言。
Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。
Ken Thompson 的 sh 是第一种 Unix Shell，Windows Explorer 是一个典型的图形界面 Shell。

### <font color=orange> Shell 脚本</font>

Shell 脚本（shell script），是一种为 shell 编写的脚本程序。
业界所说的 shell 通常都是指 `shell 脚本`，但是要知道，shell 和 shell script 是两个不同的概念。<font color=red>本文出现的 "shell编程" 都是指 shell 脚本编程，不是指开发 shell 自身。</font>. 它有点像Windows系统下面的`.bat`批处理文件.

### <font color=orange> Shell 环境</font>

Shell 编程跟 java、php 编程一样，只要有一个能编写代码的文本编辑器和一个能解释执行的脚本解释器就可以了。

* Linux 的 Shell 种类众多，常见的有:
	* Bourne Shell（/usr/bin/sh或/bin/sh）
	* Bourne Again Shell（/bin/bash）
	* C Shell（/usr/bin/csh）
	* K Shell（/usr/bin/ksh）
	* Shell for Root（/sbin/sh）

由于Bourne Again Shell的易用性和免费，Bash 在日常工作中被广泛使用。同时，Bash 也是大多数Linux 系统默认的 Shell。
在一般情况下，人们并不区分 Bourne Shell 和 Bourne Again Shell，所以，像 `#!/bin/sh`，它同样也可以改为 `#!/bin/bash`。

### <font color=orange>第一个 Shell 脚本</font>

打开文本编辑器(可以使用 vi/vim 命令来创建文件)，新建一个文件 test.sh，扩展名为 sh（sh代表shell），扩展名并不影响脚本执行，见名知意就好.

```shell
#!/bin/bash
echo "Hello World!"
```

* `#!`: 告诉系统其后路径所指定的程序即是解释此脚本文件的 Shell 程序。
* `echo`: 命令用于向窗口输出文本。

#### <font color=orange>运行 Shell 脚本有两种方法：</font>

* 作为可执行程序
进入test.sh所在的目录, 运行下面的代码

```shell
chmod +x ./test.sh  #使脚本具有执行权限
./test.sh  #执行脚本
```
* 作为解释器参数
这种运行方式是，直接运行解释器，其参数就是 shell 脚本的文件名，如：

```shell
/bin/sh test.sh
```
	* 这种方式运行的脚本，不需要在第一行指定解释器信息，写了也没用。

### <font color=orange> Shell 变量</font>

#### <font color=orange>定义变量</font>

* 定义变量时，变量名不加美元符号（$，PHP语言中变量需要），如：

```shell
homepage="coppco.github.io"
```
	* <font color=red>注意事项</font>
		* 变量名和等号之间不能有空格
		* 首个字符必须为字母（a-z，A-Z）。
		* 中间不能有空格，可以使用下划线_。
		* 不能使用标点符号。
		* 不能使用bash里的关键字（可用help命令查看保留关键字）。

* 除了显式地直接赋值，还可以用语句给变量赋值，如：

```shell
for skill in Ada Coffe Action Java; do
echo "I am good at ${skill}Script"
done
```

#### <font color=orange>使用变量</font>

使用一个定义过的变量，只要在变量名前面加美元符号即可，如：

```shell
echo $homepage
echo ${skill}
```

* <font color=red>注意事项</font>
	* 单独使用时可以不添加花括号
	* 在上例中, 必须带花括号用来识别变量的边界, 如`echo "I am good at ${skill}Script"`, 如果不加花括号`echo "I am good at $skillScript"`, 那么会把$skillScript当做变量来处理.
	* 已经定义的变量, 可以被重新定义

```shell
homepage="coppco.github.io"
echo homepage
homepage="https://coppco.github.io"
echo homepage
```

#### <font color=orange>只读变量</font>

使用 `readonly` 命令可以将变量定义为只读变量，只读变量的值不能被改变。

```shell
readonly homepage="coppco.github.io"

version="1.0.0"
readonly version
version="1.0.1"
```

再次修改命令行会提示: <font color=red>`version: readonly variable`</font>

#### <font color=orange>删除变量</font>

使用 unset 命令可以删除变量。语法：

```shell
unset 变量名
```

* <font color=red>注意事项</font>
	* 变量被删除后不能再次使用
	* unset 命令不能删除只读变量。

#### <font color=orange>变量类型</font>

1. 局部变量: 局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。
2. 环境变量: 所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。
3. shell变量: shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行

### <font color=orange>Shell 字符串</font>

#### <font color=orange>字符串定义</font>

字符串是shell编程中最常用最有用的数据类型（除了数字和字符串，也没啥其它类型好用了），字符串可以用单引号，也可以用双引号，也可以不用引号。
* <font color=red>注意事项</font>
	* 单引号字符串的限制：
		* 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
		* 单引号字串中不能出现单引号（对单引号使用转义符后也不行）。
	* 单双引号的优点：
		* 双引号里可以有变量
		* 双引号里可以出现转义字符

#### <font color=orange>拼接字符串</font>

可以在双引号字符串中使用变量, 变量也可以使用双引号括起来:

```shell
https_prefix="https://"
http_prefix="http://"
url="coppco.github.io"
baidu_http="$http_prefix"$url""
baidu_https="$https_prefix${url}"

echo $baidu_http
echo $baidu_https
echo "$http_prefix$url"
echo "$http_prefix"${url}""
```

#### <font color=orange>获取字符串长度</font>

* 格式: `${ #变量名 }`, <font color=red>这里有一个bug, 如果#和{或者}连在会出错.</font>

```shell
string="abcd"
echo ${#string} #输出 4
```

#### <font color=orange>提取子字符串</font>

* 格式: `${变量名:fromIndex:length}`

```
string="runoob is a great site"
echo ${string:1:4} # 输出 unoo
```
#### <font color=orange>查找子字符串</font>

### <font color=orange>Shell 数组</font>

bash支持一维数组（不支持多维数组），并且没有限定数组的大小。数组元素的下标由0开始编号。获取数组中的元素要利用下标，下标可以是整数或算术表达式，其值应大于或等于0。

#### <font color=orange>定义数组</font>

在Shell中，用括号来表示数组，数组元素用"空格"符号分割开。定义数组的一般形式为：

```
数组名=(值1 值2 ... 值n)
```

还可以单独定义数组的各个分量:
```
array[0]=0
array[1]=1
array[2]=2
```

#### <font color=orange>读取数组</font>

读取数组中一个元素值的一般格式是：`${数组名[下标]}`

```
echo ${array[0]}
```

使用@符号可以获取数组中的所有元素: `${数组名[@]}`
```
echo ${array[@]}
```

#### <font color=orange>获取数组的长度</font>

* 获取数组的长度
格式: `${ #数组名[@] }`或者`${ #数组名[*] }`

```
echo ${ #arrays[@] }
```

* 获取单个元素的长度
格式: `${ #数组名[下标] }`

```
echo ${ #arrays[0] }
```

### <font color=orange>Shell 注释</font>

以"#"开头的行就是注释，会被解释器忽略。sh中没有多行注释, 需要每行都添加#才可以。每一行加个#符号太费力了，可以把这一段要注释的代码用一对花括号括起来，定义成一个函数，没有地方调用这个函数，这块代码就不会执行，达到了和注释一样的效果。

