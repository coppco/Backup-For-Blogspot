---
layout: post
title: iOS 应用内版本更新检查
comments: true
date: 2016-03-13 11:37:01
tags:
    - iOS
    - Objective-C
    - Swift
---
2015年初苹果禁止在应用内新版本提示并跳转到App Store内, 如果存在并被审核人员发现, 唯一的结局就是Reject  ---- 无法通过审核. 但是有的App还是绕过了苹果的审核, 实现了App内弹出更新提示. 根本原因在于隐藏了更新提示弹窗.
<!--more-->

## <font color=orange>1、获取App在App Store内的版本信息, 有两种方式</font>
* 根据应用的Apple ID号获取, 第一次发布的可以通过iTunes Connect里面的Apple ID获取
    * https://itunes.apple.com/lookup?id=你的Apple ID号码
* 根据bundle Identifier获取
    * https://itunes.apple.com/lookup?bundleId=你的bundle Identifier

### 备注:对于某些只在国内发布的App可能获取到的数组为0, 这个时候可以把https://itunes.apple.com/lookup改为https://itunes.apple.com/cn/lookup

## <font color=orange>2、根据获取到的版本信息和本地版本信息进行对比, 决定是否显示弹窗</font>
* 本地version版本号
    * [[[NSBundle mainBundle] infoDictionary] objectForKey:@"CFBundleShortVersionString"]
* 本地构建版本号
    * [[[NSBundle mainBundle] infoDictionary] objectForKey:@"CFBundleVersion"]

![版本号和构建版本号](http://oak4eha4y.bkt.clouddn.com/version_build.png)

## 3、判断版本是否需要提示(这里是关键, 需要进行判断才显示, 不然会被拒)

    //本地版本号
    NSString *localVersion = [[[NSBundle mainBundle] infoDictionary] objectForKey:@"CFBundleShortVersionString"];
    //App Store版本号
    NSString *appStoreVersion = @"通过网络请求获取到的App Store版本号";

    if ([localVersion compare:appStoreVersion options:(NSNumericSearch)] == NSOrderedAscending) { //本地版本比App Store版本低才弹窗
        //更新提示
    }


