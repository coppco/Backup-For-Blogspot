---
layout: post
title: iOS剪切板相关
comments: true
date: 2016-03-23 13:46:28
tags:
    - iOS
    - Swift
    - Objective-C
    - 资料整理
---

在使用淘宝的时候, 有一个功能就是复制吱口令到淘宝会自动判断剪切板里面的内容, 于是自己整理了一下关于剪切板的知识.
<!--more-->

# <font color=orange>1. 构造方法</font>

    UIPasteboard *pasteboard = [UIPasteboard generalPasteboard];
    pasteboard.string = @"http:www.google.com";

>   //获取系统级别的剪切板
+(UIPasteboard *)generalPasteboard;

>   //获取一个自定义的剪切板 name参数为此剪切板的名称 create参数用于设置当这个剪切板不存在时 是否进行创建
+(nullable UIPasteboard *)pasteboardWithName:(NSString *)pasteboardName create:(BOOL)create;

>   //获取一个应用内可用的剪切板
+(UIPasteboard *)pasteboardWithUniqueName;


# <font color=orange>2. 可以存放的类型</font>
剪切板里面可以放: string、strings、URL、URLs、image、images、color、colors

# <font color=orange>3. iOS原生控件支持剪切板的只有UITextField 、UITextView、UIWebView</font>
如果一个控件能够使用UIPasteBoard需要重写
-(BOOL)canBecomeFirstResponder
-(BOOL)canPerformAction:(SEL)action withSender:(id)sender
这两个方法，前者设置控件能接收到事件(能成为第一响应者)，后者决定这个控件能够使用复制、剪切、选中、全选、粘贴等的哪一种或几种功能,并重载对应的
-(void)copy:(id)sender //复制
-(void)cut:(id)sender  //剪切
-(void)select:(id)sender //选择
-(void)selectAll:(id)sender //全选
-(void)paste:(id)sender //粘贴
等方法
使用UIMenuController来显示菜单

>   //能成为第一响应者
-(BOOL)canBecomeFirstResponder{
&nbsp;&nbsp;return YES;
}

>   //实现复制、剪切、选择、全选、粘贴等方法
-(BOOL)canPerformAction:(SEL)action withSender:(id)sender{
&nbsp;&nbsp;NSArray* array = @[@"copy:",@"cut:",@"select:",@"selectAll:",@"paste:"];
&nbsp;&nbsp;if ([array containsObject:NSStringFromSelector(action)]) {
&nbsp;&nbsp;&nbsp;&nbsp;return YES;
&nbsp;&nbsp;}
&nbsp;&nbsp;return [super canPerformAction:action withSender:sender];
}

>   //实现方法
//复制
-(void)copy:(id)sender {
&nbsp;&nbsp;UIPasteboard \*pasteboard = [UIPasteboard generalPasteboard];
&nbsp;&nbsp;pasteboard.string = @"www.google.com";
} 
//剪切
-(void)cut:(id)sender {
&nbsp;&nbsp;UIPasteboard \*pasteboard = [UIPasteboard generalPasteboard];
&nbsp;&nbsp;pasteboard.string = @"www.google.com";
}
//选择
-(void)select:(id)sender {
&nbsp;&nbsp;//逻辑
}
//全选
-(void)selectAll:(id)sender {
&nbsp;&nbsp;//逻辑
}
//粘贴
-(void)paste:(id)sender {
&nbsp;&nbsp;UIPasteboard \*pasteboard = [UIPasteboard generalPasteboard];
&nbsp;&nbsp;self.textField.text = pasteboard.string;
}

# <font color=orange>4. 显示快捷菜单UIMenuController</font>
UIMenuController：显示一个快捷菜单，用来展示复制、剪贴、粘贴等选择的项。对于自定义控件需要实现上面两个方法
>   UIMenuController *menu = [UIMenuController sharedMenuController];获取menu

>   @property(nonatomic) UIMenuControllerArrowDirection arrowDirection 显示的方向

>   -(void)setTargetRect:(CGRect)targetRect inView:(UIView *)targetView;设置显示范围和视图
>   @property(nonatomic,getter=isMenuVisible) BOOL menuVisible;	    // default is NO
    -(void)setMenuVisible:(BOOL)menuVisible animated:(BOOL)animated;必须手动设置为true才会显示

# <font color=orange>5. 对于剪切板的监听</font>
UIPasteboard系统自带了4个通知:
UIPasteboardChangedNotification //剪切板更改
UIPasteboardChangedTypesAddedKey  //剪切板类型增加
UIPasteboardChangedTypesRemovedKey //剪切板类型移除
UIPasteboardRemovedNotification //剪切板移除

//进入前台通知
UIApplicationDidBecomeActiveNotification

回到开头的问题, 我们可以在需要的页面监听UIApplicationDidBecomeActiveNotification程序已经是活动的通知,  在通知里面获取剪切板里面的内容, 根据内容判断是否需要处理.

>   [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(show:) name:UIApplicationDidBecomeActiveNotification object:nil];
    -(void)show:(id)sender {
    &nbsp;&nbsp;NSString *string = [UIPasteboard generalPasteboard].string;
    &nbsp;&nbsp;//代码逻辑
    }
