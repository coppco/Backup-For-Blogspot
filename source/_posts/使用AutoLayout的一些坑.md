---
layout: post
title: 使用AutoLayout的一些坑
comments: true
date: 2016-08-14 14:55:43
tags:
    - iOS
    - AutoLayout
    - Objective-C
    - Swift
    - Masonry
    - SnapKit
    - 坑点
    - 三方框架的使用
---

最近使用SnapKit来给UITableViewCell添加约束, 以实现自动布局, 虽然最终结果正确,但是控制台有恼人的警告, 遂网上搜索了一般最终解决了!
```
NSLayoutConstraint:0x7fb743f46730 'UIView-Encapsulated-Layout-Height' V:[UITableViewCellContentView:0x7fb743f4bfe0(353.333)]
    Will attempt to recover by breaking constraint 
SnapKit.LayoutConstraint:0x7fb743d03d00@/View/View.swift#121 PictureView:0x7fb743f74050.height == 374.0
```
<!--more-->

# <font color=orange>UITableView和UICollectionView会默认添加两个约束就是UIView-Encapsulated-Layout-Width 和UIView-Encapsulated-Layout-Hight保证大小适中。</font>
# 我在使用的时候,cell中的一个图片如果设置为固定的size就不会出现上述警告, 但是一旦根据图片的大小设置约束就会出现上述警告,这是因为系统设置的约束和我们动态设置的约束有冲突导致的.

解决方法: 使自己创建的约束优先级高点或者低点
```
bottomBar.snp_makeConstraints(closure: { (make) -> Void in
    //主要是设置bottomBar的bottom和contentView的bottom相同来决定cell的高度, 同时设置优先级为高或者低,
    来解决和系统默认添加的约束冲突问题.
    make.left.right.bottom.equalTo(self.contentView).priorityHigh() 
    make.height.equalTo(44)
    make.top.greaterThanOrEqualTo(self.contentL.snp_bottom).offset(paddingV)
    make.top.greaterThanOrEqualTo(self.pictureV.snp_bottom).offset(paddingV)
    make.top.greaterThanOrEqualTo(self.videoV.snp_bottom).offset(paddingV)
})
```

# 下面介绍一些调试约束问题的方法
>     Make a symbolic breakpoint at UIViewAlertForUnsatisfiableConstraints to catch this in the debugger    


* <font size=3>command + 7 打开断点调试器</font>
* <font size=3>点击左下方的+号,选择 Add Symbolic Breakpoint...</font>
* <font size=3>在symbol里面添加 UIViewAlertForUnsatisfiableConstraints</font>
如图:![添加symbolic breakpoint](http://oak4eha4y.bkt.clouddn.com/%E6%96%AD%E7%82%B9.png)
* <font size=3>在控制台会打印类似错误:SnapKit.LayoutConstraint:0x7f8b1588b150@/Users/xxx/View/Cell.swift#121 PictureView:0x7f8b15892b70.height == 374.0</font>
* <font size=3>使用po 0x7f8b15892b70 查看相关信息</font>
* <font size=3>po [0x7fc82aba1210 recursiveDescription] 可以看到层级关系</font>
* <font size=3>po [[0x7fc82aba1210 superview] recursiveDescription] 查看它的父视图的层级关系</font>
* <font size=3>最后发现问题,解决它</font>
