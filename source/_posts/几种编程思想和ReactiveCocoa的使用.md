---
layout: post
title: 几种编程思想和ReactiveCocoa的使用
comments: true
date: 2016-07-18 19:02:50
tags:
    - iOS
    - Objective-C
    - 编程思想
    - 三方框架的使用
    - Swift
---
# <font color=orange>常见的编程思想</font>
<!--more-->
* 面向过程
    * 如C语言
* 面向对象
    * 如Objective-C、C++、Java
* 链式编程
    * Masonry:方法返回一个block,该block的参数为需要操作的参数,block的返回值为对象本身.
* <font color=red>响应式编程(Reactive Programming)</font>:不考虑执行的顺序,只考虑结果
    * KVO:KVO本质上是判断对象的属性有没有调用setter方法.
>KVO底层实现:
>* 生成对象的子类
>* 重写setter方法,内部恢复父类方法,通知观察者
>* 修改当前对象的isa指针,使其指向子类.使用objc_setClass(objc, class)来修改isa指针
* <font color=red>函数式编程(Function Programming)</font>:把操作尽量写成一系列嵌套的函数或者方法调用 
    * ReactiveCocoa(RAC):被描述为函数响应式编程(FRP)框架, 它结合了函数式编程思想和响应式编程思想.

# <font color=orange>ReactiveCocoa的使用</font>
<font size=4 color=orange>ReactiveCocoa常见的类</font>
<font size=4><font color=red>RACSignal</font>:信号类,只要有数据改变,信号内部接收到数据,就会马上发出数据.<font color=red>__不同的信号,订阅信号的处理是不同的.__</font></font>
    1. 它本身不具备发送信号的能力,而是交给内部一个订阅者发出.
    2. 默认一个信号是冷信号,也就是值改变了,不会触发,只有订阅了这个信号,才会变为热信号.
    3. 使用RACSignal的subscribeNext方法就能订阅信号.
<font size=4><font color=red>RACSubscriber</font>:表示订阅者的意思,作用是发送数据.它是一个协议,不是一个类.</font>
<font size=4><font color=red>RACDisposable</font>:用于取消订阅和清理资源</font>
__默认执行一次就取消订阅信号了,这个时候可以把订阅者改为强引用,只要订阅者在,就不会自动取消信号的订阅,使用RACDisposable的dispose方法自己主动取消订阅__
```
@property (nonatomic, strong)id<RACSubscriber> subscriber;
//一、创建信号(冷信号)
RACSignal *signal = [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
    _subscriber = subscriber;
    //作用:发送数据
    //调用:当信号被订阅的时候

    //三、发送数据
    [subscriber sendNext:@"RACSignal"];

    return [RACDisposable disposableWithBlock:^{
    //只要信号取消了订阅才会执行block
        NSLog(@"信号取消订阅了");
    }];
}];

//二、订阅信号---->变为热信号
RACDisposable *disposable = [signal subscribeNext:^(id x) {
    //作用:接收到数据
    //调用:当接收到数据的时候
    NSLog(@"%@", x);
}];

//取消订阅
[disposable dispose];
```
<font size=4><font color=red>RACSubject</font>:信号提供者,自己可以当信号,又能订阅信号.</font>  

* 使用RACSubject来代替代理,设置成属性, 当想让其他类执行的时候就发送数据,  其他类就拿到这个属性订阅信号.这个时候就不用写协议和delegate了.

```
//一、创建信号
RACSubject *subject = [RACSubject subject];
//二、订阅信号,可以订阅多次,订阅的时候保存订阅者
[subject subscribeNext:^(id x) {
    NSLog(@"%@", x);
}];
[subject subscribeNext:^(id x) {
    NSLog(@"%@", x);
}];
//三、发送数据,执行上面的block
[subject sendNext:@"RACSubject"];
```
<font size=4><font color=red>RACReplaySubject</font>:RACSubject的子类,重复提供信号类.</font>

* RACReplaySubject可以先发送信号,再订阅信号,而RACSubject不行
* 使用场景:
    * 一个信号每被订阅一次,就把之前的值重复发一次 
    * 可以设置capacity的值来限制缓存value的值,即只保存最新的几个值

```
//一、创建信号
RACReplaySubject *replay = [RACReplaySubject subject];
//二、订阅信号
[replay subscribeNext:^(id x) {
    NSLog(@"%@", x);
}];
//三、发送数据
[replay sendNext:@"RACReplaySubject"];

//四、再次发送数据,订阅信号
[replay sendNext:@"RACReplaySubject-------"];
[replay subscribeNext:^(id x) {
    NSLog(@"==%@", x);
}];

```

<font size=4><font color=red>RACTuple</font>:元组类,类似NSArray,用来包装值.</font>
```
RACTuple *tuple = [RACTuple tupleWithObjects:@"1", @"2", @"3", nil];
NSString *str = tuple[0];
//RACTupleUnpack宏:用来解析RACTuple类型
RACTupleUnpack(NSString *key1, NSString *key2,NSString *key3) = tuple;
NSLog(@"%@-%@-%@", key1, key2, key3);
```
<font size=4><font color=red>RACSequence</font>:RAC集合类,用来代替NSArray和NSDictionary,可以使用它来快速遍历数组和字典.</font>

```
//数组
NSArray *arr = @[@"1",@"2", @"3", @"4"];
//RAC集合
RACSequence *sequence = arr.rac_sequence;
//把集合转成信号
RACSignal *signal = sequence.signal;
//订阅集合信号,内部会自动遍历所有的元素发出去
[signal subscribeNext:^(id x) {
    NSLog(@"%@", x); //打印1,2,3,4
}];

//连起来写
[@[@"1", @"3"].rac_sequence.signal subscribeNext:^(id x) {
    NSLog(@"%@", x);//打印1,3
}];
```

# <font color=orange>ReactiveCocoa的在实际开发中的常见用法</font>
* 代替代理:使用RACSubject代替代理,还可以使用rac_signalForSelector:@selector()方法把调用这个方法转换成信号
* 代替KVO
```
[self rac_observeKeyPath:@"frame" options:(NSKeyValueObservingOptionNew) observer:nil
    block:^(id value, NSDictionary *change, BOOL causedByDealloc, BOOL affectedOnlyLastComponent) {
    //code
}];
//或者
[[self rac_valuesForKeyPath:@"frame" observer:nil] subscribeNext:^(id x) {
    //code
}];
```
* 监听事件
```
UIButton *button = [UIButton buttonWithType:(UIButtonTypeCustom)];
[[button rac_signalForControlEvents:(UIControlEventTouchUpInside)] subscribeNext:^(id x) {
    //code
}];
```

* 代替通知
```
[[[NSNotificationCenter defaultCenter] rac_addObserverForName:UIKeyboardWillShowNotification 
    object:nil] subscribeNext:^(id x) {
    //code
}];
```
* 监听文本框
```
UITextField *textfield = [UITextField new];
[textfield.rac_textSignal subscribeNext:^(id x) {
    //code
}];
```



