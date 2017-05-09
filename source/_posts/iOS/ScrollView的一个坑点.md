---
layout: post
title: UIScrollView的坑点
comments: true
date: 2016-01-16 13:44:12
tags:
    - iOS
    - Swift
    - Objective-C
    - 坑点
---

# <font color=orange>一、UIScrollView的contentSize设置好了之后, UIScrollView没有弹簧效果</font>

## UITableView 和 UICollectionView默认都是有弹簧效果
UITableView 和 UICollectionView都是继承于UIScrollView, 它们两个默认情况下弹簧效果都是打开的, 即不管UITableView 和 UICollectionView的contentSize是什么情况, 那么它们在设置的滚动方向上都是有弹簧效果.

>   @property(nonatomic)         BOOL                         bounces;                        // default YES. if YES, bounces past edge of content and back again
@property(nonatomic)         BOOL                         alwaysBounceVertical;           // default NO. if YES and bounces is YES, even if content is smaller than bounds, allow drag vertically
@property(nonatomic)         BOOL                         alwaysBounceHorizontal;         // default NO. if YES and bounces is YES, even if content is smaller than bounds, allow drag horizontally

<!--more-->
## UIScrollView默认没有弹簧效果
UIScrollView有点不一样, 假如UIScrollView的contentSize比UIScrollView的size小, 那么它默认是不能滚动的, 原因就在于alwaysBounceVertical和alwaysBounceHorizontal默认是NO, 如果需要滚动则要把对应的属性改为YES, bounces也需要设置为YES.

# <font color=orange>二、使用Masonry、SnapKit和storyboard对UIScrollView、UITableView和UICollectionView添加约束的时候, 提示Scrollable content size is ambiguous for XXXX</font>
一般出现这中情况时, 是由于没有正确设置UIScrollView、UITableView和UICollectionView的上下约束导致无法确定它的contentSize所致.

