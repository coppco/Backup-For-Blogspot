---
layout: post
title: iOS和JS交互
comments: true
date: 2016-03-15 17:57:55
tags:
    - iOS
    - Swift
    - Objective-C
    - 资料整理
    - JavaScript
    - iOS和JavaScript交互
---
开发App的过程中，通常为了加快开发进度会遇到在App内部加载网页，通常用UIWebView加载。这个自iOS2开始使用的网页加载器一直是开发的心病：加载速度慢，占用内存多，优化困难。如果加载网页多，还可能因为过量占用内存而给系统kill掉。各种优化的方法效果也不那么明显.
而从iOS8.0开始, 苹果推出了新框架Webkit，提供了WKWebView替换UIWebView, 它在速度、内存占用上都比UIWebView好很多. 但是为了兼顾iOS8.0以下的系统还是需要使用UIWebView的.

<font color=orange size=4>这里主要介绍UIWebView和JavaScript的交互.</font>

<!--more-->

>   //首先html中这样写
`<!DOCTYPE html>`
`<html lang="en">`
`<head>`
`<meta charset="UTF-8">`
`<meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0" name="viewport">`
`<title>邀请有礼</title>`
`</head>`
`<body>`
`</br>`
`</br>`
`</br>`
测试OC和JS相互调用
`</br>`
`</br>`
`</br>`
//点击调用弹出框
`<a href="#"onclick="JSCallOC('JS调用原生AlertView')">弹出框</a>`
`</body>`
`<script type="text/javascript">`
//定义一个函数, 调用OC方法
function JSCallOC(title) {
&emsp;&emsp;window.location.href="OBJC:param=" + title;
}
<!--    JS里面定义了一个方法, 有返回值-->
function getShareTxt() {
&emsp;&emsp;return "{\"name\": \"苹果手机\",\"price\": \"¥5000.0\"}";
}
<!--    JS里面定义了一个方法, 有返回值有参数-->
function getShareTxtHasParam(name, price) {
&emsp;&emsp;return "{\"name\": name,\"price\": price}";
}
`</script\>`
`</html>`

</br>
<font color=orange size=5>一、iOS7之前的做法</font>

在iOS7之前和JavaScript的交互: H5调原生方法时发送虚假的请求, 原生通过拦截这个请求来处理事务, 而原生通过UIWebView的`- (nullable NSString *)stringByEvaluatingJavaScriptFromString:(NSString *)script;`方法来调用JavaScript函数.

<font color=orange size=4>1、JavaScript调用原生方法</font>

当我们点击a标签的时候会执行JSCallOC()函数, 传入的参数是当前时间对象, 在JSCallOC()函数中, 改变了当前网页的href, 此时我们在UIWebView的代理方法中`-(BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType`进行处理,返回YES就会不拦截, 返回NO就会拦截网络请求, 处理自己的逻辑即可.例如本例中收到消息弹出alert.


>   -(BOOL)webView:(UIWebView \*)webView shouldStartLoadWithRequest:(NSURLRequest \*)request navigationType:(UIWebViewNavigationType)navigationType {
&emsp;&emsp;NSString \*urlString = request.URL.absoluteString;
&emsp;&emsp;if ([[urlString uppercaseString] containsString:@"OBJC"]) {
&emsp;&emsp;&emsp;&emsp;UIAlertView \*alertView = [[UIAlertView alloc] initWithTitle:@"温馨提示" message:[[urlString URLDecoding] componentsSeparatedByString:@"="].lastObject delegate:nil cancelButtonTitle:@"知道了" otherButtonTitles:nil, nil];
&emsp;&emsp;&emsp;&emsp;[alertView show];
&emsp;&emsp;&emsp;&emsp;return false;
&emsp;&emsp;}
&emsp;&emsp;return true;
}

//URLDecoding方法是unicode解码方法, URL中如果有中文会生成unicode编码, 需要解码, 需要了解的同学可以去看看.[传送门](/2015/12/26/iOS/iOS中URL编码和URL解码/)

<font color=orange size=4>2、OC调用JavaScript函数</font>
OC调JavaScript函数也很简单, 只要在合适的位置使用UIWebView的实例方法`- (nullable NSString *)stringByEvaluatingJavaScriptFromString:(NSString *)script;`即可.
如果不需要触发希望在加载html完成之后执行可以在UIWebView的加载完成的代理方法中执行, 也可以通过按钮点击事件触发.

>   -(IBAction)share:(id)sender {
&emsp;&emsp;NSString *jsMethod = @"JSCallOC('OC调JS方法, JS又调原生AlertView')";
&emsp;&emsp;[self.webView stringByEvaluatingJavaScriptFromString:jsMethod];
}

('OC调JS方法, JS又调原生AlertView')括号里面是调用JS函数传入的参数, 如果是无参数的JS函数,直接这样写即可@"JSCallOC()", 该方法是有返回值的, 如果JS函数没有返回值, 该结果为undefined, 这个方法只能接受string, 可以返回JSON字符串.

调用有返回值有参数的JS函数如下: 

>   NSString \*jsMethod = @"getShareTxtHasParam('小米手机2999元')";
NSString \*string = [self.webView stringByEvaluatingJavaScriptFromString:jsMethod];
UIAlertView \*alertView = [[UIAlertView alloc] initWithTitle:@"收到JS返回值" message:string delegate:nil cancelButtonTitle:@"知道了" otherButtonTitles:nil, nil];
[alertView show];

<font color=orange size=5>二、iOS7之后的做法</font>
在iOS7之后, 苹果推出了JavaScriptCore.framework框架, 极大方便了我们处理JavaScript.它可以把JS对象转为OC里面的对象.

|Objective-C type|JavaScript type|
|:---:|:---:|
|nil         |     undefined|
|NSNull       |        null|
|NSString      |       string|
|NSNumber      |   number, boolean|
|NSDictionary    |   Object object|
|NSArray       |    Array object|
|NSDate       |     Date object|
|NSBlock (1)   |   Function object (1)|
|id (2)     |   Wrapper object (2)|
|Class (3)    | Constructor object (3)|
</br>
<font color=orange size=3>JSContext对象</font>
它是JS运行环境, 可以通过`JSContext *jsContext = [self.webView valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];`获取UIWebView的context上下文环境.
</br>
<font color=orange size=3>JSValue对象</font>
获取到上下文环境后可以通过`JSValue *value = [jsContext evaluateScript:@"document.title"];`方法执行JS代码, 它的返回值是JSValue类型, 它可以转为OC里面的对象、BOOL、NSNumber、NSString、NSArray、NSDictionary、NSDate、int、unsigned int等类型.
如果执行了不存在的JS代码, 肯定会报错, 可以在调用前设置异常捕捉, 
>   [jsContext setExceptionHandler:^(JSContext \*context, JSValue \*exception){
&emsp;&emsp;NSLog(@"%@", exception);
}];
JSValue \*value = [jsContext evaluateScript:@"document.title"];
NSString \*title = value.toString;
//NSDictionary \*dic = value.toDictionary;

</br>
<font color=orange size=3>JS调用原生方法, 假如JS里面定义了一个函数JSCallOC(title), 有一个参数, 希望它调用原生的方法</font>
>   jsContext[@"JSCallOC"] =   ^(NSString \*title) {
&emsp;&emsp;//title是JS传递过来的
&emsp;&emsp;UIAlertView \*alertView = [[UIAlertView alloc] initWithTitle:@"收到JS返回值" message:title delegate:nil cancelButtonTitle:@"知道了" otherButtonTitles:nil, nil];
&emsp;&emsp;[alertView show];
};
//上面也可以通过在block里面执行: `NSArray *args = [JSContext currentArguments];` 获取参数
//有返回值和参数的写法
jsContext[@"add"] =   ^NSInteger(NSInteger a, NSInteger b) {
&emsp;&emsp;return a + b;
};

参考资料:
注意循环引用问题: [JavaScriptCore的使用](http://www.jianshu.com/p/a329cd4a67ee)

<font color=orange size=5>三、使用开源库处理</font>
大家用得比较多的是WebViewJavascriptBridge和EasyJSWebView这两个开源库.
</br>
<font color=orange size=6>后记</font>
从iOS9.0之后, 如果你使用的是https加载的, 那么可以无法获取网络里面的内容, 解决方法[传送门](/2016/05/18/iOS/UIWebView使用Https无法获取里面的内容和数字默认成了电话号码/)

