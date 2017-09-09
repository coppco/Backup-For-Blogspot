---
layout: post
title: Xcode8安装低版本SDK, 支持iOS7.0的真机测试
comments: true
date: 2017-03-15 14:07:22
tags:
    - Xcode
    - iOS
    - Objective-C
    - Swift
    - 资料整理
    - IDE
---

最近新安装了Xcode8, 但是发现Deployment Target最低是8.0, 虽然可以手动改为7.0, 但是真机测试的时候还是不能完成, 而且模拟器最低版本也是为iPhone 5的 iOS 8.0, 不能使用iPhone 4s设备进行模拟器测试.

<!--more-->

# <font color=orange>一、Deployment Target最低为7.0并且可以在iOS 7.0版本真机测试
![真机测试报错](http://oak4eha4y.bkt.clouddn.com/Xcode8_iOS7.png)

1. 从Xcode7或者更低版本的Xcode里面提取DeviceSupport支持文件
>   Finder------右键------前往文件夹
    /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport
    
2. 将上述文件夹里面的7.0和7.1文件夹拷贝到Xcode8同样的目录下
3. 修改SDKSettings文件, 重启Xcode即可
>   /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/SDKSettings.plist
    * 注意拷贝进去几个文件夹就新增几个字段, 而且要按从小到大的顺序排列
    * 另外有的系统可能无法修改, 可以先把文件拷贝出来, 修改后再覆盖进去(需要输入管理员密码鉴定)
![真机测试报错](http://oak4eha4y.bkt.clouddn.com/SDKSetting.png)
4. 最后Deploymeng Target最低可以选择iOS7.0, 并且在iOS7.0的手机上面也可以真机了.
![真机测试报错](http://oak4eha4y.bkt.clouddn.com/DeploymentTarget_7.0.png)
5. 百度云下载密码:6g6j[下载地址](https://pan.baidu.com/s/1o7TJ6zw)

# <font color=orange>二、为Xcode8添加iPhone 4S模拟器
1. 方式1可以通过Xcode------偏好设置------Components------下载对应的Simlator(Xcode8 可以下载的Simlator SDK最低版本是iOS 8.1, 而且基本上因为墙的缘故会很慢!)
2. 手动下载安装, 将下载好的Simlator SDK放入下面的目录
>   Finder------右键------前往文件夹
    /Library/Developer/CoreSimulator/Profiles/Runtimes/
3. 百度云下载密码:cs7e[下载地址](https://pan.baidu.com/s/1gf7Tzs3)
