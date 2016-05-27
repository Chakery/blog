---
title: "UINavigation隐藏方式"
date: 2016-05-27
---

1. 向上滚动时隐藏NavigationBar
```swift
override func viewDidAppear(animated: Bool) {
        super.viewDidAppear(animated)
        
        navigationController?.hidesBarsOnSwipe = true
    }
```
![向上滚动时隐藏NavigationBar](https://raw.githubusercontent.com/Chakery/images/master/UINavigationTip/1.gif)

2. 点击屏幕时隐藏NavigationBar
```swift
override func viewDidAppear(animated: Bool) {
        super.viewDidAppear(animated)
         
        // setting hidesBarsOnTap to true
        navigationController?.hidesBarsOnTap = true
    }
```
![点击屏幕时隐藏NavigationBar](https://raw.githubusercontent.com/Chakery/images/master/UINavigationTip/2.gif)

3. 当键盘显示的时隐藏NavigationBar
```swift
override func viewDidAppear(animated: Bool) {
        super.viewDidAppear(animated)
         
        // in this case, it's good to combine hidesBarsOnTap with hidesBarsWhenKeyboardAppears
        // so the user can get back to the navigation bar to save
        navigationController?.hidesBarsWhenKeyboardAppears = true
    }
```
![当键盘显示的时隐藏NavigationBar](https://raw.githubusercontent.com/Chakery/images/master/UINavigationTip/3.gif)
