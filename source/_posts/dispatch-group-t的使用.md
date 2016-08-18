---
layout: post
title: 小谈dispatch_group_t的使用
comments: true
date: 2016-08-18 08:59:52
tags:
    - iOS
    - Swift
    - Objective-C
    - GCD
---


在移动端开发中,通常有这种需求:
* 有一个页面里面分有几个模块
* 每个模块都有自己单独的接口,需要分开从服务端请求数据
* 每个模块请求时间又不一致,可能有的接口返回数据是有的,有的数据没有,最后页面加载完成后"残缺不全",影响用户体验
# <font color=orange>dispatch_group_t可以很好的解决这个问题</font>
<!---more--->

dispatch_group_t是 GCD里面的一部分,它可以实现一些任务在特定的任务完成之后才执行
```
let queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0)
let group = dispatch_group_create()
dispatch_group_async(group, queue) {
    print("1")
}
dispatch_group_async(group, queue) {
    print("2")
}
dispatch_group_notify(group, dispatch_get_main_queue()) {
    print("3")
}
```
在上面的例子中 print("1")和print("2")因为是异步的可能先后顺序不一致,但是print("3")肯定是在print("1")和print("2")都执行完成之后才会执行

# <font color=red>Warning</font>
<font color=red>但是上述方法对于dispatch_group_async里面的闭包(OC中叫Block)是异步的网络请求是无效的,意思就是说如果dispatch_group_async里面的闭包(OC中叫Block)是异步的网络请求,dispatch_group_notify执行的时候,前面dispatch_group_async里面的异步网络请求可能还在执行中,导致dispatch_group_notify执行时候需要的数据为nil.</font>

此时下面两个函数(方法)就起到作用了,表示进入到group和离开group,记住它们两个是成对出现的
dispatch_group_enter(group)
dispatch_group_leave(group)

# 示例代码

```
let queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0)
let group = dispatch_group_create()
dispatch_group_async(group, queue) {
    getData1(group)
}
dispatch_group_async(group, queue) {
    getData2(group)
}

//这里还可以设置超时时间,设置为永远DISPATCH_TIME_FOREVER,可能有点问题
dispatch_group_wait(group, 30)
dispatch_group_notify(group, dispatch_get_main_queue()) {
    //code 根据得到的数据判断是否操作
}
//模拟网络请求1
func getData1(group: dispatch_group_t) {
    dispatch_group_enter(group)
    let request = Alamofire.request(.GET, baseUrl, parameters: nil)
    request.responseData { (respone) in
        switch respone.result {
            case .Success:
                //code
                dispatch_group_leave(group)
            case .Failure:
                //code
            dispatch_group_leave(group)
        }
    }
}
//模拟网络请求2
func getData2(group: dispatch_group_t) {
    dispatch_group_enter(group)
    let request = Alamofire.request(.GET, baseUrl, parameters: nil)
    request.responseData { (respone) in
        switch respone.result {
            case .Success:
                //code
                dispatch_group_leave(group)
            case .Failure:
                //code
                dispatch_group_leave(group)
        }
    }
}

```

这样就可以保证 执行dispatch_group_notify的时候, dispatch_group_asyn里面的异步请求都已经执行完成了.
<font color=red>最后重复一次:dispatch_group_enter(group)和dispatch_group_leave(group)是成对的----成对的-----成对的</font>



