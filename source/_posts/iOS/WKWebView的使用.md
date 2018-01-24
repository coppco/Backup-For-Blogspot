---
layout: post
title: WKWebView的使用
comments: true
date: 2016-02-10 18:51:37
toc: true
tags:
    - iOS
    - Objective-C
    - Swift
---

WKWebView是iOS8.0以后推出的新框架`WebKit`中用于替代UIWebView的新组件, 它在加载速度和内存使用上面都比UIWebView强太多.

<!--more-->

## <font color=orange> WKWebView </font>

### <font color=green> 加载网络或者本地HTML </font>

```oc
- (nullable WKNavigation *)loadRequest:(NSURLRequest *)request;
- (nullable WKNavigation *)loadHTMLString:(NSString *)string baseURL:(nullable NSURL *)baseURL;
```
### <font color=green> 加载本地JavaScript代码 </font>



### <font color=green> WKNavigationDelegate协议 </font>

#### <font color=green> 加载过程代理方法 </font>

```oc
// 页面开始加载时调用
- (void)webView:(WKWebView *)webView didStartProvisionalNavigation:(WKNavigation *)navigation;
// 当内容开始返回时调用
- (void)webView:(WKWebView *)webView didCommitNavigation:(WKNavigation *)navigation;
// 页面加载完成之后调用
- (void)webView:(WKWebView *)webView didFinishNavigation:(WKNavigation *)navigation;
// 页面加载失败时调用
- (void)webView:(WKWebView *)webView didFailProvisionalNavigation:(WKNavigation *)navigation;
```

#### <font color=green> 跳转代理方法 </font>

```oc
// 接收到服务器跳转请求之后调用
- (void)webView:(WKWebView *)webView didReceiveServerRedirectForProvisionalNavigation:(WKNavigation *)navigation;
// 在收到响应后，决定是否跳转
- (void)webView:(WKWebView *)webView decidePolicyForNavigationResponse:(WKNavigationResponse *)navigationResponse decisionHandler:(void (^)(WKNavigationResponsePolicy))decisionHandler;
// 在发送请求之前，决定是否跳转
- (void)webView:(WKWebView *)webView decidePolicyForNavigationAction:(WKNavigationAction *)navigationAction decisionHandler:(void (^)(WKNavigationActionPolicy))decisionHandler {
    //允许跳转
    decisionHandler(WKNavigationResponsePolicyAllow);
    //不允许跳转
    //decisionHandler(WKNavigationResponsePolicyCancel);
}
```
### <font color=green> WKUIDelegate协议 </font>
这个协议主要用于页面上面的提示框的处理以及允许不安全的链接导航.
```oc
//在JS端调用alert函数时，会触发此代理方法。
- (void)webView:(WKWebView *)webView runJavaScriptAlertPanelWithMessage:(NSString *)message initiatedByFrame:(WKFrameInfo *)frame completionHandler:(void (^)(void))completionHandler;

//JS端调用confirm函数时，会触发此方法
- (void)webView:(WKWebView *)webView runJavaScriptConfirmPanelWithMessage:(NSString *)message initiatedByFrame:(WKFrameInfo *)frame completionHandler:(void (^)(BOOL result))completionHandler;

//JS端调用prompt函数时，会触发此方法
- (void)webView:(WKWebView *)webView runJavaScriptTextInputPanelWithPrompt:(NSString *)prompt defaultText:(nullable NSString *)defaultText initiatedByFrame:(WKFrameInfo *)frame completionHandler:(void (^)(NSString * _Nullable result))completionHandler;

//处理不安全的链接允许导航, 默认不处理不允许导航
- (nullable WKWebView *)webView:(WKWebView *)webView createWebViewWithConfiguration:(WKWebViewConfiguration *)configuration forNavigationAction:(WKNavigationAction *)navigationAction windowFeatures:(WKWindowFeatures *)windowFeatures;
```

### <font color=green> WKWebView加载进度和标题 </font>
WKWebView提供了一个属性estimatedProgress可以通过KVO实时显示网页加载进度, 而UIWebView是没有的, 以前的通用做法都是做一个虚假的进度条.还有一个属性title, 网页的标题也可以获取了.
```
@property (nullable, nonatomic, readonly, copy) NSString *title;

@property (nonatomic, readonly) double estimatedProgress;

//最后注意需要在dealloc中移除观察者
[self.wkWebView addObserver:self forKeyPath:@"estimatedProgress" options:NSKeyValueObservingOptionNew context:nil];

//KVO方法
-(void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSString *,id> *)change context:(void *)context
{
    if (object == self.wkWebView && [keyPath isEqualToString:@"estimatedProgress"] ) {
        CGFloat newProgress = [[change objectForKey:NSKeyValueChangeNewKey] floatValue];
        NSLog(@"%f",newProgress); 
    }
}

```

### <font color=green> 与iOS的交互 </font>
#### <font color=green> iOS调用JavaScript中方法 </font>

WKWebView提供了下面的方法, 弥补了UIWebView类似方法返回值类型只能为NSString以及无法捕捉错误的缺陷.
```oc
- (void)evaluateJavaScript:(NSString *)javaScriptString completionHandler:(void (^ _Nullable)(_Nullable id result, NSError * _Nullable error))completionHandler;

//例如除了可以通过self.webView.title获取标题, 下面方法也可以
[self.webView evaluateJavaScript:@"document.title" completionHandler:^(id _Nullable title, NSError * _Nullable error) {
    //code
}];
```

#### <font color=green> JavaScript调用iOS中方法 </font>

##### <font color=green> URL拦截</font>
我们依然可以通过自定义Scheme拦截URL, 根据URL调用相应的方法. 在WKWebView中需要在下面的代理方法中拦截. 例如在`<a href="#toLogin">登录</a>`中 
```
//WKWebView
- (void)webView:(WKWebView *)webView decidePolicyForNavigationAction:(WKNavigationAction *)navigationAction decisionHandler:(void (^)(WKNavigationActionPolicy))decisionHandler;
```

##### <font color=green> 通过WKUserContentController实现 </font>
//注册一个webViewHandles方法 (这样会造成内存泄露)
[wkWebView.configuration.userContentController addScriptMessageHandler:self name:@"webViewHandles"];

需要代理遵循WKScriptMessageHandler并实现下面方法: 
```
- (void)userContentController:(WKUserContentController *)userContentController didReceiveScriptMessage:(WKScriptMessage *)message{
   //message.body,message.frameInfo
}
```
在h5端通过下面方式调用 body是传递的参数.  `window.webkit.messageHandlers.webViewHandles.postMessage({body: 'hello world!'});`

#### <font color=red>正确的做法: 新建一个代理类来处理</font>
.h文件中  
```
#import <Foundation/Foundation.h>
#import <WebKit/WKScriptMessageHandler.h>

@interface HJWeakScriptMessageDelegate : NSObject<WKScriptMessageHandler>

@property (nonatomic, weak) id<WKScriptMessageHandler> scriptDelegate;
- (instancetype)initWithDelegate:(id<WKScriptMessageHandler>)scriptDelegate;

+ (instancetype)new NS_UNAVAILABLE;
- (instancetype)init NS_UNAVAILABLE;
- (instancetype)initWithFrame:(CGRect)frame NS_UNAVAILABLE;
- (instancetype)initWithCoder:(NSCoder *)aDecoder NS_UNAVAILABLE;
@end
```

.m中
```
#import "HJWeakScriptMessageDelegate.h"

@implementation HJWeakScriptMessageDelegate
- (instancetype)initWithDelegate:(id<WKScriptMessageHandler>)scriptDelegate {
    self = [super init];
    if (self) {
        _scriptDelegate = scriptDelegate;
    }
    return self;
}
- (void)userContentController:(WKUserContentController *)userContentController didReceiveScriptMessage:(WKScriptMessage *)message {
    if (self.scriptDelegate && [self.scriptDelegate respondsToSelector:@selector(userContentController:didReceiveScriptMessage:)]) {
        [self.scriptDelegate userContentController:userContentController didReceiveScriptMessage:message];        
    }
}
@end
```
修改上面代码为: 
[WKWebView.configuration.userContentController addScriptMessageHandler:[[HJWeakScriptMessageDelegate alloc] initWithDelegate:self] name:@"webViewHandles"];