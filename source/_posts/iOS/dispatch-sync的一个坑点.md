---
layout: post
title: dispatch_sync的一个坑点
comments: true
date: 2016-10-13 09:12:08
tags:
    - Swift
    - Objective-C
    - 坑点
    - iOS
---

最近看到一个面试题,题目是这样的: 请说出下面一段代码的打印顺序
```
dispatch_async(dispatch_get_global_queue(0, 0), ^{
    NSLog(@"1");
    dispatch_sync(dispatch_get_main_queue(), ^{
        NSLog(@"2");
    });
    NSLog(@"3");
});
NSLog(@"4");
while (1) {
}
NSLog(@"5");
```
<!--more-->
打印结果如下(有的小伙伴打印会出现1 4): 
![打印结果](http://47.96.147.179/images/iOS/dispatch_sync.png)

相信大家对5不打印都知道, while (1) {} 是一个死循环, 会堵塞线程. 对于2和3没有打印,是因为在主线程队列里面添加一个任务,因为是同步sync,所有要等到添加的任务执行完成才会继续走下去.但是新添加的任务排在队列的末尾,要执行完成必须等到前面的任务执行完成才可以,如此就形成了死锁!!!
