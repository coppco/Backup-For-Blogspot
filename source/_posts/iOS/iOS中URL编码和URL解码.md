---
layout: post
title: iOS中URL编码和URL解码
comments: true
date: 2015-12-26 11:48:12
tags:
    - iOS
    - Swift
    - Objective-C
    - 资料整理
---

在iOS开发中网络请求时, 如果一个URL中包含中文, 如果不做处理, 这个请求是无法处理的, 甚至引起崩溃, 而和H5交互时也有可能获取到类似这样的URL字符串: `%e4%b8%ad%e6%96%87`. 此时就会用到URL编码和解码.

<!--more-->

在iOS中, 我们可以为NSString新建一个Category, 为NSString扩展两个方法: 编码方法和解码方法, 方便使用.

//.h文件中

>   @interface NSString (Unicode)
/\*\*URL编码\*/
-(NSString \*)URLEncoding;
/\*\*URL解码\*/
-(NSString \*)URLDecoding;
@end

//.m文件中

>   @implementation NSString (Unicode)
-(NSString \*)URLEncoding {
&emsp;&emsp;if ([UIDevice currentDevice].systemVersion.floatValue >= 9.0) {
&emsp;&emsp;&emsp;&emsp;return [self stringByAddingPercentEncodingWithAllowedCharacters:[NSCharacterSet URLQueryAllowedCharacterSet]];
&emsp;&emsp;} else {
&emsp;&emsp;&emsp;&emsp;return [self stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
&emsp;&emsp;}
}
-(NSString \*)URLDecoding {
&emsp;&emsp;if ([UIDevice currentDevice].systemVersion.floatValue >= 9.0) {
&emsp;&emsp;&emsp;&emsp;return [self stringByRemovingPercentEncoding];
&emsp;&emsp;} else {
&emsp;&emsp;&emsp;&emsp;return [self stringByReplacingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
&emsp;&emsp;}
}
@end
