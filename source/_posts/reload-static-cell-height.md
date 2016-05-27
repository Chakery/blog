---
title: reload static cell height
date: 2016-05-27
---

重新设置static cell的高度, 如果在tableView加载之前, 已经确定了高度cell的高度, 那么可以通过重载下面方法来修改static cell的高度.
```swift 
override func tableView(tableView: UITableView, heightForRowAtIndexPath indexPath: NSIndexPath) -> CGFloat
```


**但是**, 如果tableView加载之后, 才确定static cell的高度, (可能某一个cell的高度, 需要请求网络数据之后才可以确定), 也许你会想到这么做:
```swift
tableView.height = UITableViewAutomaticDimension
tableView.estimatedRowHeight = 44
```

或者是:
```swift
override func tableView(tableView: UITableView, heightForRowAtIndexPath indexPath: NSIndexPath) -> CGFloat {
    if indexPath.row == 1 {
        return unknownHeight
    }
    return 44
}

// 接下来直接刷新数据
self.tableView.reloadData()
// 或者刷新某一行
self.tableView.reloadRowsAtIndexPaths([NSIndexPath(forRow: 1, inSection: 0)], withRowAnimation: .None)
// 又或者刷新某一组
self.tableView.reloadSections(NSIndexSet(index: 0), withRowAnimation: .None)
```

遗憾的是, 高度已经发生了变化, 但是cell的内容高度并没有如愿的随之变化, 而是保持了原来的高度.

**解决方案:**
由于本人使用的是storyboard, 因此直接把static cell关联到代码区, 然后给cell设置高度, 之后再`reloadRowsAtIndexPaths`

**注意:**
必须先给static cell设置高度, 
然后重新设置内容, 
最后`reloadRowsAtIndexPaths`

顺序不能变.
