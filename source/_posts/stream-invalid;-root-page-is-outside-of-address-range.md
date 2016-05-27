---
title: stream invalid; root page is outside of address range
date: 2016-05-27
---

编译程序的时候得到了一个不明所以的错误提示,  在[apple's forums](https://forums.developer.apple.com/thread/21723)寻到解决方案.
错误信息如下:
```
[/BuildRoot/Library/Caches/com.apple.xbs/Sources/CoreUI_Sim/CoreUI-374.1.1/Bom/Storage/BOMStorage.c:509] 
stream invalid; root page is outside of address range
```

***问题:***
导致这样的问题会有很多情况, 我这里是因为使用了不存在的资源. `user_icon`这个图片不存在.
```
UIImage(named: "user_icon")
```


**修改于2016年05月19日15:50:10**
本以为这个问题已经解决, 但是多编译了几次之后, 又重新出现了这个错误提示. 然后又重新看了一遍这个[帖子](https://forums.developer.apple.com/thread/21723), 才发现: ` Thanks for the build log, this is a harmless message that can be ignored.`

***这个信息可以忽略, 忽略, 略...***
