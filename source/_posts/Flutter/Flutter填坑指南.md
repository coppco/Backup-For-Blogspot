---
layout: post
title: Flutter填坑指南
comments: true
toc: true
date: 2019-05-05 17:02:02
tags:
	- iOS
	- Flutter
	- Android
	- 资料整理
---

<img src="http://47.96.147.179/images/iOS/Flutter.jpeg" alt="hello" style="width: 70%; text-align: center; display: block; margin-top:30px; margin-bottom:30px"/>

[Flutter](https://flutter.dev/)是Google推出的移动UI框架, 可以快速在iOS和Android上构建高质量的原生用户界面。

本文主要记录相关Flutter遇到的坑!!!

<!--more-->

# <font color=orange> Flutter卡在**Running "flutter packages get" in project_name...** </font>

当创建项目/运行项目时
```
Running "flutter packages get" in project_name...
```

[官方解决](https://flutter.dev/community/china)办法
> export PUB_HOSTED_URL=https://pub.flutter-io.cn
> export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn

# <font color=orange> Android项目运行时出错 </font>

## <font color=orange> 卡在**Initializing gradle...** </font>

运行时会卡在`Initializing gradle...`, 此时因为Android项目会用到`Gradle`, 如果没有FQ,下载速度会非常慢, 此时我们可以在项目中的`android/gradle/wrapper/gradle-wrapper.properties`中查看`gradle`版本号以及[地址](http://services.gradle.org/distributions/), 我们可以手动下载然后解压到`~/.gradle/wrapper/dists/`中。

## <font color=orange> 卡在**Running 'gradle assembleDebug** </font>

运行时会卡在`Running 'gradle assembleDebug`, 因为Gradle的Maven仓库在国外, 可以使用阿里云的镜像地址。
	* 修改项目中`android/build.gradle`文件
```
buildscript {
    repositories {
        //修改的地方
        //google()
        //jcenter()
        maven { url 'https://maven.aliyun.com/repository/google' }
        maven { url 'https://maven.aliyun.com/repository/jcenter' }
        maven { url 'http://maven.aliyun.com/nexus/content/groups/public' }
    }

    dependencies {
        classpath 'com.android.tools.build:gradle:3.2.1'
    }
}

allprojects {
    repositories {
        //修改的地方
        //google()
        //jcenter()
        maven { url 'https://maven.aliyun.com/repository/google' }
        maven { url 'https://maven.aliyun.com/repository/jcenter' }
        maven { url 'http://maven.aliyun.com/nexus/content/groups/public' }
    }
}

rootProject.buildDir = '../build'
subprojects {
    project.buildDir = "${rootProject.buildDir}/${project.name}"
}
subprojects {
    project.evaluationDependsOn(':app')
}

task clean(type: Delete) {
    delete rootProject.buildDir
}

```
	* 修改Flutter的配置文件, 该文件在`Flutter安装目录/packages/flutter_tools/gradle/flutter.gradle`
```
buildscript {
    repositories {
        //修改的地方
        //google()
        //jcenter()
        maven { url 'https://maven.aliyun.com/repository/google' }
        maven { url 'https://maven.aliyun.com/repository/jcenter' }
        maven { url 'http://maven.aliyun.com/nexus/content/groups/public' }
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.2.1'
    }
}
```

