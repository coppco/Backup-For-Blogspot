---
layout: post
title: iOS自动编译打包脚本
comments: true
toc: true
date: 2018-08-26 13:37:04
tags:
	- iOS
	- Swift
	- Objective-C
	- Xcode
	- 持续集成
	- Shell
---

<img src="http://47.96.147.179/images/iOS/iOS_auto_build_ipa.jpeg" alt="hello" style="width: 70%; text-align: center; display: block; margin-top:30px; margin-bottom:30px"/>

最近闲暇时间研究了一下Xcode自动编译、打包以及通过脚本更改一个配置文件, 主要涉及的命令如下: 

* xcpretty
	* 美化xcodebuild的输出日志
* xcodebuild
	* 苹果发布自动构建的工具
* PlistBuddy
	* Mac自带的专门解析plist的小工具
* grep
	* 文本搜索工具
* awk
	* 文本分析工具
* sed
	* 文本编辑工具

这里只是简单介绍了这些命令的几种用法, 需要深入了解的可自行搜索学习。
<!--more-->

## <font color=orange> xcpretty </font>
xcpretty是一个针对于xcodebuild的快速和灵活的格式化程序, [github地址](https://github.com/xcpretty/xcpretty)。

### <font color=orange> 安装xcpretty </font>
>	sudo gem install

也可以自定义安装目录

>	gem install -n /usr/local/bin xcpretty

### <font color=orange> 使用xcpretty </font>

>	xcodebuild [flags] | xcpretty

或者在脚本中, 执行失败时退出

>	xcodebuild [flags] | xcpretty && exit ${PIPESTATUS[0]}

## <font color=orange> grep </font>
Linux grep命令用于查找文件里符合条件的字符串。

>	查找

```
#-m1表示取第一个结果
#-C2表示查找匹配项的下面两行
grep [-e] "要查找的字符串" 文件路径 [-m1] [-C2]
grep -E "正则表达式" 文件路径
```

## <font color=orange> awk </font>
AWK是一种处理文本文件的语言，是一个强大的文本分析工具。
配合grep命令, 可以查找Xcode项目中的一些关键配置值。

>	分割

```
#使用,分割然后输出第一个
awk -F, '{print $1}' 文件路径
```

> 示例: 在当前目录中查找Xcode project项目名称, 当grep查询有多个时,可以使用-m1取第一个
> `$(ls | grep xcodeproj -m1 | awk -F.xcodeproj '{print $1}')`

## <font color=orange> sed </font>
sed主要用来自动编辑一个或多个文件
此命令可以用来修改Xcode项目中的.h或者.m文件
>	打印

```
#查找匹配打印
sed -n "/匹配项,可以是正则表达式/p" 文件路径
#打印第四行
sed -n 4p 文件路径
```
>	替换行

```
#字符串替换
sed -i '' -e "s/被替换字符串, 可以是正则表达式/替换字符串/" 文件路径
```
> 示例: 查找并替换某一个内容
> 假如有一个xxx.h文件, 目录是~/Desktop/xxx.h, 内容如下: 
> `#define PUBLISH 1`
> 我们需要修改为`#define PUBLISH 2`
> 执行`sed -i '' -e "s/^#define[ ]*PUBLISH[ ]*[0-9]*[ ]*$/#define PUBLISH 2/" ~/Desktop/xxx.h`即可

## <font color=orange> PlistBuddy </font>
此命令可以用来修改Xcode项目中Info.plist文件中的配置。

>	打印CFBundleShortVersionString对应的值

```
/usr/libexec/PlistBuddy -c "print CFBundleShortVersionString" plist文件路径
```
>	修改CFBundleShortVersionString对应的值

```
/usr/libexec/PlistBuddy -c "Set :CFBundleShortVersionString 1.1" plist文件路径
```

## <font color=orange> xcodebuild </font>
### <font color=orange> 常见命令 </font>
这些命令可以单独使用, 无需指定`-project`、`-workspace`和`-scheme`

>	使用帮助
>	xcodebuild -help
>	man xcodebuild

>	查看当前目录中的Targets、Configurations和Schemes
>	xcodebuild -list [[-project <projectname>]|[-workspace <workspacename>]] [-json]

>	查看已安装的SDK
>	xcodebuild -showsdks

>	查看版本号
>	xcodebuild -version

>	查看简洁用法
>	xcodebuild -usage

默认第一个target,和默认的configuration
对于Xcode workspace, 需要指定`-workspace`和`-scheme`
对于Xcode project,如果有多个project, 你需要指定`-project`

> 常用格式一
> xcodebuild [-project name.xcodeproj] [[-target targetname] ... | -alltargets] -configuration configurationname] -sdk [sdkfullpath | sdkname]] [action ...] [buildsetting=value ...] [-userdefault=value ...]

> 常用格式二    
> xcodebuild [-project name.xcodeproj] -scheme schemename [[-destination destinationspecifier] ...] [-destination-timeout value] -configuration configurationname] -sdk [sdkfullpath | sdkname]] [action ...] buildsetting=value ...] [-userdefault=value ...]

> 常用格式三   
> xcodebuild -workspace name.xcworkspace -scheme schemename [[-destination destinationspecifier] ...] [-destination-timeout value] -configuration configurationname] [-sdk [sdkfullpath | sdkname]] [action ...] [buildsetting=value ...] [-userdefault=value ...]

> 常用格式四
> xcodebuild -exportArchive -archivePath xcarchivepath -exportPath destinationpath -exportOptionsPlist path

> Xcode project常见使用命令
>> clean 
>> xcodebuild clean -configuration Debug/Release -alltargets -project name.xcodeproj
>
>> build
>> xcodebuild build -configuration Debug/Release -project name.xcodeproj
> 
>> archive
>> xcodebuild archive -configuration Debug/Release -project name.xcodeproj -archivePath xcarchivepath
>
>> 导出ipa
>> xcodebuild -exportArchive -archivePath xcarchivepath -exportPath destinationpath -exportOptionsPlist path

> Xcode workspace常见使用命令
> clean
>> xcodebuild clean -configuration Debug/Release -alltargets -workspace name.xcworkspace -scheme schemename
>
>> build
>> xcodebuild build -configuration Debug/Release -workspace name.xcworkspace -scheme schemename
> 
>> archive
>> xcodebuild archive -configuration Debug/Release -workspace name.xcworkspace -scheme schemename -archivePath xcarchivepath
>
>> 导出ipa
>> xcodebuild -exportArchive -archivePath xcarchivepath -exportPath destinationpath -exportOptionsPlist path

### <font color=orange> 常见action </font>

> build
> 编译

> build-for-testing
> 编译并且运行单元测试

> analyze
> 分析

> archive
> 存档

> test
> 

> test-without-building
> 

> install-src
> 复制资源到SRCROOT

> install
> 安装

> clean
> 清理

### <font color=orange> exportOptionsPlist </font>

这个文件可以自己生成, 但是推荐使用Xcode打包编译后, 选择相应的App Store、Ad Hoc、Development导出IPA后, 导出目录中会有一个相应的`ExportOptions.plist`文件, 这样出错的几率小很多。

Available keys for -exportOptionsPlist:

|Key|Value|Description|
|:---:|:---:|:---:|
| compileBitcode | Bool |For non-App Store exports, should Xcode re-compile the app from bitcode? Defaults to YES.|
| destination | String |Determines whether the app is exported locally or uploaded to Apple. Options are export or upload. The available options vary based on the selected distribution method. Defaults to export.|
|embedOnDemandResourcesAssetPacksInBundle | Bool | For non-App Store exports, if the app uses On Demand Resources and this is YES, asset packs are embedded in the app bundle so that the app can be tested without a server to host asset packs. Defaults to YES unless onDemandResourcesAssetPacksBaseURL is specified.|
|generateAppStoreInformation | Bool|For App Store exports, should Xcode generate App Store Information for uploading with iTMSTransporter? Defaults to NO.|
|iCloudContainerEnvironment | String |If the app is using CloudKit, this configures the "com.apple.developer.icloud-container-environment" entitlement. Available options vary depending on the type of provisioning profile used, but may include: Development and Production.|
|installerSigningCertificate | String | For manual signing only. Provide a certificate name, SHA-1 hash, or automatic selector to use for signing. Automatic selectors allow Xcode to pick the newest installed certificate of a particular type. The available automatic selectors are "Mac Installer Distribution" and "Developer ID Installer". Defaults to an automatic certificate selector matching the current distribution method.|
|manifest | Dictionary | non-App Store exports, users can download your app over the web by opening your distribution manifest file in a web browser. To generate a distribution manifest, the value of this key should be a dictionary with three sub-keys: appURL, displayImageURL, fullSizeImageURL. The additional sub-key assetPackManifestURL is required when using on-demand resources.|
|method | String |Describes how Xcode should export the archive. Available options: app-store, validation, package, ad-hoc, enterprise, development, developer-id, and mac-application. The list of options varies based on the type of archive. Defaults to development.|
|onDemandResourcesAssetPacksBaseURL | String | non-App Store exports, if the app uses On Demand Resources and embedOnDemandResourcesAssetPacksInBundle isn't YES, this should be a base URL specifying where asset packs are going to be hosted. This configures the app to download asset packs from the specified URL. |
|provisioningProfiles | Dictionary | For manual signing only. Specify the provisioning profile to use for each executable in your app. Keys in this dictionary are the bundle identifiers of executables; values are the provisioning profile name or UUID to use.|
|signingCertificate | String | For manual signing only. Provide a certificate name, SHA-1 hash, or automatic selector to use for signing. Automatic selectors allow Xcode to pick the newest installed certificate of a particular type. The available automatic selectors are "Mac App Distribution", "iOS Developer", "iOS Distribution", "Developer ID Application", and "Mac Developer". Defaults to an automatic certificate selector matching the current distribution method.|
|signingStyle | String | The signing style to use when re-signing the app for distribution. Options are manual or automatic. Apps that were automatically signed when archived can be signed manually or automatically during distribution, and default to automatic. Apps that were manually signed when archived must be manually signed during distribtion, so the value of signingStyle is ignored.|
|stripSwiftSymbols | Bool |Should symbols be stripped from Swift libraries in your IPA? Defaults to YES.|
|teamID | String | Developer Portal team to use for this export. Defaults to the team used to build the archive.|
|thinning | String |  non-App Store exports, should Xcode thin the package for one or more device variants? Available options: <none> (Xcode produces a non-thinned universal app), <thin-for-all-variants> (Xcode produces a universal app and all available thinned variants), or a model identifier for a specific device (e.g. "iPhone7,1"). Defaults to <none>.|
|uploadBitcode | Bool | For App Store exports, should the package include bitcode? Defaults to YES.|
|uploadSymbols | Bool | For App Store exports, should the package include symbols? Defaults to YES.|

## <font color=orange> 自动上传到App Store或者蒲公英 </font>

### <font color=orange> 自动上传到App Store </font>

> 使用altool上传, 需要ApplID和密码
> /Applications/Xcode.app/Contents/Applications/Application\ Loader.app/Contents/Frameworks/ITunesSoftwareService.framework/Versions/A/Support/altool --validate-app -f ipaPath -u AppleID -p password -t ios --output-format xml

### <font color=orange> 自动上传到蒲公英 </font>

> 使用Shell脚本
> curl -F "file=xxx.ipa" -F "uKey=xxx" -F "_api_key=xxx" https://qiniu-storage.pgyer.com/apiv1/app/upload

## <font color=orange> 结束 </font>
这里我就不放脚本了, 自己动手丰衣足食~~~
可以根据自己项目的配置, 个性化自己的打包脚本!
