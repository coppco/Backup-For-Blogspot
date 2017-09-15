---
layout: post
title: 软件逆向工程之Delphi
comments: true
toc: true
date: 2017-08-09 12:16:08
tags:
	- 逆向工程
	- Delphi
---

最近以前的同事问我能不能破解一个桌面软件(当然是一个比较简单的小程序), 当时夸下开口说可以, 于是乎私下研究了下逆向工程.

<!--more-->

## <font color=orange>什么是逆向工程?</font>

&emsp;&emsp;逆向工程（又称逆向技术），是一种产品设计技术再现过程，即对一项目标产品进行逆向分析及研究，从而演绎并得出该产品的处理流程、组织结构、功能特性及技术规格等设计要素，以制作出功能相近，但又不完全一样的产品。逆向工程源于商业及军事领域中的硬件分析。其主要目的是在不能轻易获得必要的生产信息的情况下，直接从成品分析，推导出产品的设计原理.
&emsp;&emsp;逆向工程在工业设计中很早就有应用. 举个栗子, 关注军事的同学都知道, 我国第一艘航空母舰------辽宁号航空母舰, 它的前身是上个世纪80年代中后期，于乌克兰建造时遭逢苏联解体建造工程中断，完成度68%的瓦良格号. 从购买到真正抵达大连期间总共历时3年多的时间花费了大量人力、物力和财力. 再比如中国的山寨之王------企鹅, 大家印象最深的应该就是市面上刚刚有个产品火了, 不到两个月腾讯肯定有相关的产品问世, 从QQ、拍拍、财付通、QQ校友、Q飞车、QQ炫舞、QQ电脑管家、CF、微信等等, 不过难得微信的小程序被支付宝山寨了一次.

## <font color=orange>合法性</font>

&emsp;&emsp;在2007年初，我国相关的法律为逆向工程正名，承认了逆向技术用于学习研究的合法性。根据有关法律，对于任何计算机方面的逆向工程，只要不用于商业用途都不违法。比如对商业软件的反编译，代码分析等

&emsp;&emsp;我国虽然在计算机软件保护方面已制定了《计算机软件保护条例》、《计算机软件著作权登记条例》等法律法规，但都未涉及软件反向工程问题，这一点应尽早引起立法机关的重视，正是由于我国法律对有关软件反向工程的问题没有规定，因此诸如微软之类的公司在其软件产品的最终用户使用协议中都规定：“禁止对该软件产品进行反向工程，如果当地法律允许反向工程则除外。”

## <font color=orange>逆向工程需要什么?</font>

* 一些基础的语言如: C、C++(很多游戏、嵌入式使用)
* 汇编语言
* 一些逆向工程常见的软件(这很重要): 如静态反汇编IDA pro和动态调试器OllyDbg还有内核调试winDbg
* 掌握外壳原理和技巧，熟悉常见的加解密算法、反调试技巧

## <font color=orange>Delphi的反编译</font>
### 明确目标软件的编程语言和加壳情况
&emsp;&emsp;PEiD是一款著名的查壳工具，其功能强大，现在有软件很多都加了壳，给破解汉化带来非常大的不便，PEiD几乎可以侦测出所有的壳，其数量已超过470 种PE文档 的加壳类型和签名，另外还可识别出EXE文件是用什么语言编写的，比如：VC++、Delphi、VB或Delphi等.
其他查壳工具还有: `Fi`、`GetTyp`和`pe-scan`等.

<center>
<img src="http://oak4eha4y.bkt.clouddn.com/peid.png" alt="使用PEiD查壳" style="width: 70%; text-align: center; display: block;"/>
</center>	

运气比较好, 如上图所示这个exe文件是没有壳的,而且是使用Delhpi编写的, 如果红圈里面是`Not Found`就是加过壳的, 如果后面有[Overlay]可能是加了伪装成对应语言的壳.

#### 去壳
不同的壳有不同的去壳方式, 高手都是自己脱壳, 对于不会的人可以先尝试脱壳工具, 不一定能成功.

[常见的脱壳工具](https://down.52pojie.cn/Tools/Unpackers/)

##### 常见的壳
* 压缩壳
	* ASPack
	* UPX
	* PeCompact
	* NsPack(国产北斗壳)
* 加密壳
	* ASProtect加密壳
	* Armadillo加密壳
	* EXECryptor加密壳
	* Themida加密壳
	* VM Protect 
	* Code Virtualizer
	* EncryptPE
	* PE-Armor 

### 反编译和反汇编
<font color=green>反汇编得到的是汇编代码</font><br/>
<font color=green>反编译得到的是所用语言的源代码</font>

### 反编译软件的选择

#### 微软开发平台
对于微软开发平台开发出来的软件，我们通常使用.NET Reflector.

#### Borland Delphi
常用的软件有DeDe

#### Java
常用的有Java Decompiler.

### 开始破解
我使用的是由[52破解](http://www.52pojie.cn)提供的汉化版的[ollydbg](https://pan.baidu.com/s/1skMYbQd), 提取密码: acrq

#### 将可执行文件拖入软件或者从文件菜单打开
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/ollydbg%E4%B8%BB%E7%9B%AE%E5%BD%95.png" alt="ollydbg主目录" style="width: 70%; text-align: center; display: block;"/>
</center>	

#### 通过`查找------所有参考文本字符串`快速查找
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/ollydbg_search.png" alt="快速查找" style="width: 70%; text-align: center; display: block;"/>
</center>	

#### 在汇编窗口查看, 双击该字符串行或者选择`反汇编窗口中跟随`

<center>
<img src="http://oak4eha4y.bkt.clouddn.com/ollydbg_search.png" alt="快速查找" style="width: 70%; text-align: center; display: block;"/>
</center>	

<center>
<img src="http://oak4eha4y.bkt.clouddn.com/ollydbg_search2.png" alt="反汇编窗口查看" style="width: 70%; text-align: center; display: block;"/>
</center>	

#### 修改汇编语言以及相关文字
* 修改汇编语言, 双击汇编区域的一行
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/ollydbg_edtingCode.png" alt="修改汇编语言" style="width: 70%; text-align: center; display: block;"/>
</center>
* 修改文字, `右键`------`数据窗口中跟随`------`选择`
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/ollydbg_edtingWord.png" alt="修改文字1" style="width: 70%; text-align: center; display: block;"/>
</center>
双击`HEX数据`中的数据位置, 即可更改文字, 需要注意的是: <font color=red>保持大小</fong>需要勾选.
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/ollydbg_edting_word2.png" alt="修改文字2" style="width: 70%; text-align: center; display: block;"/>
</center>

#### 保存修改保存到可执行文件
`右键`------`复制到壳执行文件`------`全部复制`------`新窗口右键`------`保存`
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/ollydbg_saving1.png" alt="保存" style="width: 70%; text-align: center; display: block;"/>
</center>
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/ollydbg_saving2.png" alt="全部复制" style="width: 70%; text-align: center; display: block;"/>
</center>
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/ollydbg_saving3.png" alt="保存到文字即可" style="width: 70%; text-align: center; display: block;"/>
</center>
