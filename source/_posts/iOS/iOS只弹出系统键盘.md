---
layout: post
title: iOS只弹出系统键盘
comments: true
date: 2016-03-23 10:56:35
tags:
    - iOS
    - Objective-C
    - Swift
---
最近App要求键盘只弹出系统键盘, 不允许使用第三方的键盘.
<!--more-->
## 实现方法大致有几种
* 自定义键盘(工作量最大)
自己写一个自定义View作为UITextField或者UITextView的inputView.
* 设置输入框的secureTextEntry为true
passwordTF.secureTextEntry = true;但是当secureTextEntry为false的时候还是会弹出第三方键盘
* 全局设置(iOS 8.0开始开放第三方键盘, 这个方法也是iOS 8.0提供的)
Apple提供了一个接口在Appdelegate里面全局设置


>   -(BOOL)application:(UIApplication *)application shouldAllowExtensionPointIdentifier:(UIApplicationExtensionPointIdentifier)extensionPointIdentifier {
    &#8195;&#8195;if ([extensionPointIdentifier isEqualToString:UIApplicationKeyboardExtensionPointIdentifier]) {
    &#8195;&#8195;&#8195;&#8195;return NO;
    &#8195;&#8195;}
    &#8195;&#8195;return YES;
    }

