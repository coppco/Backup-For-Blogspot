---
layout: post
title: iTunes Connect构建版本不显示
comments: true
toc: true
date: 2018-01-02 08:53:57
tags:
	- iOS
	- Swift
	- Objective-C
	- 坑点
	- 资料整理
---

最近稍微空闲了, 整理一下上传ipa到iTunes Connect时, 上传成功但是构建版本一直不显示的问题.

<!--more-->

## <font color=orange> 问题说明 </font>

公司的主项目上App Store时间的较早, 大约在2015年上传的, 之后一直都是迭代迭代, 老项目上传iTunes Connect没有出现上传成功但是不显示的问题.

最近公司新项目需要上传, 申请邓白氏编码、Apple开发者账户、证书、在iTunes Connect上面新建App都没有问题.但是在Xcode打包上传到App Store后,在iTunes Connect构建版本中居然找不到构建版本~~~

这里注意的是使用Xcode或者Application Loader上传时, 上传结果明明是Successfull, 但是iTunes Connect中构建版本却没有构建版本.此时说明你上传了无效的ipa包.

## <font color=orange> 解决方法 </font>
大致原因主要有: 
* 苹果服务器经常性抽风, 多上传几次构建版本会出来
* 苹果关于App的政策发生变化, 导致上传无效的ipa包.

第二种情况下, 使用Application Loader或其他途径上传iTunes Connect成功后, 如果是无效的ipa, 苹果可能会给你的开发者账号邮箱发送邮件, 可能由于<font color=red>使用了私有API</font>或者其他问题, 如<font color=red>从iOS10开始,苹果更加注重对用于隐私的保护,App 里边如果需要访问用户隐私,必须要做描述,所以要在 plist 文件中添加描述, 麦克风权限、相机权限和相册权限是必须添加的, 即使的你App没有用到这些功能</font>~~~~.

* 私有API
	* 检查自己的代码, 发现有使用私有API的去掉
	* 检查使用的第三方SDK, 咨询相关客服有没有使用私有API
* iOS 10以后权限
	* 在`Info.plist`中必须添加麦克风、相机和相册权限
```xml
<!-- 麦克风 --> 
<key>NSMicrophoneUsageDescription</key> 
<string>XXX需要访问麦克风</string> 
<!-- 相机 --> 
<key>NSCameraUsageDescription</key> 
<string>XXX需要访问相机</string> 
<!-- 相册 --> 
<key>NSPhotoLibraryUsageDescription</key> 
<string>XXX需要访问相册</string> 
```
	* 其他权限视情况添加
```xml
<!-- 位置 --> 
<key>NSLocationUsageDescription</key> 
<string>XXX需要访问位置</string> 
<!-- 在使用期间访问位置 --> 
<key>NSLocationWhenInUseUsageDescription</key> 
<string>XXX需要在使用期间访问位置</string> 
<!-- 始终访问位置 --> 
<key>NSLocationAlwaysUsageDescription</key> 
<string>XXX需要始终访问位置</string> 
<!-- 日历 --> 
<key>NSCalendarsUsageDescription</key> 
<string>XXX需要访问日历</string> 
<!-- 提醒事项 --> 
<key>NSRemindersUsageDescription</key> 
<string>XXX需要访问提醒事项</string> 
<!-- 运动与健身 --> 
<key>NSMotionUsageDescription</key> <string>XXX需要访问运动与健身</string> 
<!-- 健康更新 --> 
<key>NSHealthUpdateUsageDescription</key> 
<string>XXX需要访问健康更新 </string> 
<!-- 健康分享 --> 
<key>NSHealthShareUsageDescription</key> 
<string>XXX需要访问健康分享</string> 
<!-- 蓝牙 --> 
<key>NSBluetoothPeripheralUsageDescription</key> 
<string>XXX需要访问蓝牙</string> 
<!-- 媒体资料库 --> 
<key>NSXXXleMusicUsageDescription</key> 
<string>XXX需要访问媒体资料库</string>
```

## <font color=orange> 其他 </font>
通过Application Loader上传ipa过慢, 解决方法: 
```
cd ~      
mv .itmstransporter/ .old_itmstransporter/      
"/Applications/Xcode.app/Contents/Applications/Application Loader.app/Contents/itms/bin/iTMSTransporter"  
```

上传ipa包, 除了使用Application Loader和Xcode, 还可以使用 Appuploader和fastlane等.

## <font color=orange> 后记 </font>
在添加完麦克风、相机和相册的权限信息之后, 重新打包上传成功后果然在iTunes Connect中出现了构建版本, 心中一万个草泥马路过...
