---
title: no identity found
date: 2016-05-27
---

***问题:***

```
Command /bin/sh failed with exit code 1

PhaseScriptExecution 📦\ Embed\ Pods\ Frameworks /Users/yourMac/Library/Developer/Xcode/DerivedData/ProjectName-eyofkrwwvzbqwqgwvbbrpkhwamve/Build/Intermediates/ProjectName.build/Debug-iphoneos/ProjectName.build/Script-FE87731C6B456BBA55A0B469.sh
    cd /Users/yourMac/Documents/CompanyProject/ProjectName
    /bin/sh -c /Users/yourMac/Library/Developer/Xcode/DerivedData/ProjectName-eyofkrwwvzbqwqgwvbbrpkhwamve/Build/Intermediates/ProjectName.build/Debug-iphoneos/ProjectName.build/Script-FE87731C6B456BBA55A0B469.sh

mkdir -p /Users/yourMac/Library/Developer/Xcode/DerivedData/ProjectName-eyofkrwwvzbqwqgwvbbrpkhwamve/Build/Products/Debug-iphoneos/ProjectName.app/Frameworks
rsync -av --filter "- CVS/" --filter "- .svn/" --filter "- .git/" --filter "- .hg/" --filter "- Headers" --filter "- PrivateHeaders" --filter "- Modules" "/Users/yourMac/Library/Developer/Xcode/DerivedData/ProjectName-eyofkrwwvzbqwqgwvbbrpkhwamve/Build/Products/Debug-iphoneos/ActionCableClient.framework" "/Users/yourMac/Library/Developer/Xcode/DerivedData/ProjectName-eyofkrwwvzbqwqgwvbbrpkhwamve/Build/Products/Debug-iphoneos/ProjectName.app/Frameworks"
building file list ... done
ActionCableClient.framework/
ActionCableClient.framework/ActionCableClient
ActionCableClient.framework/Info.plist
ActionCableClient.framework/_CodeSignature/
ActionCableClient.framework/_CodeSignature/CodeResources

sent 399808 bytes  received 98 bytes  799812.00 bytes/sec
total size is 399396  speedup is 1.00
Code Signing /Users/yourMac/Library/Developer/Xcode/DerivedData/ProjectName-eyofkrwwvzbqwqgwvbbrpkhwamve/Build/Products/Debug-iphoneos/ProjectName.app/Frameworks/ActionCableClient.framework with Identity iPhone Developer: yourEmail@gmail.com (XP8M6628G4)
/usr/bin/codesign --force --sign A10E2F58BF2FF44A9F93AE308F79E9B143FA92FF --preserve-metadata=identifier,entitlements "/Users/yourMac/Library/Developer/Xcode/DerivedData/ProjectName-eyofkrwwvzbqwqgwvbbrpkhwamve/Build/Products/Debug-iphoneos/ProjectName.app/Frameworks/ActionCableClient.framework"
A10E2F58BF2FF44A9F93AE308F79E9B143FA92FF: no identity found
Command /bin/sh failed with exit code 1
```

***解决方案:***
删除过期的profile文件, 然后重新安装

```
cd ~/Library/MobileDevice/Provisioning\ Profiles/

rm -rf ./*
```
