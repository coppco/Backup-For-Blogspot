---
layout: post
title: 记录Swift3.0的一些变化
comments: true
date: 2016-10-08 09:18:38
tags:
    - Swift
    - iOS
    - Xcode
    - 资料整理
---

最近升级了Xcode8.0, 老项目几百个红,说多了都是泪~~~~,这里记录一下Swift3.0的一些变化,仅供参考!!!
<!--more-->

## 1. 调用函数或者方法时,第一个参数不再省略, 除非使用_来忽略,Swift3.0以前第一个参数是忽略的

## 2. 取消了var参数

## 3. inout参数修饰放到了类型前面

```
func --(a: inout Int) {
    a = a - 1
}
```

## 4. Swift3.0后方法的返回值必须要有接受者,否则会有警告.当然可以使用_来忽略返回值,或者使用@discardableResult来声明可以不接受返回值

```
@discardableResult
func +(a: Int, b: Int) -> Int {
    return a + b
}
```

## 5. Selector的变化, 从最早的字符串到#selector(method(param1:))

## 6. 可选协议方法optional, 必须要在协议名称和方法前面都添加@objc

```
@objc protocol Mappering {
    @objc optional func mapperHelper()
}
```
## 7. 取消了++、--操作符, 去掉了C分隔的for循环

## 8. SDK的一些变化,  对一些API进行了简化
### 1. CGRectZero、CGPoint、CGSize和UIEdgeInsets等的变化, 取消了CGRectMake等函数
```
CGRect.zero
CGPoint.zero
CGSize.zero
UIEdgeInsets.zero
```
### 2. UIApplication的变化
```
UIApplication.shared.isNetworkActivityIndicatorVisible = true
```

### 3. NSUserDefaults的变化
```
UserDefaults.standard.removeObject(forKey: customer_RETURN_DATA_KEY)
UserDefaults.standard.synchronize()
```
### 4. NotificationCenter的变化
```
NotificationCenter.default.post(name: NSNotification.Name(rawValue: customerDidLogoutSuccessNotification), object: nil)
```
### 5. 枚举成员从大写变成了小写
```
.Center---->.center
```

### 6. CAAnimationDelegate 现在开始强制需要遵循协议了

## 9. Xcode的变化
### 1. 新增了command + option + / 的注释, 类似于VVDocument
### 2. 对一些权限需要在Info.plist里面添加描述
```
麦克风权限：Privacy - Microphone Usage Description 是否允许此App使用你的麦克风？
相机权限： Privacy - Camera Usage Description 是否允许此App使用你的相机？
相册权限： Privacy - Photo Library Usage Description 是否允许此App访问你的媒体资料库？
通讯录权限： Privacy - Contacts Usage Description 是否允许此App访问你的通讯录？
蓝牙权限：Privacy - Bluetooth Peripheral Usage Description 是否许允此App使用蓝牙？
语音转文字权限：Privacy - Speech Recognition Usage Description 是否允许此App使用语音识别？
日历权限：Privacy - Calendars Usage Description 是否允许此App使用日历？
定位权限：Privacy - Location When In Use Usage Description 我们需要通过您的地理位置信息获取您周边的相关数据
定位权限: Privacy - Location Always Usage Description 我们需要通过您的地理位置信息获取您周边的相关数据
```

### 3. Xcode8.0 项目默认会有很多系统打印, 可以在Edit Scheme---->Run---->Arguments---->Environment Variables里面添加OS_ACTIVITY_MODE, 值为disable, 注意前面需要勾选! 即可在控制台隐藏这些系统的输出打印.

![如图](http://47.96.147.179/images/iOS/xcode8_logs.png)
