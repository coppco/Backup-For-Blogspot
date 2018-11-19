---
layout: post
title: 使用Gitlab-CI实现iOS的可持续集成
comments: true
toc: true
date: 2017-11-29 15:51:46
tags:
    - Objective-C
    - Xcode
    - iOS
    - Swift
    - 持续集成
    - Gitlab
---

持续集成(Continuous Integration)是一种软件开发实践，即团队开发成员经常集成他们的工作，通过每个成员每天至少集成一次，也就意味着每天可能会发生多次集成。每次集成都通过自动化的构建（包括编译，发布，自动化测试）来验证，从而尽早地发现集成错误。

<img src="http://47.96.147.179/images/iOS/gitlab-ci-iOS.jpg" alt="hello" style="width: 50%; text-align: center; display: block; margin-top:30px; margin-bottom:30px"/>

<!--more-->

# <font color=orange> 常见持续集成框架 </font>

* <font color=red>Jenkins</font>: 基于Java语言开发的持续构建/部署开源免费的持续集成框架, 目前使用最多的持续集成框架.
* <font color= red>Buildbot</font>: 基于Python开发而成的项目.
* <font color= red>Travis CI</font>: 开源项目免费使用, 使用Travis Pro需要付费.
* <font color= red>Gitlab-CI</font>: 是Gitlab提供的一个持续集成套件, 在8.x以后版本已经集成进Gitlab中, 使用时配置Gitlab-runner即可.

# <font color=orange> Gitlab-CI </font>

## <font color=orange> 安装Gitalb </font>

### <font color=green> 安装 </font>

使用Gitlab-CI需要首先需要使用[Gitlab](https://www.gitlab.com)或者自己[安装Gitlab](/2017/07/07/Java Web/CentOS 7搭建GitLab/index.html)到自己的服务器, 从8.0版本开始, Gitlab默认已经集成Gitlab-CI.

### <font color= green> 汉化 </font>

* Gitlab v8.8以及之前的版本汉化
	* https://gitlab.com/larryli/gitlab.git
* Gitlab v8.8之后的汉化
	* https://gitlab.com/xhang/gitlab.git
	* 该项目延续了larryli的汉化.

## <font color=orange> 配置Gitlab-runner </font>

### <font color= green> Gitlab-CI和Gitlab-runner的区别 </font>
* Gitlab-CI
	* 它安装在Gitlab服务器
	* 用来管理各个项目的各个runner
* Gitlab-runner
	* 它可安装在不同的操作系统
	* 基于不同IDE、编译环境

### <font color= green> 安装Gitlab-runner </font>

<font color=red>从Gitlab-runner 10.0开始, `gitlab-ci-multi-runner`重新命名为`gitlab-runner`, 安装时需要注意!</font>

##### <font color= green> GitLab Runner和Gitlab版本支持情况 </font>

|GitLab Runner / GitLab	|9.0.x (03.2017)|	9.1.x (04.2017)|	9.2.x (05.2017)|	9.3.x (06.2017)|	9.4.x (07.2017)|	9.5.x (08.2017)|	10.0.x (09.2017)|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|v1.10.x  |✓ |✓	|✓ |✓	| ✓|✓ | ✗|
|v1.11.x	|✓	|✓	|✓	|✓	|✓ |✓ | ✗|
|v9.0.x	|✓	|✓	|✓	|✓	|✓	|✓ | ✓|
|v9.1.x	|✓	|✓	|✓	|✓	|✓	|✓ | ✓|
|v9.2.x	|✓	|✓	|✓	|✓	|✓	|✓ | ✓|
|v9.3.x	|✓	|✓	|✓	|✓	|✓	|✓ | ✓|
|v9.4.x	|✓	|✓	|✓	|✓	|✓	|✓ | ✓|
|v9.5.x	|✓	|✓	|✓	|✓	|✓	|✓ | ✓|
|v10.0.x	|✓	|✓	|✓	|✓	|✓	|✓ | ✓|

##### <font color= green> 不同系统安装gitlab-runner </font>

* [Install using GitLab's repository for Debian/Ubuntu/CentOS/RedHat (preferred)](https://docs.gitlab.com/runner/install/linux-repository.html)
* [Install on GNU/Linux manually (advanced) ](https://docs.gitlab.com/runner/install/linux-manually.html)
* [Install on macOS (preferred) ](https://docs.gitlab.com/runner/install/osx.html)
* [Install on Windows (preferred) ](https://docs.gitlab.com/runner/install/windows.html)
* [Install as a Docker Service ](https://docs.gitlab.com/runner/install/docker.html)
* [Install in Auto-scaling mode using Docker machine ](https://docs.gitlab.com/runner/install/autoscaling.html)
* [Install on FreeBSD ](https://docs.gitlab.com/runner/install/freebsd.html)
* [Install on Kubernetes ](https://docs.gitlab.com/runner/install/kubernetes.html)
* [Install the nightly binary manually (development) ](https://docs.gitlab.com/runner/install/bleeding-edge.html)

##### <font color= green> Mac下安装gitlab-runner </font>
以下是官网10.0以后版本安装步骤:

* 下载可执行文件: `sudo curl --output /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-darwin-amd64`
	* (由于墙的原因安装速度比较慢), 也可以下载该文件然后拷贝到`/usr/local/bin`目录中, 并且重命名为: `gitlab-runner`或者`gitlab-ci-multi-runner`
* 更改权限: `sudo chmod +x /usr/local/bin/gitlab-runner`

##### <font color= green> Linux下安装gitlab-runner </font>

由于墙的原因下载很慢, 对于`ubuntu`、`debian`和`centOS`等Linux系统可以使用[清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/gitlab-ci-multi-runner/)来进行安装, 它里面现在更新到了9.x版本的gitlab-runner.

如CentOS下使用yum安装: 
```
#1、创建`gitlab-ci-multi-runner.repo`
vi /etc/yum.repos.d/gitlab-ci-multi-runner.repo

#2、将下面的内容写入并保存
[gitlab-ci-multi-runner]
name=gitlab-ci-multi-runner
baseurl=http://mirrors.tuna.tsinghua.edu.cn/gitlab-ci-multi-runner/yum/el7
repo_gpgcheck=0
gpgcheck=0
enabled=1
gpgkey=https://packages.gitlab.com/gpg.key

#3、运行命令
sudo yum makecache
sudo yum install gitlab-ci-multi-runner
```

#### <font color=green> 注册Gitlab-runner </font>

安装好gitlab-runner以后, 可以根据操作系统来注册runner(注意gitlab-runner版本不同, 命令也不同): 
* Linux和Mac在终端中运行命令: `sudo gitlab-runner register`
* Windows需要`以管理员身份`打开cmd然后运行: `./gitlab-runner.exe register` 

* 第一步输入Gitlab CI地址: 例如: 可以在`Gitlab管理区域中Runners中`查看url或者在`单个项目中的Runners中`查看url.
* 第二步输入项目CI或者共享Token: 决定该Runner是Shared Runner还是针对单个项目的Specific Runner, 可以在`Gitlab管理区域中Runners中`查看Token或者在`单个项目中的Runners中`查看Token. 
* 第三步输入runner描述: runner的描述.
* 第四部输入runner标签: 可以有多个,使用逗号隔开
* v10.0及以后版本才有配置(是否在没有tag时执行任务): 可以在Gitlab UI界面中更改. 
* v10.0及以后版本配置(是否锁定当前runner, 当当前项目执行该runner时): 可以在Gitlab UI界面中更改. 
* 第五步输入runner的执行语言: 可以是`ssh`, `docker+machine`, `docker-ssh+machine`, `kubernetes`, `docker`, `parallels`, `virtualbox`, `docker-ssh`, `shell`.
* 最后在Gitlab中就会有一个runner. 
* 查看当前机器的runner状态: `sudo gitlab-ci-multi-runner list`

#### <font color=green> 安装服务并启动gitlab-runner </font>

* Windows
```
gitlab-runner install
gitlab-runner start
```
* Linux和Mac
```
cd ~
gitlab-runner install
gitlab-runner start
```

### <font color=orange> 在项目根目录配置`.gitlab-ci.yml` </font>

当安装好Gitlab、Gitlab-CI以及注册好Gitlab-runner之后, 我们要做的事情就是在项目根目录中添加 `.gitlab-ci.yml` 文件配置构建任务。当我们添加了 `.gitlab-ci.yml` 了之后，每次提交代码或者合并 master 都会自动运行构建任务了。它使用`YAML`文件进行配置, 缩进时使用空格, 不要使用`tab`键.

* [英文文档](https://docs.gitlab.com/ee/ci/yaml/README.html#configuration-of-your-jobs-with-gitlab-ci-yml)

* Stages: 阶段
`.gitlab-ci.yml`中可以包含很多阶段: 安装依赖、运行测试、编译、部署测试服务器、部署生产服务器等流程。使用`stages`来定义,如下定义了`build`和`test`两个阶段: 
```
stages:
  - build
  - test
```
	* 特点
		* 所有Stages会按照顺序运行，即当一个Stage完成后，下一个Stage才会开始
		* 只有当所有Stages完成后，该构建任务才会成功
		* 如果任何一个Stage失败，那么后面的Stages不会执行，该构建任务失败

* Job: 工作(工作名唯一, 并且不能为关键字)
Jobs 表示构建工作，表示某个Stage里面执行的工作。我们可以在Stages里面定义多个Jobs.
```
build_project:
  stage: build
  script:
    - xcodebuild clean -project ProjectName.xcodeproj -scheme SchemeName | xcpretty
    - xcodebuild test -project ProjectName.xcodeproj -scheme SchemeName -destination 'platform=iOS Simulator,name=iPhone 6s,OS=9.2' | xcpretty -s
  tags:
    - ios_9-2
    - xcode_7-2
    - osx_10-11

archive_project:
  stage: archive
  script:
    - xcodebuild clean archive -archivePath build/ProjectName -scheme SchemeName
    - xcodebuild -exportArchive -exportFormat ipa -archivePath "build/ProjectName.xcarchive" -exportPath "build/ProjectName.ipa" -exportProvisioningProfile "ProvisioningProfileName"
  only:
    - master
  artifacts:
    paths:
      - build/ProjectName.ipa
  tags:
    - ios_9-2
    - xcode_7-2
    - osx_10-11
```
	* Job中的`stage`: 表示所属的阶段
	* Job中的`script`: 定义由Runner执行的脚本
	* Job中的`only`: 表示只在 `分支/标签/触发器` 才会执行job
	* Job中的`except`: 表示除了`分支/标签/触发器`才会执行job
	* Job中的`tag`: 定义用于选择Runner的标签列表
	* Job中的`artifacts`: 定义Job中生成的附件
	* 特点
		* 相同Stage中的Jobs会并行执行
		* 相同Stage中的Jobs都执行成功时，该Stage才会成功
		* 如果任何一个Job失败，那么该Stage失败，即该构建任务失败


## <font color=orange> iOS相关编译、打包命令 </font>

持续集成无论是使用Gitlab-CI还是Jenkins, 对于iOS来说都需要知道底层的打包、编译、上传App Store、蒲公英的一些底层命令.

### <font color=orange> xcodebuild命令 </font>
xcodebuild 是苹果提供的打包项目或者工程的命令，了解该命令最好的方式就是终端使用`man xcodebuild`命令查看, [官方文档](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/xcodebuild.1.html).

##### <font color=green> 使用注意 </font>

* 需要在包含`*.xcodeproj`的目录中执行`xcodebuild`目录, 如果该目录下有多个 projects，那么需要使用`-project`指定需要编译的项目。
* 不指定编译的target时, 默认会编译项目下第一个target.
* 当编译worspace时需要同时指定`-workspace`和`-scheme`参数, scheme参数决定哪个targets会被编译以及编译方式.
* `-list`、`-showBuildSetting`和`-showsdks`可以查看项目或者工程的信息.

##### <font color= green> 常用xcodebuild命令 </font>

* 查看Xcode所有可用的SDKs: 
>	xcodebuild -showsdks

```
iOS SDKs:
	iOS 11.0                      	-sdk iphoneos11.0

iOS Simulator SDKs:
	Simulator - iOS 11.0          	-sdk iphonesimulator11.0

macOS SDKs:
	macOS 10.13                   	-sdk macosx10.13

tvOS SDKs:
	tvOS 11.0                     	-sdk appletvos11.0

tvOS Simulator SDKs:
	Simulator - tvOS 11.0         	-sdk appletvsimulator11.0

watchOS SDKs:
	watchOS 4.0                   	-sdk watchos4.0

watchOS Simulator SDKs:
	Simulator - watchOS 4.0       	-sdk watchsimulator4.0
```

* 查看当前工程或项目build setting配置参数: 
>	xcodebuild -showBuildSettings [-project xxx.xcodeproj | [-workspace xxx.xcworkspace -scheme schemename]]

* 查看当前工程或项目的target、configurations或workspace中的schemes:
>	xcodebuild -list [-project xxx.xcodeproj | -workspace xxx.xcworkspace]

```
Information about project "HelloWorld":
    Targets:
        HelloWorld
        HelloWorldTests
        HelloWorldUITests

    Build Configurations:
        Debug
        Release

    If no build configuration is specified and -scheme is not passed then "Release" is used.

    Schemes:
        HelloWorld
```

* 编译、测试、分析、archive等: 
	* 使用Cocoapods管理项目, 会生成xcworkspace文件, 使用该方式: 
	>	xcodebuild -workspace name.xcworkspace -scheme schemename [[-destination destinationspecifier] …] [-destination-timeout value] [-configuration configurationname] [-sdk [sdkfullpath | sdkname]] [action …] [buildsetting=value …] [-userdefault=value …]
	
	* 对于使用`xxx.xcodeproj`的项目, 使用该方式: 
	>	xcodebuild [-project name.xcodeproj] [[-target targetname] … | -alltargets] [-configuration configurationname] [-sdk [sdkfullpath | sdkname]] [action …] [buildsetting=value …] [-userdefault=value …]
	* 其中`action`对应的有(如果没有指定, 默认值是build): 
		* `build`: 编译
		* `analyze`: 分析
		* `archive`: archive路径
			* 还需要配置
				* `-archivePath`: archive包存储路径
				* `CODE_SIGN_IDENTITY`: 证书名称(Xcode8可以不配置,自动选择, 在钥匙串-证书中查看(类似: `Phone Distribution: Company name Co. Ltd (xxxxxxxx)`))
				* `PROVISIONING_PROFILE`: 描述文件UUID(Xcode8可以不配置, 自动选择)
		* `test`: 测试
		* `installsrc`: 
		* `install`: 安装
		* `clean`: 清理
	* 其中`-configuration`对应的值有(如果没有指定, 默认是Release): 
		* `Debug`: 开发环境
		* `Release`: 生产环境
	* 其中`-sdk`可以通过`xcodebuild -showsdks`获取
	* 其中`-configuration`和`-target`可以通过`xcodebuild -list`获取
* 打ipa包
	* xcodebuild + xcrun的`PackageApplication`打包已经被废弃了, 不推荐使用
	* 可以使用xcodebuild的方式
	>	xcodebuild -exportArchive -archivePath xcarchivepath -exportPath destinationpath -exportOptionsPlist path
		
		* `exportOptionsPlist`参数: 是一个plist文件
```
<?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>teamID</key>
      <string>xxxxxxxx</string> //TeamID
    <key>method</key>
      <string>ad-hoc</string> //ad-hoc打包
    <key>compileBitcode</key> //是否编译bitcode
      <false/>
  </dict>
</plist>
```

