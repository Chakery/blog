---
title: 打造一个愉快的XCode控制台
date: 2016-05-27
---

> [原文出自：打造一个愉快的 Swift Debug 控制台](http://blog.dianqk.org/2016/01/26/%E6%89%93%E9%80%A0%E4%B8%80%E4%B8%AA%E6%84%89%E5%BF%AB%E7%9A%84%20Swift%20Debug%20%E6%8E%A7%E5%88%B6%E5%8F%B0/)

#1 需要用到的库/插件：
* [XCGLogger](https://github.com/DaveWoodCom/XCGLogger) 方便快捷的输出日志
* [XcodeColors](https://github.com/robbiehanson/XcodeColors) 是 Console 窗口彩色化输出（xcode插件）
* [KZLinkedConsole](https://github.com/krzysztofzablocki/KZLinkedConsole) 直接从 Console 跳转到文件（xcode插件）
* [DXXcodeConsoleUnicodePlugin](https://github.com/dhcdht/DXXcodeConsoleUnicodePlugin) 在控制台转换Unicode编码（xcode插件）

#2 安装
##2.1 添加XCGLogger
在你项目的Podfile中添加：
```
pod 'XCGLogger', '~> 3.3' // xcode7.3 必须使用3.3版本
```

##2.2 安装xcode插件
###2.2.1 直接使用[Alcatraz](http://alcatraz.io/)安装以下插件：

* XcodeColors
* KZLinkedConsole
* DXXcodeConsoleUnicodePlugin

##2.3 配置输出窗口
新建一个Log.swift文件，当然名字随意。
```
import XCGLogger

public let log: XCGLogger = {
    let log = XCGLogger.defaultInstance()
    #if DEBUG
        log.setup(.Debug, showThreadName: true, showLogLevel: true, showFileNames: true, showLineNumbers: true, writeToFile: nil)
    #else
        log.setup(.Severe, showThreadName: true, showLogLevel: true, showFileNames: true, showLineNumbers: true, writeToFile: nil)
    #endif
    log.xcodeColorsEnabled = true
    log.xcodeColors = [
        .Verbose: .lightGrey,
        .Debug: .darkGrey,
        .Info: .darkGreen,
        .Warning: .orange,
        .Error: .red,
        .Severe: .whiteOnRed
    ]
    return log
}()
```

**XCGLogger 的等级从高到地分别是：**

* Verbose
* Debug
* Info
* Warning
* Error
* Severe

##2.4 配置DEBUG
TARGETS -> Build Setting -> Other Swift Flags -> -D DEBUG
*如图：*
![](http://upload-images.jianshu.io/upload_images/2049411-7738a4b202dba2c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
*最后，效果图：*
![](http://upload-images.jianshu.io/upload_images/2049411-e241db5912e251f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
