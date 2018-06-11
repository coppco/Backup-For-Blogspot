---
layout: post
title: Cocoapods的安装
comments: true
toc: true
date: 2015-07-18 09:04:24
tags:
    - 杂谈
    - iOS
    - 坑点
    - Cocoapods
---

14年年底装的黑苹果最近太卡了,然后系统版本又不支持最新的Xcode8-beta版本,所以决定新装一个mac OS 10.10系统, 黑苹果安装最麻烦的就是驱动了,具体安装步骤这里就不赘述了.
<!--more-->

## <font color=orange> 如何在Mac OS X上安装 Ruby运行环境 </font>
### <font color=orange> 首先安装Xcode </font>
安装好Xcode会帮你一些开发包
### <font color=orange> 安装 RVM </font>
* 安装RVM
```
curl -L https://get.rvm.io | bash -s stable
```
* 载入RVM环境
```
source ~/.rvm/scripts/rvm
```
* 查看RVM版本
```
rvm -v
```
### <font color=orange> 用 RVM 安装 Ruby 环境 </font>
* 列出已知的ruby版本
```
rvm list known
```
* 选择现有的rvm版本来进行安装
```
rvm install 2.4.1
```
* 查询已经安装的ruby
```
rvm list
```
* 卸载一个已安装版本 
```
rvm remove 1.9.2
```
### <font color=orange> 设置 Ruby 版本 </font>
* 将指定版本的 Ruby 设置为系统默认版本, 前提是需要本地安装了该版本
```
rvm 2.4.1 --default
```
* 查看版本
```
ruby -v
gem -v
```
### <font color=orange> 更改Ruby的默认源 </font>
* 替换源
```
gem source -r https://rubygems.org/
gem source -a https://ruby.taobao.org
```
* 验证是否替换成功
```
gem sources -l 
```
	* 提示如下表示成功
```
CURRENT SOURCES　　　　　　　　　　　　

http://ruby.taobao.org/　
```

## <font color=orange> 如何下载和安装CocoaPods？ </font>

* 首先安装Ruby环境
* 然后安装Cocoapods
```
sudo gem install cocoapods
```

## <font color=orange> 2016年07月18日使用淘宝镜像错误 </font>
安装完系统,怎么能少了Cocoapods这个神器呢, 安装网上的步骤一步一步Ruby使用淘宝镜像,但是到了最后安装cocoapods的时候`sudo gem install cocoapods`
报错了,错误如下:证书验证失败

>Error fetching https://ruby.taobao.org:
>SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed (https://rubygems-china.oss-cn-hangzhou.aliyuncs.com/specs.4.8.gz)

最后网上搜索了一下:解决方法就是使用Ruby默认源,最后安装成功,至于为什么使用淘宝镜像会失败,原因未知,这里只是解决办法:不使用淘宝终端,打开终端依次输入下面命令:
* 移除现有Ruby淘宝镜像

>gem sources --remove https://ruby.taobao.org/

* 使用Ruby默认源

>gem sources -a https://rubygems.org/

* 使用其他源

>gem sources -a https://gems.ruby-china.org

* 验证新源是否替换成功

>gem sources -l

如果终端显示
>*** CURRENT SOURCES ***

>https://rubygems.org/

即为成功了, 最后再次执行`sudo gem install cocoapods`即可安装成功


Failed to connect to GitHub to update the CocoaPods/Specs specs repo

## <font color=orange> 2018年03月08日 pod install报错误</font>

>Failed to connect to GitHub to update the CocoaPods/Specs specs repo

网上说是因为是Github在不久之前的2018年2月23号移除了一些低加密标准协议，包括TLSv1/TLSv1.1，diffie-hellman-group1-sha1，diffie-hellman-group14-sha1, [Weak cryptographic standards removed](https://blog.github.com/2018-02-23-weak-cryptographic-standards-removed/).

* 首先需要升级Ruby
```
rvm install 2.4.1
rvm 2.4.1 --default
```
* 然后更新Cocoapods
```
gem install cocoapods -n /usr/local/bin
```