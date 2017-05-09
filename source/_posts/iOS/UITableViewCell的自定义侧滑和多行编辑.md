---
layout: post
title: UITableView的侧滑按钮和多行编辑
comments: true
date: 2016-07-19 10:45:34
tags:
    - iOS
    - Swift
    - Objective-C
---

UITableView在iOS开发中是使用最多的控件之一,基本上每个app都会使用到,从设置到一些列表都有它的身影,今天介绍一下UITableView的侧滑按钮和多行编辑.
<!--more-->
今天就介绍一下UITableView的侧滑按钮和多行编辑的使用,使用的都是系统自带的API.
# <font color=orange>__侧滑按钮__</font>
在iOS8.0以前, 实现UITableViewDataSource协议里面下面的方法即可实现Cell向左滑动出现一个按钮,默认样式是UITableViewCellEditingStyleDelete(删除),默认文字是<font color=red size=5>Delete</font>文字.
```
override func tableView(tableView: UITableView, commitEditingStyle 
    editingStyle: UITableViewCellEditingStyle, forRowAtIndexPath indexPath: NSIndexPath) {
    //code  你的代码逻辑
    /*
    numbers -= 1
    guard 0 != numbers else {
        numbers = 8
        return
    }
    self.tableView.reloadData()
    */
}
```
默认文字Delete文字使用UITableViewDelegate协议里面下面的方法修改
```
override func tableView(tableView: UITableView, titleForDeleteConfirmationButtonForRowAtIndexPath 
    indexPath: NSIndexPath) -> String? {
    return "删除"
}
```
样式使用UITableViewDelegate协议里面的下面的方法修改, 该方法默认使用.Delete(Swift中)或者UITableViewCellEditingStyleDelete(Objective-C中), 可以改为其他枚举值,为none的时候不显示.

```
override func tableView(tableView: UITableView,
    editingStyleForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCellEditingStyle {
    return .Delete
}
```

然而在iOS8.0以后出现了一个UITableViewRowAction类,实现UITableViewDataSource协议中下面的方法返回一个或者多个UITableViewRowAction对象即可实现侧滑多个按钮,它的点击方法也是使用block或者闭包来实现,更加方便

```
voerride func tableView(tableView: UITableView,
    editActionsForRowAtIndexPath indexPath: NSIndexPath) -> [UITableViewRowAction]?  {
    let actionDelete = UITableViewRowAction(style: .Default, title: "删除") { 
        (UITableViewRowAction, NSIndexPath) -> Void in
        print("删除了\(indexPath.row)行")
        self.endEditing()
    }
    actionDelete.backgroundColor = UIColor.orangeColor()
    let actionHaHa = UITableViewRowAction(style: .Normal, title: "默默转身") { 
        (UITableViewRowAction, NSIndexPath) -> Void in
        print("默默转身了\(indexPath.row)行")
        self.endEditing()
    }
    actionHaHa.backgroundColor = UIColor.redColor()
    let actionMark = UITableViewRowAction(style: .Normal, title: "标记") { 
        (UITableViewRowAction, NSIndexPath) -> Void in
        print("标记了\(indexPath.row)行")
        self.endEditing()
    }
    actionMark.backgroundEffect = UIBlurEffect(style: .ExtraLight)
    return [actionDelete, actionHaHa, actionMark]
}

func endEditing() {
    self.tableView.setEditing(false, animated: true)
}

```
<font color=red>需要注意的是:当这个方法返回值不为nil的时候,忽略tableView:titleForDeleteConfirmationButtonForRowAtIndexPath:方法, 并且tableView(tableView:, commitEditingStyle editingStyle:,
forRowAtIndexPath indexPath:) 方法不会调用,而是使用UITableViewRowAction的handle替代</font>

# 当iOS8.0以前版本使用多个侧滑按钮可以使用一个三方库SWTableViewCell[传送门](https://github.com/CEWendel/SWTableViewCell),它支持iOS7.0以上


# <font color=orange>__多行编辑__</font>
当下面这个方法返回值是(Swift中)<font color=red>__UITableViewCellEditingStyle(rawValue: 3)__!</font>或者(Objective-C)<font color=red>__UITableViewCellEditingStyleDelete | UITableViewCellEditingStyleInsert__</font>的时候,出现的就是选择与否的图标, 但是此时cell的侧滑按钮是不会有效果的
```
override func tableView(tableView: UITableView,
    editingStyleForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCellEditingStyle {
    return UITableViewCellEditingStyle(rawValue: 3)!
}
```
<font color=red size=4>注意:这里还有一个小技巧,当`Swift中使用retuen UITableViewCellEditingStyle(rawValue: 3)!`  `Objective-C中使用retuen UITableViewCellEditingStyleDelete | UITableViewCellEditingStyleInsert`的时候,当tableView处于编辑状态(editing为true)的时候,左侧出现的是选择与否的按钮,选中cell会出现√图标,不再出现侧滑按钮.而使用除了None时,出现的是对应的删除或者添加图标,点击后会出现侧滑按钮.</font>


所以在这个方法中我加了一个判断:当处于编辑状态的时候,返回UITableViewCellEditingStyle(rawValue: 3)!, 非编辑状态使用.Delete,这样非编辑时候侧滑按钮会出现, 编辑状态可以多选
```
override func tableView(tableView: UITableView, editingStyleForRowAtIndexPath 
    indexPath: NSIndexPath) -> UITableViewCellEditingStyle {
    if true == self.tableView.editing {
        return UITableViewCellEditingStyle(rawValue: 3)!
    }
    return .Delete
}
```
当然多行编辑也可以使用自定义cell,cell里面左侧添加一个按钮,当编辑的时候显示该按钮,然后里面控件整体右移,相比而言这个方法相对简单些.
# 附上[Demo地址](https://github.com/coppco/UITableViewUesedDemo), 使用Xcode7.2 Swift2.1.1 版本编写.
# 效果图:
![效果图](http://oak4eha4y.bkt.clouddn.com/Simulator%20Screen%20Shot%202016%E5%B9%B47%E6%9C%8819%E6%97%A5%20%E4%B8%8B%E5%8D%886.29.07.png)
![效果图](http://oak4eha4y.bkt.clouddn.com/Simulator%20Screen%20Shot%202016%E5%B9%B47%E6%9C%8819%E6%97%A5%20%E4%B8%8B%E5%8D%886.29.18.png)


