---
layout: post
title: UIWebView使用Https无法获取里面的内容和数字默认成了电话号码
comments: true
date: 2016-05-18 11:52:44
tags:
    - iOS
    - Swift
    - Objective-C
    - 资料整理
---
从iOS9.0开始, 苹果默认网络请求使用https, 对于使用http网络请求的App可以通过Info.plist添加字段来解决.

    <key>NSAppTransportSecurity</key>
    <dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
    </dict>

当我们使用UIWebView加载一个网页, 而网页是通过https加载的, 那么我们使用`- (nullable NSString *)stringByEvaluatingJavaScriptFromString:(NSString *)script;`方法或者`JSValue *value = [jsContext evaluateScript:@"document.title"];`执行JS代码, 并没有返回值, 这是因为没有信任服务器.可以为NSURLRequest添加一个Category. <font color=red>但是网上有人说这个是私有API, 有可能被拒, 我项目审核时并没有被拒.</font>

>   @implementation NSURLRequest(ViewController)
+(BOOL)allowsAnyHTTPSCertificateForHost:(NSString *)host {
//containsString:方法只能在iOS8.0以后使用, 需要适配iOS7也可使用Category重写该方法.
&emsp;&emsp;if ([host containsString:@"your server host"]) {
&emsp;&emsp;&emsp;&emsp;return true;
&emsp;&emsp;}
&emsp;&emsp;return false;
}


## <font color=orange>解决UIWebView里面数字默认成了电话号码, 可以直接拨打出去的问题.</font>
可以设置下面两个属性来控制
>   @property (nonatomic) BOOL detectsPhoneNumbers NS_DEPRECATED_IOS(2_0, 3_0);
@property (nonatomic) UIDataDetectorTypes dataDetectorTypes NS_AVAILABLE_IOS(3_0);



