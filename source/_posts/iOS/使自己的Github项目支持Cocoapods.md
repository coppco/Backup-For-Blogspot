---
layout: post
title: 使自己的Github项目支持Cocoapods以及私有远程Repo仓库
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
<img src="http://47.96.147.179/images/iOS/github_cocoapods.jpg" alt="hello" style="width: 50%; text-align: center; display: block; margin-top:30px; margin-bottom:30px"/>
<!--more-->

# <font color=orange>项目支持Cocoapods</font>
这里演示的是Githu项目, 可以视自己的情况时有Coding、开源中国等Git服务器.
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

将你的项目添加到仓库中, 或者使用下面命令创建新项目

> pod lib create 项目名称

## 配置 .podspec文件
使用上一步的命令创建的项目会自动创建`xxx.podspec`, 如果是自己新建的项目则没有, 需要自己创建。
* 在仓库中新建配置文件

```
pod spec create 项目名称
```

* 配置文件中字段说明, [官方文档](http://guides.cocoapods.org/syntax/podspec.html)

```
Pod::Spec.new do |s|

# 名称 使用的时候pod search [name]
s.name = "WhatKeyboard"

# 代码库的版本, 需要和代码仓库中的tag值一致
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

#新建目录, 如果有多个表示可以分库, 引入时也可以单独引入分库
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
#本地验证
pod lib lint xxx.podspec
#远程验证
pod spec lint xxx.podspec
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

# <font color=orange>搭建远程私有Repo库</font>

有时候公司有多个项目, 每个项目都用到相同的组件, 但是我们又不希望开源这些组件, 我们可以创建私有远程Repo库, 它有点类似于Maven私服.

* 1、创建一个远程私有Spec库, 可以在远程的git服务器上
* 2、将远程私有Spec库的git地址加入repo, 注意修改xxx名称
```
pod repo add xxx 远程私有repo的git地址
```
	* 执行完之后会在`/Users/用户名/.cocoapods/repos/`下会生成`xxx`文件夹.
		* 验证是否正确: 在`/Users/用户名/.cocoapods/repos/xxx`目录下运行: 
```
pod repo lint .
```
		* 如果出现`All the specs passed validation.`即表示添加成功.
		* 显示本地repo列表: `pod repo list`
		* 删除本地私有repo: `pod repo remove xxx`
		* 更新某个repo: `pod repo update xxx`
* 3、创建一个代码git仓库, 并克隆到本地, 然后在代码仓库中运行
```
pod spec create 你的项目名
```
	* 修改`你的项目名.podspec`文件, 根据实际情况修改
```
Pod::Spec.new do |s|
    #搜索时名称
    s.name                = 'WhatKeyboard'
    #tag值
    s.version             = '1.1.2'
    #描述
    s.summary          = '自定义密码输入键盘'
    #主页
    s.homepage         = 'https://github.com/coppco/WhatKeyboard'
    #开源协议
    s.license              = 'MIT'
    #作者和邮箱
    s.author               = { 'coppco' => 'coppco@qq.com' }      
    #支持的系统和最低版本
    s.platform           = :ios, '7.1'
    #git地址和tag值
    s.source               = { :git => 'https://github.com/coppco/WhatKeyboard.git', :tag => s.version}
    #默认目录
    s.default_subspec = 'Core'
    #分目录
    s.subspec 'Core' do |ss|
        #代码路径
        ss.source_files = 'WhatKeyboard-master/*.{h,m}'
    end
    #资源路径
    s.resources           = 'WhatKeyboard-master/*.{xib,storyboard,nib,bundle}'
    #是否ARC
    s.requires_arc      = true
    #依赖的其他库
    #s.dependency  'AFNetworking','3.1.0'
end
```
* 4、验证你的podspec文件
	* 本地验证
```
pod lib lint 项目名.podspec --verbose --use-libraries --allow-warnings
```
	* 远程验证
```
pod spec lint 项目名.podspec --verbose --use-libraries --allow-warnings
```
	* `--verbose`: 查看详细的验证过程
	* `--use-libraries`: 如果你的库使用了静态库或者引用的三方库使用了静态库, 验证无法通过
	* `--allow-warnings`: 允许警告
* 5、推送podspec文件到远程私有Cocoapods库
```
#名字xx必须和第二步名字一样
pod repo push xxx spec文件名称.podspec
```
	* 推送成功之后, 远程的repo仓库会添加`/spec文件名称/tag/spec文件名称.podspec`文件夹和文件.
* 6、修改工程podfile文件, 引入远程私有repo库的git地址
```
source 'https://xxx.xxx.xxx/xxx/xxx.git
source 'https://github.com/CocoaPods/Specs.git'

platform :ios, '9.0'

target 'Example' do
	pod 'xxx'
end

```
* 7、后期更新远程私有repo
	* 代码仓库打tag, 并推送到远程代码库
	* 修改podspec文件, 将`s.version`改为代码仓库的tag值
	* 验证podspec文件
	* podspec文件验证通过, 更新本地远程私有仓库


# <font color=orange>上传podspec到github时报错</font>
今天更新项目的github时, 报如下错误: 

>Encountered an unknown error (/usr/bin/xcrun simctl list -j devices
xcrun: error: unable to find utility "simctl", not a developer tool or in PATH
) during validation.

* 解决办法
>`Xcode`--------`Command + ,`--------`Locations`--------`Command Line Tools`设置一下即可



# <font color=orange> CocoaPods和SVN配合使用 </font>

2019年, 新入职公司代码控制工具使用的是SVN, 所以又研究了一下CocoaPods和SVN结合使用。

* 1、首先在SVN服务器创建项目目录
	* 需要包含正确的目录: `branches`、`tags`、`trunk`
* 2、在`trunk`目录中添加项目以及`xxx.podspec`
	* 也可以使用`pod lib create xxx`自动生成
* 3、添加代码并配置好`xxx.podspec`
	* 这里需要注意的是`xxx.podspec`里面的配置
	> Pod::Spec.new do |s|
	> s.name         = "xxx"
	> #<font color=red>注意这里的version必须是SVN中该项目存在的tag值</font>
	> s.version      = "1.0.0"
	> s.summary      = "封装xxx"
	> s.homepage     = "http://www.xxx.com"
	> s.license      = "MIT"
	> s.author             = { "xxx" => "xxx@xxx.com" }
	> s.ios.deployment_target = "7.0"
	> s.platform           = :ios, '7.0'
	> 
	> #<font color=red>这里需要修改为你项目所在SVN的地址</font>
	> s.source       = { :git => "http://192.168.1.100/iOSComponent/xxx", :tag => "#{s.version}" }
	> 
	> s.public_header_files = 'xxx/*.h'
	>
	> s.source_files = 'xxx/*.{h,m}'
	> 
	> s.requires_arc      = true
	> 
	> end

* 4、代码提交到SVN服务器
* 5、选中`trunk`文件夹, 添加tag
	* 添加完成之后, 会在`tags`文件夹下面自动生成对应的tag文件夹
* 6、在`Profile`文件中添加私有库的引用
	> #<font color=red>这里注意SVN地址是项目所在的SVN地址,tag就是项目中已经添加的tag</font>
	> pod 'xxx',:svn =>'http://192.168.1.100/iOSComponent/xxx',:tag =>'1.0.0'
* 7、执行`pod install`完成导入
	* <font color=red>这里需要注意的是: 由于SVN一般有权限, 所以在第一次执行之前需要我们手动`svn check`一次, 然后输入有该项目权限的用户名和密码</font>