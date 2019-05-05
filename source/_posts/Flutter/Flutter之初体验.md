---
layout: post
title: Flutter之初体验
comments: true
toc: true
date: 2019-04-25 09:34:32
tags:
	- iOS
	- Flutter
	- Android
---

<img src="http://47.96.147.179/images/iOS/Flutter.jpeg" alt="hello" style="width: 70%; text-align: center; display: block; margin-top:30px; margin-bottom:30px"/>

[Flutter](https://flutter.dev/)是Google推出的移动UI框架, 可以快速在iOS和Android上构建高质量的原生用户界面。

[Flutter遇到的问题](/2019/05/05/Flutter/Flutter填坑指南/)

<!--more-->

# <font color=orange> 安装Flutter </font>

Flutter可以安装在`Windows`、`macOS`和`Linux`平台上, 可以参考[官方安装向导](https://flutter.dev/docs/get-started/install)。

这里我介绍一下在**macOS**上面安装Flutter的步骤, 以及安装Android Studio等。

## <font color=orange> 系统需求 </font>
* 操作系统: macOS (64-bit)
* 磁盘空间: 700 MB (不包含IDE和工具).
* 命令: bash、curl、git 2.x、mkdir、rm、unzip、which

## <font color=orange> 安装FLutterSDK </font>
* 1、下载FLutter SDK, 截止目前(2019-04-25), 最新版本是v1.2.1
	* [下载地址](https://flutter.dev/docs/development/tools/sdk/releases?tab=macos)
* 2、解压到安装目录, 我一般安装到/usr/local/bin
	> cd /usr/local/bin
	> unzip ~/Downloads/flutter_macos_v1.2.1-stable.zip
* 3、修改`/etc/porfile`文件, 在文件末尾添加
	> export PATH=/usr/local/bin/flutter/bin:$PATH
* 4、使修改配置生效
	> source /etc/profile
* 5、预下载(可选)
	> flutter precache
	
## <font color=orange> 运行Flutter 医生 </font>
该命令可以检查当前机器的Flutter环境、Android环境、iOS环境等。
> flutter doctor

结果如下: 
```
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, v1.2.1, on Mac OS X 10.14.2 18C54, locale
    zh-Hans-CN)
[!] Android toolchain - develop for Android devices (Android SDK version 28.0.3)
    ! Some Android licenses not accepted.  To resolve this, run: flutter doctor
      --android-licenses
[✓] iOS toolchain - develop for iOS devices (Xcode 10.1)
[!] Android Studio (version 3.4)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[✓] Connected device (1 available)

! Doctor found issues in 2 categories.
```

Flutter默认使用Google 分析工具提交使用报告和崩溃信息等, 在国内由于墙的原因或者不想上传可以关闭。
> 关闭
> flutter config --no-analytics
> 开启
> flutter config --analytics

## <font color=orange> iOS </font>
### <font color=orange> 安装Xcode并配置 </font>
* 1、安装Xcode, 需要9.0以后版本
* 2、配置Xcode命令行使用最新安装的Xcode
	> sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
* 3、打开Xcode一次确保Xcode许可协议签署,或者命令行运行:  
	> sudo xcodebuild -license

### <font color=orange> iOS simulator </font>
运行下面命令打开一个iOS模拟器
> open -a Simulator

### <font color=orange> iOS 设备 </font>
* 1、安装homebrew
	> /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
* 2、更新homebrew
	> brew update
* 3、安装其他工具
	> brew install --HEAD usbmuxd
	> brew link usbmuxd
	> brew install --HEAD libimobiledevice
	> brew install ideviceinstaller ios-deploy cocoapods
	> pod setup
* 4、运行Flutter项目中的ios/Runner.xcworkspace, 给项目设置Bundle Identifier、证书和描述文件等
* 5、使用数据线连接iPhone到电脑, 并信任电脑
* 6、运行`flutter run`

## <font color=orange> Android </font>

### <font color=orange> 安装Android Studio </font>
* 1、[官网](https://developer.android.com/studio)下载安装Android Sdudio
* 2、打开Android Studio, 然后安装通过`Android Studio Setup Wizard`页面
* 3、Android授权, 运行下面命令
	> flutter doctor --android-licenses
* 4、Android Studio安装`Flutter`和`Dark`插件, 打开`Android Studio`--->打开偏好设置`Preferences`--->插件`Plugins`--->搜索Flutter--->安装Flutter插件--->重启Android Studio

### <font color=orange> Android emulator </font>
* 1、开启VM acceleration
* 2、打开`Android Studio`--->`Configure`--->`AVD Manager`--->`Create Virtual Device`
* 3、选择一个设备然后点击`Next`
* 4、首先下载镜像, 然后选择一个或者多个镜像, 点击`Next`
* 5、在`Verify Configuartion`页面中的`Graphics`选项选择`Hardware - GLES 2.0`后点击`Finish`
* 6、在`Android Virtual Device Manager`页面启动添加的模拟器

### <font color=orange> Android 设备 </font>
* 1、允许开发者选项和USB调试
* 2、如果是Windows, 安装`Google USB Driver`
* 3、使用USB线连接电脑, 并信任电脑
* 4、在命令行中输入下面的命令并允许, 检测你的设备
	>	flutter devices

# <font color=orange> 创建Flutter项目 </font>

目前创建Flutter项目支持一下几种方式: 

## <font color=orange> Android Studio/IntelliJ </font>

* 1.1、选择`New Flutter Project`
* 1.2、选择`Flutter application`
* 1.3、确保`Flutter SDK Path`正确
* 1.4、填写项目名称、描述以及项目存储位置
* 1.5、点击完成

## <font color=orange> Visual Studio Code </font>

* 2.1、`View`--->`Command Palette`
* 2.2、Type`Flutter`, 选择`Flutter: New Project`
* 2.3、填写项目名称、描述以及项目存储位置
* 2.4、点击完成

## <font color=orange> Terminal & editor </font>

* 3.1、进入项目存放目录, 创建项目
	> mkdir -p ~/Desktop/Flutter
	> cd ~/Desktop/Flutter
	> flutter create myApp
* 3.2、打开模拟器
	> 设备
	> flutter devices
	> 模拟器
	> flutter emulators
	> flutter emulators --launch emulatorid
* 3.2、运行
	> 当前只打开一个模拟器时, 直接运行
	> flutter run
	> 在所有模拟器运行
	> flutter run -d all
	> 指定模拟器运行
	> flutter run -d deviceId

# <font color=orange> 运行效果 </font>

<center>
<img src="http://47.96.147.179/images/iOS/flutter_demo_app.png" alt="运行效果" style="width: 100%; text-align: center; display: block;"/>
</center>	