---
layout: post
title: iOS应用内跳转和一些基本功能
comments: true
date: 2015-11-20 14:09:59
tags:
    - iOS
    - Swift
    - Objective-C
    - 资料整理
    - iOS应用跳转
---

App内可以实现打电话、发短信、发邮件、跳转Safari上网等, 可以跳转到手机内的设置等界面.

<!--more-->

# <font color=orange size=4>打电话</font>
* 使用UIWebView打电话

>   UIWebView \*webView =[[UIWebView alloc] init];  
//NSURL \*tel =[NSURL URLWithString:@"tel:10086"]; 
NSURL \*tel =[NSURL URLWithString:@"telprompt:10086"]; 
[webView loadRequest:[NSURLRequest requestWithURL:tel]];  
//记得添加到view上  
[self.view addSubview:webView];

* 使用[UIApplication sharedApplication]的`- (BOOL)openURL:(NSURL*)url `方法

>   //NSURL \*tel =[NSURL URLWithString:@"tel:10086"]; 
NSURL \*tel =[NSURL URLWithString:@"telprompt:10086"]; 
//可以先判断一下,能打开才去执行
if ([[UIApplication sharedApplication] canOpenURL:url]) {
&emsp;&emsp;[[UIApplication sharedApplication] openURL:url];
}

<font color=red>注意: tel和telprompt的区别: tel直接会拨打电话, 而telprompt会弹出一个框, 让用户选择是否拨打电话</font>

# <font color=orange size=4>发短信</font>

>   //NSURL \*sms =[NSURL URLWithString:@"sms:xxxxxx"]; 
//可以先判断一下,能打开才去执行
if ([[UIApplication sharedApplication] canOpenURL:sms]) {
&emsp;&emsp;[[UIApplication sharedApplication] openURL:sms];
}

注意: 上面的方法需要跳转到系统短信内发短信, 不能指定发送的内容, 在iOS4.0以后提供了MFMessageComposeViewController和MFMessageComposeViewControllerDelegate可以实现在App内部发短信, 需要导入MessageUI/MessageUI.h.

>   //MFMessageComposeViewControllerDelegate协议方法
-(void)messageComposeViewController:(MFMessageComposeViewController *)controller didFinishWithResult:(MessageComposeResult)result{  
&emsp;&emsp;[controller dismissModalViewControllerAnimated:NO];//关键的一句   不能为YES  
&emsp;&emsp;switch ( result ) {  
&emsp;&emsp;&emsp;&emsp;case MessageComposeResultCancelled:  //取消
&emsp;&emsp;&emsp;&emsp;break;  
&emsp;&emsp;case MessageComposeResultFailed:// 失败
&emsp;&emsp;&emsp;&emsp;break;  
&emsp;&emsp;case MessageComposeResultSent:  //成功
&emsp;&emsp;&emsp;&emsp;break;  
&emsp;&emsp;default:  MFMailComposeResultSaved
&emsp;&emsp;&emsp;&emsp;break;   
&emsp;&emsp;}  
}  


# <font color=orange size=4>发邮件</font>

>   //NSURL \*email =[NSURL URLWithString:@"mailto:xxxxxx"]; 
//可以先判断一下,能打开才去执行
if ([[UIApplication sharedApplication] canOpenURL:email]) {
&emsp;&emsp;[[UIApplication sharedApplication] openURL:email];
}

注意: 和发短信一样, 上面的方法需要跳转到系统邮件内发邮件, 不能指定发送的内容, 在iOS4.0以后提供了MFMailComposeViewController和MFMailComposeViewControllerDelegate可以实现在App内部发邮件, 需要导入MessageUI/MessageUI.h.

>   //MFMailComposeViewControllerDelegate协议方法
-(void)mailComposeController:(MFMailComposeViewController \*)controller didFinishWithResult:(MFMailComposeResult)result error:(nullable NSError *)error {  
&emsp;&emsp;[controller dismissModalViewControllerAnimated:NO];//关键的一句   不能为YES  
&emsp;&emsp;switch ( result ) {  
&emsp;&emsp;&emsp;&emsp;case MFMailComposeResultCancelled:  //取消
&emsp;&emsp;&emsp;&emsp;break;  
&emsp;&emsp;case MFMailComposeResultFailed:// 失败
&emsp;&emsp;&emsp;&emsp;break;  
&emsp;&emsp;case MFMailComposeResultSent:  //成功
&emsp;&emsp;&emsp;&emsp;break;  
&emsp;&emsp;case MFMailComposeResultSaved: //保存
&emsp;&emsp;&emsp;&emsp;break;   
&emsp;&emsp;}  
}  


# <font color=orange size=4>使用Safari上网</font>

>   //NSURL \*boke =[NSURL URLWithString:@"https:coppco.github.io"]; 
//可以先判断一下,能打开才去执行
if ([[UIApplication sharedApplication] canOpenURL:boke]) {
&emsp;&emsp;[[UIApplication sharedApplication] openURL:boke];
}

# <font color=orange size=4>跳转到设置页面</font>
当用户选择禁用某项功能时, 如定位、相机、相册、通知等时, 我们希望引导用户去打开, 直接跳转到系统设置页面.从iOS8开始, 系统提供了<font color=red>UIApplicationOpenSettingsURLString</font>, 它可以帮助我们跳转到系统设置页面, 使用时注意需要判断是否是在iOS8.0以后的版本.

>   if ([UIDevice currentDevice].systemVersion.floatValue >= 8.0) {
&emsp;&emsp;NSURL *setting = [NSURL URLWithString:UIApplicationOpenSettingsURLString];
&emsp;&emsp;if ([[UIApplication sharedApplication] canOpenURL:setting]) {
&emsp;&emsp;&emsp;&emsp;[[UIApplication sharedApplication] openURL:setting];
&emsp;&emsp;}
}

注意: 如App还没请求任何权限, 会跳转到系统设置界面, 如果有请求过权限则会跳转到App的设置页面.

# <font color=orange size=4>跳转到AppStore中详情页面</font>
首先获取App在iTunes Connect里面的Apple ID号码, 如支付宝的iD为333206289
那么你的App跳转地址就是:
* https://itunes.apple.com/cn/app/id333206289?mt=8
    * 使用openURL可以打开
* itms-apps://itunes.apple.com/cn/app/id333206289?mt=8
    * 使用openURL可以打开, UIWebView和WKWebView的load方法都可以跳转
* itms://itunes.apple.com/cn/app/id333206289?mt=8
    * 移动端可以跳转, 但是是网页版的Appstore
    * PC端是打开只是能打开电脑本身的ituns

# <font color=orange size=4>去AppStore内评论</font>
* 方法一: 跳转到AppStore评分功能

>   NSString \*evaluateString = @"http://itunes.apple.com/WebObjects/MZStore.woa/wa/viewContentsUserReviews?id=333206289&pageNumber=0&sortOrdering=2&type=Purple+Software&mt=8";
if ([[UIApplication sharedApplication] canOpenURL:evaluateString]) {
&emsp;&emsp;[[UIApplication sharedApplication] openURL:evaluateString];
}

* 方法二: App内实现评论
iOS6.0以后苹果推出了StoreKit.framework框架, 可以显示应用内评价.

>   //初始化控制器
SKStoreProductViewController \*storeProductViewContorller = [[SKStoreProductViewController alloc] init];
//设置代理请求为当前控制器本身
storeProductViewContorller.delegate = self;
//加载一个新的视图展示
[storeProductViewContorller loadProductWithParameters:  @{SKStoreProductParameterITunesItemIdentifier : @"333206289"} completionBlock:^(BOOL result, NSError \*error) {
&emsp;&emsp;if(!error) {
&emsp;&emsp;}
}];
//模态弹出AppStore应用界面
[self presentViewController:storeProductViewContorller animated:true completion:nil];
//实现代理方法
-(void)productViewControllerDidFinish:(SKStoreProductViewController \*)viewController{
&emsp;&emsp;[viewController dismissViewControllerAnimated:YES completion:nil];
}

# <font color=orange size=4>与指定QQ聊天</font>
这个在不使用QQ的SDK前提下, 只要手机安装了QQ即可

>   NSString *qq = @"mqq://im/chat?chat_type=wpa&uin=120903100&version=1&src_type=web";
//如果只是QQ的主界面的话直接使用@"mqq:"即可
if ([[UIApplication sharedApplication] canOpenURL:qq]) {
&emsp;&emsp;[[UIApplication sharedApplication] openURL:qq];
}


需要注意的是: 如果该QQ不是你的好友, 不能发送消息成功, 一般客服的QQ需要开启QQ推广功能.[QQ推广](http://shang.qq.com/v3/widget.html)
还需要在Info.plist里面添加允许Schemes的名单, 自己也需要设置URL Schemes(Targets -----> 项目名称 -----> Info -----> URL Types -----> + 添加一个即可), 不然左上角没有返回按钮.

    //Info.plist里面需要添加
    <key>LSApplicationQueriesSchemes</key>
    <array>
    <string>mqq</string>
    </array>

# <font color=orange size=4>跳转到微信</font>
* 打开微信, 不用集成微信SDK
    * 直接使用openURL:方法打开 @"weixin:", 需要和QQ一样添加Schemes白名单和设置自己的URL Schemes.
* 跳转到微信公众号
    * 需要集成微信SDK, 注册微信公众号, 注册微信开放平台

>   JumpToBizProfileReq \*req = [[JumpToBizProfileReq alloc]init];
req.profileType = WXBizProfileType_Normal;
//enum WXBizProfileType{
&emsp;&emsp;//WXBizProfileType_Normal = 0, /\*普通公众号添加这一段代码 \*/
&emsp;&emsp;//WXBizProfileType_Device = 1, /\*硬件公众号添加这一段代码\*/
//};
req.username = @"gh_4e224b86bcd2"; /\*公众号原始ID\*/
//req.extMsg = @"extMsg"; /\*若为服务号或订阅号则本字段为空，硬件号则填写相关的硬件二维码串\*/
BOOL result = [WXApi sendReq:req];





# <font color=orange size=4>其他手机页面</font>

* 一级界面
    * @"Prefs:root=WIFI",//打开WiFi
    * @"Prefs:root=Bluetooth", //打开蓝牙设置页 
    * @"Prefs:root=NOTIFICATIONS_ID",//通知设置
    * @"Prefs:root=General",  //通用        
    * @"Prefs:root=DISPLAY&BRIGHTNESS",//显示与亮度
    * @"Prefs:root=Wallpaper",//墙纸
    * @"Prefs:root=Sounds",//声音
    * @"Prefs:root=Privacy",//隐私
    * @"Prefs:root=STORE",//存储
    * @"Prefs:root=NOTES",//备忘录
    * @"Prefs:root=SAFARI",//Safari
    * @"Prefs:root=MUSIC",//音乐
    * @"Prefs:root=Photos",//照片与相机
    * @"Prefs:root=CASTLE"//iCloud
    * @"Prefs:root=FACETIME",//FaceTime
    * @"Prefs:root=LOCATION",//定位服务
    * @"Prefs:root=Phone",//电话
* 通用设置页面
    * @"Prefs:root=General&path=About",//关于本机      
    * @"Prefs:root=General&path=SOFTWARE_UPDATE_LINK",//软件更新
    * @"Prefs:root=General&path=DATE_AND_TIME",//日期和时间
    * @"Prefs:root=General&path=ACCESSIBILITY",//辅助功能
    * @"Prefs:root=General&path=Keyboard",//键盘
    * @"Prefs:root=General&path=VPN",//VPN设置
    * @"Prefs:root=General&path=AUTOLOCK",//自动锁屏
    * @"Prefs:root=General&path=INTERNATIONAL",//语言与地区
    * @"Prefs:root=General&path=ManagedConfigurationList",//描述文件
* 隐私设置页面
    * @"Prefs:root=Privacy&path=CAMERA",//设置相机使用权限
    * @"Prefs:root=Privacy&path=PHOTOS"//设置照片使用权限
    * @"Prefs:root=Privacy&path=LOCATION_SERVICES"

# <font color=orange>后记, iOS10出来后, 写法变化了</font>
上面这些在iOS8 和 iOS9 是没问题的, 但是在iOS10 需要在前面添加App-, 如
@"App-Prefs:root=WIFI",//打开WiFi, 但是定位的变化了.
* 设置定位是否开启, 比较特殊
    * @"Prefs:root=Privacy&path=LOCATION_SERVICES" //iOS10以前
    * @"App-Prefs:root=Privacy&path=LOCATION" //iOS 10
