---
layout: post
title: iOS之App瘦身实践
comments: true
toc: true
date: 2019-02-01 09:19:34
tags:
	- iOS
	- 资料整理
	- App瘦身
---

<img src="http://47.96.147.179/images/iOS/iOS_App_thinning.jpeg" alt="hello" style="width: 70%; text-align: center; display: block; margin-top:30px; margin-bottom:30px"/>

随着时间的推移、项目开发和迭代, App包体积越来越大, App瘦身势在必行!

<!--more-->

## <font color=orange> 分析App的组成 </font>

* 资源文件
	* 数据库、配置文件、数据文件
	* 字体文件
	* 图片
* 源代码

### <font color=orange> 生成LinkMap</font>

通过生成`LinkMap`文件可以分析生成ipa包的内容组成:
`Xcode`--->`TARGETS`--->`Build Settings`--->`搜索Link Map`--->在`Write Link Map File`中把Debug中的值改为YES, Release中改为NO--->在`Path to Link Map File`中是生成文件的路径, 一般默认在`~/Library/Developer/Xcode/DerivedData/XXX-eumsvrzbvgfofvbfsoqokmjprvuh/Build/Intermediates.noindex/XXX.build/Debug-iphoneos/XXX.build/XXX-LinkMap-normal-arm64.txt`

LinkMap会包含每个可执行文件的偏移量及大小，所以可以很方便的知道每个可执行文件的大小。可以通过[LinkMap分析工具](https://github.com/huanxsd/LinkMap)快速分析App内组成。

## <font color=orange> 图片 </font>

### <font color=orange> 图片压缩 </font>
图片是相当占用资源的, 对于一些比较大的图片, 我们可以无损压缩一下, 这样可以节约60%的图片大小的空间。

* 在线压缩
	* [tinypng](https://tinypng.com/)
* 软件工具
	* [ImageOptim](https://github.com/ImageOptim/ImageOptim)

### <font color=orange> 查找未使用的图片 </font>
以下两种方式删除图片时都需要谨慎, 最好删除之前项目中搜索一下。
* 方式一: 通过`ack`命令自己写一个脚本

```shell
#判断是否安装了ack命令, 没有则安装
function checkAckAndInstall() {
  if hash ack ; then
    eturn 1
  else
    echo "brew install ack"
    echo "brew install ack" >> $logFile
    `brew install ack`
    if [[ $? = "0" ]]; then
      echo "安装ack失败"
      echo "Install ack failed!" >> $logFile
      return 0
    else
      echo "install ack success"
      echo "Install ack success" >> $logFile
      return 1
    fi
  fi
}

checkAckAndInstall

#安装出错
if [[ $? -eq 0 ]]; then
  echo "ACK命令未安装并且brew install ack失败, 请先安装ack"
  exit
fi

#查找所有图片
for i in `find . -name "*.png" -o -name "*.jpg" -o -name "*.jpeg" -o -name "*.gif"`; do
  #图片名
  file=`basename -s .jpg "$i" | xargs basename -s .png | xargs basename -s @2x | xargs basename -s @3x`
  #查找
  result=`ack -i "$file"`
  #如果查找结果为空
  if [ -z "$result" ]; then
    echo "发现未使用图片: $i"
  fi
done
```
* 方式二: [LSUnusedResources](https://github.com/tinymind/LSUnusedResources)

## <font color=orange> 字体 </font>
### <font color=orange> 系统字体 </font>
系统字体, 但不在预装字体列表中, 注意: 退出当前控制器或者App重启后都需要重新下载

```objc
//用字体的PostScript名字创建一个Dictionary    NSMutableDictionary *attrs = [NSMutableDictionary dictionaryWithObjectsAndKeys:fontName, kCTFontNameAttribute, nil];
    
// 创建一个字体描述对象CTFontDescriptorRef
CTFontDescriptorRef desc = CTFontDescriptorCreateWithAttributes((__bridge CFDictionaryRef)attrs);
    
//将字体描述对象放到一个NSMutableArray中
NSMutableArray *descs = [NSMutableArray arrayWithCapacity:0];
[descs addObject:(__bridge id)desc];
CFRelease(desc);

//匹配字体
CTFontDescriptorMatchFontDescriptorsWithProgressHandler((__bridge CFArrayRef)descs, NULL, ^bool(CTFontDescriptorMatchingState state, CFDictionaryRef  _Nonnull progressParameter) {
        
  if (state == kCTFontDescriptorMatchingDidBegin) {//字体已经匹配
        
  } else if (state == kCTFontDescriptorMatchingDidFinish) {
    if (!errorDuringDownload) {
      //下载完成
    }
  }
  return (BOOL)YES;
});
```
### <font color=orange> 自定义字体 </font>
字体文件相对来说比较大, 我们可以把字体文件放在服务器, 使用的时候从服务器下载后再使用。可以一次下载多次使用.

```objc
//下载字体后, 本地沙盒路径
NSURL *fontUrl = [NSURL fileURLWithPath:path];
CGDataProviderRef fontDataProvider = CGDataProviderCreateWithURL((__bridge CFURLRef)fontUrl);    
CGFontRef fontRef = CGFontCreateWithDataProvider(fontDataProvider);
CGDataProviderRelease(fontDataProvider);
CTFontManagerRegisterGraphicsFont(fontRef, NULL);
//字体名称
NSString *fontName = CFBridgingRelease(CGFontCopyPostScriptName(fontRef));
CGFontRelease(fontRef);
```

## <font color=orange> 代码 </font>
### <font color=orange> 第三方库 </font>
项目中或多或少的使用一些第三方SDK, 清理一些不使用的SDK, 或者根据需要使用精简版的SDK。
### <font color=orange> bitcode </font>
Xcode中要使用bitcode要求说有的SDK都必须支持bitcode, 可以在`Xcode`--->`PROJECT`--->`Build Settings`--->搜索`bitcode`开启。
### <font color=orange> 未使用的类 </font>
注意防止误删!
* [AppCode](https://blog.jetbrains.com/objc/2014/01/appcode-inspections-for-your-code-perfection/)
* [Fui](https://github.com/dblock/fui)