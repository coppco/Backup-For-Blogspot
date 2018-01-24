layout: ios
title: iOS 10的一些权限设置
date: 2016-09-18 09:00:33
tags:
    - Xcode
    - iOS
    - Objective-C
    - Swift3.0
---
iOS 10出来一段时间了, 小伙伴们发现以前的应用在iOS 10下面各种崩溃, 这是因为iOS 10开始苹果对于各种手机权限要求更加严格,这点从iOS 8的定位功能,需要在Info.plist里面添加对于的Key就可以窥见一斑.
</hr>
需要下面权限的同学可以在工程的Info.plist里面添加对应的键值对, 也可以Info.plist右键  ----> Open As ----> Source Code直接拷贝进入对于的键值对,其中字符串可以自己改!
<!--more-->
```
<!-- 相册 --> 
<key>NSPhotoLibraryUsageDescription</key> 
<string>XXX需要访问相册</string> 

<!-- 相机 --> 
<key>NSCameraUsageDescription</key> 
<string>XXX需要访问相机</string> 

<!-- 麦克风 --> 
<key>NSMicrophoneUsageDescription</key> 
<string>XXX需要访问麦克风</string> 

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
