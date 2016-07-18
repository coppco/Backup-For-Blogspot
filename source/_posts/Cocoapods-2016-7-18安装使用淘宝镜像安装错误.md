---
layout: post
title: Cocoapods 2016-7-18安装使用淘宝镜像安装错误
comments: true
date: 2016-07-18 09:04:24
tags:
    - 杂谈
    - 坑点
---

14年年底装的黑苹果最近太卡了,然后系统版本又不支持最新的Xcode,所以决定新装一个mac OS 10.10系统, 黑苹果安装最麻烦的就是驱动了,具体安装步骤这里就不赘述了.
安装完系统,怎么能少了Cocoapods这个神器呢, 安装网上的步骤一步一步Ruby使用淘宝镜像,但是到了最后安装cocoapods的时候`sudo gem install cocoapods`
报错了,错误如下:证书验证失败

>Error fetching https://ruby.taobao.org:
>SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed (https://rubygems-china.oss-cn-hangzhou.aliyuncs.com/specs.4.8.gz)

最后网上搜索了一下:解决方法就是使用Ruby默认源,最后安装成功,至于为什么使用淘宝镜像会失败,原因未知,这里只是解决办法:不使用淘宝终端,打开终端依次输入下面命令:
* 移除现有Ruby淘宝镜像

>gem sources --remove https://ruby.taobao.org/

* 使用Ruby默认源

>gem sources -a https://rubygems.org/

* 验证新源是否替换成功

>gem sources -l

如果终端显示
>*** CURRENT SOURCES ***

>https://rubygems.org/

即为成功了, 最后再次执行`sudo gem install cocoapods`即可安装成功


