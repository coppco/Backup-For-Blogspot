---
layout: post
title: 使自己的Github项目支持Cocoapods
comments: true
toc: true
date: 2017-08-12 17:27:17
tags:
	- iOS
	- Swift
	- Objective-C
	- Cocoapods
	- 资料整理
---

很早之前就想做的事情之一, 刚好最近有个新功能需要添加到项目中, 于是就想顺便上传到github, 使用Cocoapods来管理.
<img src="http://oak4eha4y.bkt.clouddn.com/github_cocoapods.jpg" alt="hello" style="width: 50%; text-align: center; display: block; margin-top:30px; margin-bottom:30px"/>
<!--more-->

## 准备(安装Cocoapods和注册trunk)
现在Cocoapods使用trunk方式, 并且需要版本在0.33以上.

* 安装: `sudo gem install cocoapods`
* 查看版本: `pod --version`
* 注册trunk, 执行下面命令, 邮箱会收到验证链接, 打开链接注册成功
```
pod trunk register xxxx@qq.com '用户名'  --verbose
```
* 成功之后查询自己的信息: `pod trunk me`

## 初始化仓库

在Github上面新建一个仓库, 免费的用户只能创建`Public`类型的仓库, 任何人都可以clone该仓库. 创建的同时可以根据开发语言选择忽略文件, 选择开源协议.

## 克隆仓库到本地

* 克隆命令(替换你的用户名和项目名)
```
git clone https://github.com/你的用户名/你的项目名.git
```

## 将你的代码拷贝到该仓库中

将你的项目添加到仓库中

## 配置 .podspec文件
* 在仓库中新建配置文件(推荐使用`你的项目名`)

```
pod spec create 你的项目名
```

* 配置文件中字段说明, [官方文档](http://guides.cocoapods.org/syntax/podspec.html)

```
Pod::Spec.new do |s|

# 名称 使用的时候pod search [name]
s.name = "WhatKeyboard"

# 代码库的版本
s.version = "1.0.0"

# 简介
s.summary = "自定义密码安全键盘"

s.description  = <<-DESC
                        这是一个自定义密码安全键盘.
                        DESC

# 主页
s.homepage = "https://github.com/coppco/WhatKeyboard"

# 许可证书类型，要和仓库的LICENSE 的类型一致
s.license = "MIT"

# 作者名称 和 邮箱
s.author = { "coppco" => "coppco@qq.com" }

# 作者主页
s.social_media_url ="https://coppco.github.io"

# 代码库最低支持的版本
s.platform = :ios, "7.0"

# 代码的Clone 地址 和 tag 版本
s.source = { :git => "https://github.com/coppco/WhatKeyboard.git", :tag => "#{s.version}" }

#新建目录
s.default_subspec = 'Core'

s.subspec 'Core' do |ss|
ss.source_files = 'WhatKeyboard/classes/*.{h,m}'
end

#框架被其他工程引入时, 导入的.h和.m等  **/*表示目录及其子目录下所有文件, 如果多个目录, 用逗号分开。.{}限定导入的格式
#s.source_files = "WhatKeyboardDemo/WhatKeyboardDemo/WhatKeyboard/classes/**/*.{h,m}"
#s.source_files = "WhatKeyboardDemo/WhatKeyboardDemo/WhatKeyboard/**/*.{h,m}"

#框架被其他工程引入时, 导入的资源包括图片、bundle、xib、storyboard等
s.resources = "WhatKeyboard/resource/*.{bundle}", "WhatKeyboard/classes/*.{xib,storyboard,nib}"

# 框架是否使用的ARC
s.requires_arc = true

#框架依赖的framework
#s.framework    = 'CoreData'

#框架依赖的其他第三方库
#s.dependency 'MagicalRecord', :git => 'https://github.com/iiiyu/MagicalRecord.git', :tag => 'sumiGridDiary2.1'
#s.dependency 'MBProgressHUD'

#框架公开的头文件
#s.public_header_files = 'WhatKeyboardDemo/WhatKeyboardDemo/WhatKeyboard/**/*.h'

end
```
* 验证配置文件是否正确
```
pod lib lint
```
如果提示`WhatKeyboard passed validation`, 即表示正确.

## 打tag, 上传到github

* 首先需要打一个tag, 这个tag需要是`.podspec`文件中的version值.
```
git tag '1.0.0' 
```
* 然后上传到github
```
git add .
git commit -m '提交内容'
git push --tags 
```
* <font color=red size=20>通过trunk上传你的podspec文件</font>, 在仓库目录执行下面的命令
```
pod trunk push 你的podspec文件名
```
* 删除指定tag的Cocoapod支持
```
pod trunk delete WhatKeyboard 1.0.0
```
出现下面结果, 表示成功了
```
 🎉  Congrats

 🚀  WhatKeyboard (1.0.0) successfully published
 📅  September 13th, 00:33
 🌎  https://cocoapods.org/pods/WhatKeyboard
 👍  Tell your friends!
 ```
* 测试: `pod search WhatKeyboard`, 如果没有成功, 可以先更新一下本地依赖库`pod setup`


## 其他
* 图片问题
图片建议新建一个`Settings Bundle`, 将图片放入该bundle中, 加载图片时使用`[UIImage imageNamed:@"xxx.bundle/xxxx"]`
* xib等加载问题
不能再使用`[NSBudle mainBundle]`了, 需要改成下面的方式: 
```
[[NSBundle bundleForClass:[self class]] loadNibNamed:NSStringFromClass(self) owner:nil options:nil].firstObject;
```
* 注意事项
Cocoapods是根据仓库中的tag值进行安装的, 所以必须打tag, `.podspec`文件中的version也必须和tag值一样.
* 更新
	* 需要上传代码到github仓库, 然后打一个新tag.
	* 上传`.podspec`文件到trunk上面
		* 在仓库中运行: `pod trunk push 你的podspec文件名`
* 取消对Cocoapods的支持, WhatKeyboard是项目名称, 1.0.0是对应的tag值
```
pod trunk delete WhatKeyboard 1.0.0
```