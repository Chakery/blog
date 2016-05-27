---
title: 在Swift中使用NSClassFromString
date: 2016-05-27
---

NSClassFromString("") 在swift中已经没法正常使用。应使用一下代替：
```
    if  let appName: String? = NSBundle.mainBundle().objectForInfoDictionaryKey("CFBundleName") as! String? {
    
        let classStringName = "_TtC\(appName!.characters.count)\(appName!)\(className.characters.count)\(className)"
    
        let  cls: AnyClass? = NSClassFromString(classStringName)
    }
    ```
但是，如果代码需要打包成framework, 需要把APP名字更改为framework的名字, 否则...
