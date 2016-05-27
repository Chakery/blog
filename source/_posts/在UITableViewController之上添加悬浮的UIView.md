---
title: 在UITableViewController之上添加悬浮的UIView
date: 2016-05-27
---

如果在UIViewController中添加一个UITableView，然后在self.view中直接添加一个UIView就可以悬浮在UITableView之上。（只要确保该UIView在UITableView的上层）

但是，现在的需求是，在UITableViewController上添加一个悬浮的UIView，因为UITableViewController的`self.view`就是`tableView`，因此如果直接在self.view或者self.tableView上添加UIView的话，没办法实现悬浮效果。当然，办法总是有的，这里是通过在window上添加UIView来实现悬浮效果。

**解决方案：**
```
UIApplication.sharedApplication().windows.first!.addSubview(view)
```
