---
layout: post
title: '浅谈iOS中#if、#ifdef、#ifndef等预编译使用误区'
comments: true
date: 2016-05-15 15:51:52
tags:
    - iOS
    - Objective-C
    - 坑点
---


最近看了好多Objective-C代码,发现很多人使用了#if、#ifdef、#idndef来解决适配的问题,但是却存在使用的误区,所以有感而发!对于这个可能大家都知道它是预编译,即在编译时进行编译, 它不能替代运行时判断.
<!--more-->
而下面我举个栗子来说明它的一个使用误区:iOS8.0 以后新出的注册远程通知来说吧, iOS8.0 之前是这样的:

><font color=orange>UIRemoteNotificationType myTypes = UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeSound;</font>
><font color=orange>[[UIApplication sharedApplication] registerForRemoteNotificationTypes:myTypes];</font>

而iOS8.0以后,是这样注册通知的:

><font color=orange>UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeBadge|UIUserNotificationTypeSound|UIUserNotificationTypeAlert categories:nil];</font>
<font color=orange>[[UIApplication sharedApplication] registerUserNotificationSettings:settings];</font>
<font color=orange>[[UIApplication sharedApplication] registerForRemoteNotifications];</font>

于是就看到有人使用类似的写法了:
><font color=orange>\#import Availability.h
\#ifdef __IPHONE_8_0 
　　UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert categories:nil];
　　[[UIApplication sharedApplication] registerUserNotificationSettings:settings];
　　[[UIApplication sharedApplication] registerForRemoteNotifications];
\#else
　　UIRemoteNotificationType myTypes = UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeSound;
　　[[UIApplication sharedApplication] registerForRemoteNotificationTypes:myTypes];
\#endif</font>


这里Availability.h是导入的一个系统头文件,里面有一些这样的宏.
    #define __IPHONE_7_1     70100
    #define __IPHONE_8_0     80000
    #define __IPHONE_8_1     80100
    #define __IPHONE_8_2     80200
    #define __IPHONE_8_3     80300
    #define __IPHONE_8_4     80400
    #define __IPHONE_9_0     90000
    #define __IPHONE_9_1     90100
    #define __IPHONE_9_2     90200
而条件编译就是根据不同的条件来选择性的编译某些代码,它和Xcode的编辑SDK版本有关系,在Targets-----Bulid Setting ---- Base SDK 中可以看到选择编译的SDK版本, 一般都是选择和Xcode支持的最新版本一致.而Deployment Target是支持的最低系统版本, 一般都是低于Base SDK的.如果此时选择使用Base SDK iOS 9.2版本编译打包ipa,那么安装在系统版本iOS 8.0以前的手机上就会crash. 原因:使用的是iOS 9.2 SDK编译的, __IPHONE_8_0肯定是定义过的,所以else后面的代码就不会参与编译, 在iOS8.0 以前的版本使用8.0以后的API引起crash.

# <font color=red>总结:从这个栗子可以看出,条件编译只是在选择性的编译代码,它不能替代运行时判断,所以需要做适配的程序员一定要注意,条件编译不能完全替代运行时,一般都是先预编译判断, 然后再运行时判断如:</font>
><font color=orange>#if __IPHONE_8_0
if ([UIDevice currentDevice].systemVersion.floatValue >= 8.0) {
　//code
} else {
　//code
}
#endif</font>


# <font color=red>条件编译适用于一旦编译就不会变化的情况,或者使用条件编译解决使用低版本SDK编译时,高版本SDK里面API报错问题,例如:程序中NSLog很多,但是不希望发布release版本的时候打印,是可以使用条件编译的</font>

>\#ifdef DEBUG
\#define XHJLog(FORMAT, ...) NSLog(@"%@:%d行   \n%@", [[NSString stringWithUTF8String:__FILE__] lastPathComponent], __LINE__, [NSString stringWithFormat:FORMAT, \#\#__VA_ARGS__])
\#else
\#define XHJLog(FORMAT, ...) nil
\#endif

# 记录一下版本适配时一些常用方法
一、通过系统版本来判断
    #define __iOS_VERSION ([[UIDevice currentDevice].systemVersion floatValue])
    if (__iOS_VERSION >= 8.0) {
    //8.0以后版本
    } else {
    //8.0以前版本
    }

二、通过方法能否调用来判断

    if ([self respondsToSelector:@selector(edgesForExtendedLayout)]) {
        //判断对象能否调用方法
    }
    if ([UIAlertController instancesRespondToSelector:@selector(alertControllerWithTitle:message:preferredStyle:)]) {
        //判断类方法能否被调用
    }

三、通过类是否存在来判断

    if (NSClassFromString(@"UIAlertController")) {
        //UIAlertController类存在
    }


## 备注:有什么错误或者遗漏的地方请告知我,coppco@qq.com
