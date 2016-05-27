---
title: Swift 使用 #warning
date: 2016-05-27
---

swift 中没法使用`#Warning`来提示警告, 可以通过给`TODO:` `FIXME:`加上警告, 实现类似的效果.

Build Phases ---> Run Script ---> add a new Build Phases ---> new run script phase

```
TAGS="TODO:|FIXME:"
echo "searching ${SRCROOT} for ${TAGS}"
find "${SRCROOT}" \( -name "*.swift" \) -print0 | xargs -0 egrep --with-filename --line-number --only-matching "($TAGS).*\$" | perl -p -e "s/($TAGS)/ warning: \$1/"
```
如图:
![](http://upload-images.jianshu.io/upload_images/2049411-bebc07db48c2dbb9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/2049411-96dfaaed44745562.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
