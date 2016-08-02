---
layout: post
title: Swift一些资料记录
comments: true
date: 2016-07-22 10:42:22
tags:
    - iOS
    - Swift
    - 资料整理
---

最近开始使用Swift写项目,这里记录一下一些Swift方面的开源库、学习资料以及小技巧.
<!--more-->
# 开源库
<font size=4>[Alamofire](https://github.com/Alamofire/Alamofire):Swift版本的AFNetworking</font>
<font size=4>[SnapKit](https://github.com/SnapKit/SnapKit):Swift版本的Masonry,AutoLayout自动布局</font>

* __<font color= red>简单使用:使用前先把view添加到父控件, 用法基本和Masonry相同</font>__
```
self.addSubview(self.titleL)
titleL.snp_makeConstraints(closure: { (make) -> Void in
make.left.equalTo(30)
make.right.equalTo(-30)
//设置优先级priorityLow()
make.bottom.equalTo(imageV.snp_top).offset(-50).priorityLow()
make.top.greaterThanOrEqualTo(self).offset(84).priorityHigh()
})
```
* __<font color= red>除了equalTo()还有lessThanOrEqualTo()和greaterThanOrEqualTo()</font>__
* __<font color= red>设置优先级  priorityLow()等</font>__
* __<font color= red>比例 multipliedBy()和dividedBy(), 此时equal()里面的参数必须是自己本身的控件属性如:snp_width</font>__
* __<font color= red>snp_updateConstraints() 更新约束</font>__
* __<font color= red>snp_remakeConstraints 重新设置约束</font>__
* __<font color= red>snp_removeConstraints() 移除约束</font>__


<font size=4>[SwiftyJSON](https://github.com/SwiftyJSON/SwiftyJSON):JSON的处理</font>


# 小技巧
__<font size=4 color=red>1. 使用Swift时,Release版本不希望打印输出,而Debug版本正常打印</font>__
* __在Target--->Build Setting--->搜索Other Swift Flags--->在Debug 里面添加-D DEBUG__
![添加步骤](http://oak4eha4y.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-07-23%20%E4%B8%8A%E5%8D%8811.06.10.png)
* 这样在项目中就可以使用下面的代码进行添加编译了
```
#if DEBUG
#else
#endif
```

附上我使用的自定义函数供大家参考:先按照上述步骤添加-D DEBUG,然后是定义一个全局函数HJLog

```
//Swift2.2后使用#line #file #function #column替换__LINE__等
//这里文件名file和行数line使用参数,并且给它默认值__FILE__等,这样在哪里调用就是哪个文件名和行数.而不能直接在函数中使用__FILE__等,不然打印出来都是HJLog所在的文件名和行数
func HJLog(items: Any..., file:String = __FILE__, line:Int = __LINE__, function:String = __FUNCTION__) {
    #if DEBUG
        //添加时间、文件名和行数
        var fileString = "======时间:\(NSDate())" + " 文件名:\((file as NSString).lastPathComponent)" + " 函数名:\(function)" + " 行数:\(line)======\n"
        for item in items {
            fileString += (String(item) + " ")
        }
        print(fileString)
    #else
    #endif
}
```

效果图:![自定义打印](http://oak4eha4y.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-07-23%20%E4%B8%8A%E5%8D%8811.16.34.png)
当切换到Release版本下,HJLog就不再会打印了.一劳永逸~~

__<font size=4 color=red>2. 在Swift中获取实例的类型,除了使用Objective-C中的方法,还可以使用</font>__
`方法1:使用类名.self`
`方法2:使用类名.classForCoder(),只要是NSObject子类都可以使用`
`方法3:使用实例.classForCoder, 只要是NSObject对象都可以使用`
`方法4:使用实例.dynamicType, 但是在Swift中dynamicType是一个关键字`

```
HJLog(HJTabBarController.self)                   //方法1:使用类名.self
HJLog(HJTabBarController.classForCoder())       //方法2:使用类名.classForCoder()
HJLog(self.classForCoder)                       //方法3:使用实例.classForCoder
HJLog(self.dynamicType)                        //方法4:使用实例.dynamicType
```
效果图:![获取类型](http://oak4eha4y.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-08-02%20%E4%B8%8A%E5%8D%8811.24.07.png)


__<font size=4 color=red>3. Swift闭包中解决循环引用问题</font>__
`方法1:使用OC类似的方法  weak var weakself = self`
`方法2:使用unowned,此时对象不能为nil`
`方法3:使用weak,此时对象可以为nil`
使用方法2和方法3的时候,在闭包的参数列表前面使用[unowned self, weak button],多个对象使用逗号,隔开
```
window?.rootViewController = HJGuideController(closure: {[unowned self, weak window] () -> Void in
    //weak var weakself = self  //方法1
    self.window?.rootViewController = HJTabBarController()
    self.window?.makeKeyAndVisible()
})
```


