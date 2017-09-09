---
layout: post
title: iOS摇一摇的实现
comments: true
date: 2016-05-29 14:29:00
tags:
    - iOS
    - Swift
    - Objective-C
    - 资料整理
---

目前iOS实现摇一摇功能, 大致有两种方案.
* ShakeToEdit
* 加速计

<!--more-->


## <font color=orange>ShakeToEdit</font>
在需要支持摇一摇的控制器中添加下面代码:

```
//设置可以成为第一响应者
-(BOOL)canBecomeFirstResponder  {  
	return YES;  
}  
-（void)viewDidLoad{
    [super viewDidLoad];
    //设置第一响应者
    [self becomeFirstResponder];

}
// 摇一摇开始摇动
- (void)motionBegan:(UIEventSubtype)motion withEvent:(UIEvent *)event {
    [super motionBegan: motion withEvent: event];
}

//摇一摇结束摇动
-(void)motionEnded:(UIEventSubtype)motion withEvent:(UIEvent*)event
{
    if(motion == UIEventSubtypeMotionShake){
    //
    }
}
```

控制摇一摇开关的代码是: true是打开,  false是关闭.

```
[UIApplication sharedApplication].applicationSupportsShakeToEdit = YES;
```

这个方法有时会来大姨妈,  省电模式或者大量重复操作时系统可能会关闭.

## <font color=orange>加速计</font>
目前微信的摇一摇使用是加速计, 使用步骤如下: 
* 1、导入`CoreMotion.framework`框架, 高版本Xcode新建的项目可能不要导入
* 2、引入头文件`#import <CoreMotion/CoreMotion.h>`
* 3、获取实例
```
- (CMMotionManager *)motionManager {
    if (!_motionManager) {
        _motionManager = ({
            CMMotionManager *object = [[CMMotionManager alloc] init];
            //更新频率(单位:秒)
            object.accelerometerUpdateInterval = 1;
            object;
        });
    }
    return _motionManager;
}
```
* 4、采集数据, 有两种方式
	* startAccelerometerUpdatesToQueue(push方式):实时采集所有数据
	* startAccelerometerUpdates(pull方式):在有需要的时候，再主动去采集数据

```
//在viewDidAppear中调用下面方法.
- (void)startAccelerometer {
	 //使用之前需要判断是否能用
    if (self.motionManager.isAccelerometerAvailable) {
        [self.motionManager startAccelerometerUpdatesToQueue:[NSOperationQueue mainQueue] withHandler:^(CMAccelerometerData * _Nullable accelerometerData, NSError * _Nullable error) {
            if (error) {
                [self.motionManager stopAccelerometerUpdates];
            } else {
            	//accelerometerData.acceleration可以获取各个方向上的加速度
             //需要处理的逻辑
            }
        }];
    }
}
```
* 5、优化处理: 监听App进入前台、后台, 开始和停止监听
```
//添加通知
- (void)viewWillAppear:(BOOL)animated {
    [[NSNotificationCenter defaultCenter]addObserver:self selector:@selector(receiveNotification:) name:UIApplicationDidEnterBackgroundNotification object:nil];

    [[NSNotificationCenter defaultCenter]addObserver:self selector:@selector(receiveNotification:) name:UIApplicationWillEnterForegroundNotification object:nil];
}
//移除通知
- (void)viewWillDisappear:(BOOL)animated {
    [[NSNotificationCenter defaultCenter]removeObserver:self name:UIApplicationDidEnterBackgroundNotification object:nil];

    [[NSNotificationCenter defaultCenter]removeObserver:self name:UIApplicationWillEnterForegroundNotification object:nil];
}

-(void)receiveNotification:(NSNotification*)notification {
    if([notification.name isEqualToString:UIApplicationDidEnterBackgroundNotification]) {
        [self.motionManager stopAccelerometerUpdates];
    }else {
        [self startAccelerometer];
    }
}
```