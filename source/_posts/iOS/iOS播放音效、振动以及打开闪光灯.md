---
layout: post
title: iOS播放音效、振动以及打开闪光灯
comments: true
date: 2016-08-29 15:43:25
tags:
    - iOS
    - Swift
    - Objective-C
    - 资料整理
---

iOS中有一些小的功能, 有的可以提高一下用户体验, 有的是一些使用功能.

## 播放音效
* 1、导入框架:`import <AVFoundation/AVFoundation.h>`
* 2、获取SoundID, 并播放
```
- (void)palySoundName:(NSString *)name {
    NSString *audioFile = [[NSBundle mainBundle] pathForResource:name ofType:nil];
    NSURL *fileUrl = [NSURL fileURLWithPath:audioFile];
    
    SystemSoundID soundID = 0;
    AudioServicesCreateSystemSoundID((__bridge CFURLRef)(fileUrl), &soundID);
    //设置回调函数
    AudioServicesAddSystemSoundCompletion(soundID, NULL, NULL, soundCompleteCallback, NULL);
    AudioServicesPlaySystemSound(soundID); // 播放音效
    AudioServicesDisposeSystemSoundID(soundID);//释放
}
void soundCompleteCallback(SystemSoundID soundID, void *clientData){

}
```


## 振动
* 1、导入框架:`AudioToolbox.framework`
* 2、播放系统声音
```
AudioServicesPlaySystemSound(kSystemSoundID_Vibrate); 
```

## 闪光灯
需要导入`#import <AVFoundation/AVFoundation.h>`

```
/** 打开手电筒 */
+ (void)openFlashlight {
    AVCaptureDevice *captureDevice = [AVCaptureDevice defaultDeviceWithMediaType:AVMediaTypeVideo];
    NSError *error = nil;
    if ([captureDevice hasTorch]) {
        BOOL locked = [captureDevice lockForConfiguration:&error];
        if (locked) {
            captureDevice.torchMode = AVCaptureTorchModeOn;
            [captureDevice unlockForConfiguration];
        }
    }
}
/** 关闭手电筒 */
+ (void)closeFlashlight {
    AVCaptureDevice *device = [AVCaptureDevice defaultDeviceWithMediaType:AVMediaTypeVideo];
    if ([device hasTorch]) {
        [device lockForConfiguration:nil];
        [device setTorchMode: AVCaptureTorchModeOff];
        [device unlockForConfiguration];
    }
}

```