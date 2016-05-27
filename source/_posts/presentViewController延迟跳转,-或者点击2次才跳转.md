---
title: presentViewController延迟跳转, 或者点击2次才跳转
date: 2016-05-27
---

presentViewController延迟跳转, 或者点击2次才跳转.

**解决方案:**
把`presentViewController`放在主线程中执行.
```swift
dispatch_async(dispatch_get_main_queue(), ^{
    [self presentViewController: viewController animated:YES completion: nil];
});
```
