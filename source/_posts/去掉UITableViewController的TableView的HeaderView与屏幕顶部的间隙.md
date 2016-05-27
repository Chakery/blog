---
title: 去掉UITableViewController的TableView的HeaderView与屏幕顶部的间隙
date: 2016-05-27
---

在使用UITableViewController的时候, tableHeaderView默认对齐StatusBar的底部, 这时候距离屏幕顶部会出现20像素点的间隙.

***解决方案***
1. 使用Storyboard时, 选择UITableViewController, 在属性栏把`Adjust Scroll View Insets`取消勾选, 如图:
![](http://upload-images.jianshu.io/upload_images/2049411-7e09d751b713e8a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 使用代码, 如下
```
self.automaticallyAdjustsScrollViewInsets = false
```
