---
layout: post
title: Shell脚本学习
comments: true
toc: true
date: 2017-09-15 14:37:55
tags:
	- Shell
	- Linux
---

## <font color=orange>前言</font>

最近希望可以一键自动打包iOS App Store上传的ipa包, 所有来学习一下Shell脚本.

<img src="http://47.96.147.179/images/others/shell_top.jpg" alt="hello" style="width: 50%; text-align: center; display: block; margin-top:30px; margin-bottom:30px"/>

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

#### <font color=green>运行 Shell 脚本有两种方法：</font>

* 作为可执行程序
进入test.sh所在的目录, 运行下面的代码

```shell
chmod +x ./test.sh  #使脚本具有执行权限
./test.sh  #执行脚本
```
* 作为解释器参数
这种运行方式是，直接运行解释器，其参数就是 shell 脚本的文件名，这种方式运行的脚本，不需要在第一行指定解释器信息，写了也没用。如: 

```shell
/bin/sh test.sh
```

### <font color=orange> Shell 变量</font>

#### <font color=green>定义变量</font>

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

#### <font color=green>使用变量</font>

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

#### <font color=green>只读变量</font>

使用 `readonly` 命令可以将变量定义为只读变量，只读变量的值不能被改变。

```shell
readonly homepage="coppco.github.io"

version="1.0.0"
readonly version
version="1.0.1"
```

再次修改命令行会提示: <font color=red>`version: readonly variable`</font>

#### <font color=green>删除变量</font>

使用 unset 命令可以删除变量。语法：

```shell
unset 变量名
```

* <font color=red>注意事项</font>
	* 变量被删除后不能再次使用
	* unset 命令不能删除只读变量。

#### <font color=green>变量类型</font>

1. 局部变量: 局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。
2. 环境变量: 所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。
3. shell变量: shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行

### <font color=orange>Shell 字符串</font>

#### <font color=green>字符串定义</font>

字符串是shell编程中最常用最有用的数据类型（除了数字和字符串，也没啥其它类型好用了），字符串可以用单引号，也可以用双引号，也可以不用引号。
* <font color=red>注意事项</font>
	* 单引号字符串的限制：
		* 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
		* 单引号字串中不能出现单引号（对单引号使用转义符后也不行）。
	* 单双引号的优点：
		* 双引号里可以有变量
		* 双引号里可以出现转义字符
	* 反引号\`\`
		* 反引号是命令替换, Shell可以先执行\`\`中的命令, 将结果保存起来, 在适当的地方输出.

#### <font color=green>拼接字符串</font>

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

#### <font color=green>获取字符串长度</font>

* 格式: `${ #变量名 }`, <font color=red>这里有一个bug, 如果#和{或者}连在会出错.</font>

```shell
string="abcd"
echo ${#string} #输出 4
```

#### <font color=green>提取子字符串</font>

* `# 号截取`: 删除左边字符，保留右边字符。
```shell
echo ${var#*子字符串}
```
说明: 其中 var 是变量名，# 号是运算符，`*子字符串` 表示从左边开始删除第一个 `子字符串` 及左边的所有字符

* `## 号截取`: 删除左边字符，保留右边字符。
```shell
echo ${var##*子字符串}
```
说明: 表示从左边开始删除最后（最右边）一个 `子字符串` 号及左边的所有字符

* `%号截取`: 删除右边字符，保留左边字符
```shell
echo ${var%子字符串*}
```
说明: 表示从右边开始，删除第一个 `子字符串` 号及右边的字符
* `%% 号截取`: 删除右边字符，保留左边字符
```shell
echo ${var%%子字符串*}
```
说明: 表示从右边开始，删除最后（最左边）一个 `子字符串` 号及右边的字符
* 从左边第几个字符开始，及字符的个数 
```shell
echo ${var:0:5}
```
说明: 其中的 0 表示左边第一个字符开始，5 表示字符的总个数。
* 从左边第几个字符开始，一直到结束。
```shell
echo ${var:7}
```
说明: 其中的 7 表示左边第8个字符开始，一直到结束。
* 从右边第几个字符开始，及字符的个数
```shell
echo ${var:0-7:3}
```
说明: 其中的 0-7 表示右边算起第七个字符开始，3 表示字符的个数。
* 从右边第几个字符开始，一直到结束。
```shell
echo ${var:0-7}
```
说明: 表示从右边第七个字符开始，一直到结束。

#### <font color=green>查找子字符串</font>

下面脚本中 "`" 是反引号，而不是单引号 "'"，不要看错了哦。

```shell
string="runoob is a great company"
echo `expr index "$string" is`  # 输出 8
```

### <font color=orange>Shell 数组</font>

bash支持一维数组（不支持多维数组），并且没有限定数组的大小。数组元素的下标由0开始编号。获取数组中的元素要利用下标，下标可以是整数或算术表达式，其值应大于或等于0。

#### <font color=green>定义数组</font>

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

#### <font color=green>读取数组</font>

读取数组中一个元素值的一般格式是：`${数组名[下标]}`

```
echo ${array[0]}
```

使用@符号可以获取数组中的所有元素: `${数组名[@]}`
```
echo ${array[@]}
```

#### <font color=green>获取数组的长度</font>

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


### <font color=orange>Shell 传递参数</font>

我们可以在执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为：$n。n 代表一个数字，1 为执行脚本的第一个参数，2 为执行脚本的第二个参数，以此类推……

```
#!/bin/bash

echo "Shell 传递参数实例！";
echo "执行的文件名：$0";
echo "第一个参数为：$1";
echo "第二个参数为：$2";
echo "第三个参数为：$3";
```
执行该sh文件传递三个参数: 
```
$ chmod +x test.sh 
$ ./test.sh 1 2 3
Shell 传递参数实例！
执行的文件名：./test.sh
第一个参数为：1
第二个参数为：2
第三个参数为：3
```

|参数处理|说明|
|:--:|:--:|
|$n|n表示数字, 从1开始表示第一个参数|
|$#	|传递到脚本的参数个数|
|$*	|以一个单字符串显示所有向脚本传递的参数。如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。|
|$$|	脚本运行的当前进程ID号|
|$!|	后台运行的最后一个进程的ID号|
|$@|	与$*相同，但是使用时加引号，并在引号中返回每个参数。如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。|
|$-|	显示Shell使用的当前选项，与set命令功能相同。|
|$?|	显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。|


### <font color=orange>Shell 基本运算符</font>

Shell 和其他编程语言一样，支持多种运算符，包括：

#### <font color=green>算术运算符</font>
* 注意事项
	*  条件表达式要放在方括号之间，并且要有空格，例如: [$a==$b] 是错误的，必须写成 [ $a == $b ]。
	* 乘号(`*`)前边必须加反斜杠(`\`)才能实现乘法运算；
	* 在 MAC 中 shell 的 expr 语法是：$((表达式))，此处表达式中的 "\*" 不需要转义符号 "\" , Mac中如 `$(( $a != $b ))`、`$(($a*$b))`
下表列出了常用的算术运算符，假定变量 a 为 10，变量 b 为 20：

|运算符|说明|举例|
|:--:|:--:|:--:|
|+|	加法|	\`expr $a + $b\` 结果为 30。|
|-|	减法|	\`expr $a - $b\` 结果为 -10。|
|*|	乘法|	\`expr $a \\* $b\` 结果为  200。|
|/|	除法|	\`expr $b / $a\` 结果为 2。|
|%|	取余|	\`expr $b % $a\` 结果为 0。|
|=|	赋值|	a=$b 将把变量 b 的值赋给 a。|
|==	|相等。用于比较两个数字，相同则返回 true。|	[ $a == $b ] 返回 false。|
|!=|	不相等。用于比较两个数字，不相同则返回 true。|	[ $a != $b ] 返回 true。|

原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 `awk` 和 `expr`，expr 最常用。expr 是一款表达式计算工具，使用它能完成表达式的求值操作。

例如，两个数相加(注意使用的是反引号 ` 而不是单引号 ')：
```shell
#!/bin/bash

val=`expr 2 + 2`
echo "两数之和为 : $val"
```

#### <font color=green>关系运算符</font>

关系运算符只支持数字，不支持字符串，除非字符串的值是数字。
下表列出了常用的关系运算符，假定变量 a 为 10，变量 b 为 20：

|运算符	|说明|	举例|
|:--:|:--:|:--:|
|-eq|	检测两个数是否相等，相等返回 true。|	[ $a -eq $b ] 返回 false。|
|-ne|	检测两个数是否相等，不相等返回 true。	|[ $a -ne $b ] 返回 true。|
|-gt|	检测左边的数是否大于右边的，如果是，则返回 true。|	[ $a -gt $b ] 返回 false。|
|-lt|	检测左边的数是否小于右边的，如果是，则返回 true。|	[ $a -lt $b ] 返回 true。|
|-ge|	检测左边的数是否大于等于右边的，如果是，则返回 true。|	[ $a -ge $b ] 返回 false。|
|-le|	检测左边的数是否小于等于右边的，如果是，则返回 true。|	[ $a -le $b ] 返回 true。|

```shell
a=100
b=200
#对于数字下面两个都可以
#if [ $a -eq $b ]
if [ $a == $b ]
then
echo "$a -eq $b : a 等于 b"
else
echo "$a -eq $b: a 不等于 b"
fi
```


#### <font color=green>布尔运算符</font>
下表列出了常用的布尔运算符，假定变量 a 为 10，变量 b 为 20：

|运算符|	说明|	举例|
|:--:|:--:|:--:|
|!|	非运算，表达式为 true 则返回 false，否则返回 true。|	[ ! false ] 返回 true。|
|-o|	或运算，有一个表达式为 true 则返回 true。|	[ $a -lt 20 -o $b -gt 100 ] 返回 true。|
|-a	|与运算，两个表达式都为 true 才返回 true。|[ $a -lt 20 -a $b -gt 100 ] 返回 false。|

#### <font color=green>逻辑运算符</font>
以下介绍 Shell 的逻辑运算符，假定变量 a 为 10，变量 b 为 20:
(ll在表格中显示有问题, 实际上是`enter`上面的符号). <font color=red>它和布尔运算符有区别,  它需要外面有两个[]括住.</font>

|运算符|	说明|	举例|
|:--:|:--:|:--:|
|&&|	逻辑的 AND|	[[ $a -lt 100 && $b -gt 100 ]] 返回 false|
|ll |逻辑的 OR|	[[ $a -lt 100 ll $b -gt 100 ]] 返回 true|

#### <font color=green>字符串运算符</font>
下表列出了常用的字符串运算符，假定变量 a 为 "abc"，变量 b 为 "efg"：

|运算符|	说明|	举例|
|:--:|:--:|:--:|
|=	|检测两个字符串是否相等，相等返回 true。	|[ $a = $b ] 返回 false。|
|!=|	检测两个字符串是否相等，不相等返回 true。|	[ $a != $b ] 返回 true。|
|-z|	检测字符串长度是否为0，为0返回 true。	|[ -z $a ] 返回 false。|
|-n	|检测字符串长度是否为0，不为0返回 true。|	[ -n $a ] 返回 true。|
|$字符串|	检测字符串是否为空，不为空返回 true。	|[ $a ] 返回 true。|

#### <font color=green>文件测试运算符</font>

文件测试运算符用于检测 Unix 文件的各种属性。

|操作符|	说明|	举例|
|:--:|:--:|:--:|
|-b file|	检测文件是否是块设备文件，如果是，则返回 true。|	[ -b $file ] 返回 false。|
|-c file|	检测文件是否是字符设备文件，如果是，则返回 true。|	[ -c $file ] 返回 false。|
|-d file|	检测文件是否是目录，如果是，则返回 true。|	[ -d $file ] 返回 false。|
|-f file|	检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。|[ -f $file ] 返回 true。|
|-g file	|检测文件是否设置了 SGID 位，如果是，则返回 true。|	[ -g $file ] 返回 false。|
|-k file|	检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。	[ -k $file ] 返回 false。
|-p file|	检测文件是否是有名管道，如果是，则返回 true。|	[ -p $file ] 返回 false。|
|-u file|	检测文件是否设置了 SUID 位，如果是，则返回 true。|	[ -u $file ] 返回 false。|
|-r file|	检测文件是否可读，如果是，则返回 true。|	[ -r $file ] 返回 true。|
|-w file|	检测文件是否可写，如果是，则返回 true。|	[ -w $file ] 返回 true。|
|-x file|	检测文件是否可执行，如果是，则返回 true。|	[ -x $file ] 返回 true。|
|-s file|	检测文件是否为空（文件大小是否大于0），不为空返回 true。|	[ -s $file ] 返回 true。|
|-e file|	检测文件（包括目录）是否存在，如果是，则返回 true。|	[ -e $file ] 返回 true。|

### <font color=orange>Shell echo命令</font>
用于字符串的输出。命令格式：`echo string`, 支持字符串常量、变量、转义字符串等输出.

#### <font color=green>显示普通字符串</font>

```shell
echo "It is a test"
```
说明: 这里的双引号完全可以省略，以下命令与上面实例效果一致

#### <font color=green>显示转义字符</font>
```shell
echo "\"It is a test\""
```
说明: 这里的双引号也可以省略

#### <font color=green>显示变量</font>
```shell
name "Are you OK ?" 
echo $name
```

#### <font color=green>显示换行</font>
```shell
echo -e "OK! \n" # -e 开启转义
echo "It it a test"
```
#### <font color=green>显示不换行</font>
```shell
echo -e "OK! \c" # -e 开启转义 \c 不换行
echo "It is a test"
```

#### <font color=green>显示结果定向至文件</font>
```shell
echo "It is a test" > myfile
```

#### <font color=green>原样输出字符串，不进行转义或取变量(用单引号)</font>
```shell
echo '$name\"'
```

#### <font color=green>显示命令执行结果</font>
```shell
echo `date`
```

### <font color=orange>Shell printf 命令</font>

printf 命令模仿 C 程序库（library）里的 printf() 程序。
标准所定义，因此使用printf的脚本比使用echo移植性好。
printf 使用引用文本或空格分隔的参数，外面可以在printf中使用格式化字符串，还可以制定字符串的宽度、左右对齐方式等。默认printf不会像 echo 自动添加换行符，我们可以手动添加 \n。
它的格式: `printf  format-string  [arguments...]`

```shell
printf "%-10s %-8s %-4.2f\n" 郭靖 男 66.1234 
```
%d: Decimal 十进制整数 -- 对应位置参数必须是十进制整数
%s: String 字符串 -- 对应位置参数必须是字符串或者字符型
%c: Char 字符 -- 对应位置参数必须是字符串或者字符型
%f: Float 浮点 -- 对应位置参数必须是数字型
%-10s 指一个宽度为10个字符（-表示左对齐，没有则表示右对齐），任何字符都会被显示在10个字符宽的字符内，如果不足则自动以空格填充，超过也会将内容全部显示出来。
%-4.2f 指格式化为小数，其中.2指保留2位小数

#### <font color=green>printf的转义序列</font>

|序列|	说明|
|:--:|:--:|
|\a|	警告字符，通常为ASCII的BEL字符|
|\b|	后退|
|\c|	抑制（不显示）输出结果中任何结尾的换行字符（只在%b格式指示符控制下的参数字符串中有效），而且，任何留在参数里的字符、任何接下来的参数以及任何留在格式字符串中的字符，都被忽略|
|\f	|换页（formfeed）|
|\n	|换行|
|\r	|回车（Carriage return）|
|\t	|水平制表符|
|\v	|垂直制表符|
|\\|	一个字面上的反斜杠字符|
|\ddd	|表示1到3位数八进制值的字符。仅在格式字符串中有效|
|\0ddd|	表示1到3位的八进制值字符|

### <font color=orange>Shell test 命令</font>

Shell中的 test 命令用于检查某个条件是否成立，它可以进行数值、字符和文件三个方面的测试。

#### <font color=green>数值测试</font>

|参数|	说明|
|:--:|:--:|
|-eq|	等于则为真|
|-ne|	不等于则为真|
|-gt|	大于则为真|
|-ge|	大于等于则为真|
|-lt|	小于则为真|
|-le|	小于等于则为真|

```shell
num1=100
num2=100
if test $[num1] -eq $[num2]
then
    echo '两个数相等！'
else
    echo '两个数不相等！'
fi
#结果为: 两个数不相等!
```

代码中的 [] 执行基本的算数运算，如：

```shell
#!/bin/bash
a=5
b=6
result=$[a+b] # 注意等号两边不能有空格
echo "result 为： $result"
```

#### <font color=green>字符串测试</font>

|参数|	说明|
|:--:|:--:|
|=	|等于则为真|
|!=|	不相等则为真|
|-z 字符串|	字符串的长度为零则为真|
|-n 字符串|	字符串的长度不为零则为真|

#### <font color=green>文件测试</font>

|参数|	说明|
|:--:|:--:|
|-e 文件名|	如果文件存在则为真|
|-r 文件名|	如果文件存在且可读则为真|
|-w 文件名|	如果文件存在且可写则为真|
|-x 文件名|	如果文件存在且可执行则为真|
|-s 文件名|	如果文件存在且至少有一个字符则为真|
|-d 文件名|	如果文件存在且为目录则为真|
|-f 文件名|	如果文件存在且为普通文件则为真|
|-c 文件名|	如果文件存在且为字符型特殊文件则为真|
|-b 文件名|	如果文件存在且为块特殊文件则为真|

### <font color=orange>Shell 流程控制</font>

#### <font color=green>if</font>

在sh/bash里可不能这么写，如果else分支没有语句执行，就不要写这个else。

if 语句语法格式：
```shell
if condition
then
    command1 
    command2
    ...
    commandN 
fi
```
写成一行（适用于终端命令提示符）：
```
if [ $(ps -ef | grep -c "ssh") -gt 1 ]; then echo "true"; fi
```

#### <font color=green>if-else</font>

if-else 语句语法格式：
```shell
if condition
then
    command1 
    command2
    ...
    commandN
else
    command
fi
```

#### <font color=green>if else-if else</font>

if else-if else 语法格式：
```shell
if condition1
then
    command1
elif condition2 
then 
    command2
else
    commandN
fi
```

#### <font color=green>for 循环</font>

for循环一般格式为：
```
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```
写成一行：
```
for var in item1 item2 ... itemN; do command1; command2… done;
```

#### <font color=green> while 循环</font>

while循环用于不断执行一系列命令，也用于从输入文件中读取数据；命令通常为测试条件。其格式为：
```
while condition
do
    command
done
```
当条件是true或者`:`时, 是无限循环

#### <font color=green>until 循环</font>
until循环执行一系列命令直至条件为真时停止。
until循环与while循环在处理方式上刚好相反。
一般while循环优于until循环，但在某些时候—也只是极少数情况下，until循环更加有用。
until 语法格式:
```
until condition
do
    command
done
```
条件可为任意测试条件，测试发生在循环末尾，因此循环至少执行一次—请注意这一点。

#### <font color=green>case</font>

Shell case语句为多选择语句。可以用case语句匹配一个值与一个模式，如果匹配成功，执行相匹配的命令。case语句格式如下：
```
case 值 in
模式1)
    command1
    command2
    ...
    commandN
    ;;
模式2）
    command1
    command2
    ...
    commandN
    ;;
esac
```

case工作方式如上所示。取值后面必须为单词in，每一模式必须以右括号结束。取值可以为变量或常数。匹配发现取值符合某一模式后，其间所有命令开始执行直至 ;;。
取值将检测匹配的每一个模式。一旦模式匹配，则执行完匹配模式相应命令后不再继续其他模式。如果无一匹配模式，使用星号 * 捕获该值，再执行后面的命令。

### <font color=orange>跳出循环</font>
在循环过程中，有时候需要在未达到循环结束条件时强制跳出循环，Shell使用两个命令来实现该功能：break和continue。

#### <font color=green>break命令</font>
break命令允许跳出所有循环（终止执行后面的所有循环）。

```
while :
do
    echo -n "输入 1 到 5 之间的数字:"
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的! 游戏结束"
            break
        ;;
    esac
done
```

#### <font color=green> continue命令</font>

continue命令与break命令类似，只有一点差别，它不会跳出所有循环，仅仅跳出当前循环。
```shell
while :
do
    echo -n "输入 1 到 5 之间的数字: "
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的!"
            continue
            echo "游戏结束"
        ;;
    esac
done
```

#### <font color=green> esac命令</font>

case的语法和C family语言差别很大，它需要一个esac（就是case反过来）作为结束标记，每个case分支用右圆括号，用两个分号表示break。

### <font color=orange>Shell 函数</font>

#### <font color=green> 函数定义</font>

linux shell 可以用户定义函数，然后在shell脚本中可以随便调用。
shell中函数的定义格式如下：[]中内容可以省略
```
[ function ] funname [()]
{
    action;
    [return int;]
}
```
* <font color=red>注意事项</font>
	* 函数返回值在调用该函数后通过 $? 来获得。
	* 所有函数在使用前必须定义。这意味着必须将函数放在脚本开始部分，直至shell解释器首次发现它时，才可以使用。调用函数仅使用其函数名即可。
	* 可以带function fun() 定义，也可以直接fun() 定义,不带任何参数。
	* 参数返回，可以显示加：return 返回，如果不加，将以最后一条命令运行结果，作为返回值。 return后跟数值n(0-255)


```
funWithReturn(){
    echo "这个函数会对输入的两个数字进行相加运算..."
    echo "输入第一个数字: "
    read aNum
    echo "输入第二个数字: "
    read anotherNum
    echo "两个数字分别为 $aNum 和 $anotherNum !"
    return $(($aNum+$anotherNum))
}
funWithReturn
echo "输入的两个数字之和为 $? !"
```

#### <font color=green> 函数参数</font>

在Shell中，调用函数时可以向其传递参数。在函数体内部，通过 $n 的形式来获取参数的值，例如，$1表示第一个参数，$2表示第二个参数...
基本和`Shell 传递参数`中一样.


### <font color=orange>Shell 输入/输出重定向</font>
大多数 UNIX 系统命令从你的终端接受输入并将所产生的输出发送回​​到您的终端。一个命令通常从一个叫标准输入的地方读取输入，默认情况下，这恰好是你的终端。同样，一个命令通常将其输出写入到标准输出，默认情况下，这也是你的终端。
重定向命令列表如下：

|命令|	说明|
|:--:|:--:|
|command > file|	将输出重定向到 file。|
|command < file|	将输入重定向到 file。|
|command >> file|	将输出以追加的方式重定向到 file。|
|n > file|	将文件描述符为 n 的文件重定向到 file。|
|n >> file|	将文件描述符为 n 的文件以追加的方式重定向到 file。|
|n >& m|	将输出文件 m 和 n 合并。|
|n <& m|	将输入文件 m 和 n 合并。|
|<< tag|	将开始标记 tag 和结束标记 tag 之间的内容作为输入。|

### <font color=orange>Shell 文件包含</font>

和其他语言一样，Shell 也可以包含外部脚本。这样可以很方便的封装一些公用的代码作为一个独立的文件。被包含的文件不需要可执行权限。
Shell 文件包含的语法格式如下：
```
. filename   # 注意点号(.)和文件名中间有一空格
或
source filename
```