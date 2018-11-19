---
layout: post
title: 国内镜像站mirror整理
comments: true
toc: true
date: 2016-08-08 14:28:32
tags:
	- 资料整理
	- mirror
	- 镜像站
	- Java
	- iOS
	- ruby
	- npm
---

<img src="http://47.96.147.179/images/others/mirror.jpg" alt="hello" style="width: 70%; text-align: center; display: block; margin-top:30px; margin-bottom:30px"/>

最近学习JAVA不得不从网外下载一些东西, 但是由于GFW的缘故导致下贼慢, 所有整理了一下目前国内的一些开源Mirror镜像站.

<!--more-->
# 企业镜像站
## 搜狐
http://mirrors.sohu.com/
## 网易
http://mirrors.163.com/
## 阿里云
http://mirrors.aliyun.com/
## 首都在线
http://mirrors.yun-idc.com/
## 中国电信天翼云
http://mirrors.ctyun.cn/

# 教育开源镜像站
比较大的教育镜像站点是清华大学、中国科学技术大学的镜像站, 使用时根据所在地区以及资源目录选择合适的站点.
## 东北地区
### 东北大学开源镜像站
* IPv4/IPv6: http://mirror.neu.edu.cn/
* IPv6: http://mirror.neu6.edu.cn/

### 大连理工大学开源镜像站
* IPv4/IPv6: http://mirror.dlut.edu.cn/

### 大连东软信息学院开源镜像站
* IPv4/IPv6: http://mirrors.neusoft.edu.cn/

### 哈尔滨工业大学开源镜像站(已失效)
* IPv4/IPv6: http://run.hit.edu.cn/
* IPv6: http://run.hit6.edu.cn/

## 华北地区
### 清华大学开源镜像站
* IPv4/IPv6: http://mirrors.tuna.tsinghua.edu.cn/ 
* IPv4: http://mirrors.4.tuna.tsinghua.edu.cn/
* IPv6: http://mirrors.6.tuna.tsinghua.edu.cn/

### 北京理工大学开源镜像站
* IPv4: http://mirror.bit.edu.cn/
* IPv6: http://mirror.bit6.edu.cn/

### 北京交通大学开源镜像站
* IPv4/IPv6: http://mirror.bjtu.edu.cn/

### 天津大学开源镜像站
* IPv4: http://mirror.tju.edu.cn/
* IPv6: http://mirror.tju6.edu.cn/

## 华东北地区
### 中国科学技术大学开源镜像站
* IPv4/IPv6: http://mirrors.ustc.edu.cn/
* IPv4: http://mirrors4.ustc.edu.cn/
* IPv6: http://mirrors6.ustc.edu.cn/

## 华中地区
### 华中科技大学开源镜像站
* IPv4: http://mirror.hust.edu.cn/

### 中国地质大学开源镜像站
* IPv4/IPv6: http://mirrors.cug.edu.cn/
* IPv6: http://mirrors.cug6.edu.cn/

## 华东南地区

### 上海交通大学开源镜像站
* IPv4: http://ftp.sjtu.edu.cn/
* IPv6: http://ftp6.sjtu.edu.cn/

### 浙江大学开源镜像站
* IPv4/IPv6: http://mirrors.zju.edu.cn/

### 厦门大学开源镜像站
* IPv4/IPv6: http://mirrors.xmu.edu.cn/
* IPv6: http://mirrors.xmu6.edu.cn/

## 华南地区
### 中山大学开源镜像站(已失效)
* IPv4: http://mirror.sysu.edu.cn/

## 西北地区
### 兰州大学开源镜像站
* IPv4/IPv6: http://mirror.lzu.edu.cn/

## 西南地区
### 重庆大学开源镜像站
* IPv4: http://mirrors.cqu.edu.cn/

# ruby镜像
## 淘宝ruby镜像
https://ruby.taobao.org/
### 怎么使用
```
gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/
$ gem sources -l
```

# npm镜像
## 淘宝npm镜像
https://npm.taobao.org/
### 怎么使用
* 方式一: 使用定制的`cnpm`命令行工具代替`npm`
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
* 方式二: 你直接通过添加`npm`参数`alias`一个新命令
```
alias cnpm="npm --registry=https://registry.npm.taobao.org \
--cache=$HOME/.npm/.cache/cnpm \
--disturl=https://npm.taobao.org/dist \
--userconfig=$HOME/.cnpmrc"

# Or alias it in .bashrc or .zshrc
$ echo '\n#alias for cnpm\nalias cnpm="npm --registry=https://registry.npm.taobao.org \
  --cache=$HOME/.npm/.cache/cnpm \
  --disturl=https://npm.taobao.org/dist \
  --userconfig=$HOME/.cnpmrc"' >> ~/.zshrc && source ~/.zshrc
```