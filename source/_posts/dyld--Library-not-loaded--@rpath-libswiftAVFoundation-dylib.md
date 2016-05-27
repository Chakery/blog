---
title: dyld Library not loaded -@rpath libswiftAVFoundation dylib
date: 2016-05-27
---

> [原文出自](http://stackoverflow.com/questions/32730312/reason-no-suitable-image-found/32730393#32730393)

问题:
```
dyld: Library not loaded: @rpath/libswiftAVFoundation.dylib
  Referenced from: /var/containers/Bundle/Application/978800D2-D772-4B2F-8C6C-04B89F6BD138/xxx.app/xxx
  Reason: no suitable image found.  Did find:
	/private/var/containers/Bundle/Application/978800D2-D772-4B2F-8C6C-04B89F6BD138/xxx.app/Frameworks/libswiftAVFoundation.dylib: mmap() errno=1 validating first page of '/private/var/containers/Bundle/Application/978800D2-D772-4B2F-8C6C-04B89F6BD138/xxx.app/Frameworks/libswiftAVFoundation.dylib'
	/private/var/containers/Bundle/Application/978800D2-D772-4B2F-8C6C-04B89F6BD138/xxx.app/Frameworks/libswiftAVFoundation.dylib: mmap() errno=1 validating first page of '/private/var/containers/Bundle/Application/978800D2-D772-4B2F-8C6C-04B89F6BD138/xxx.app/Frameworks/libswiftAVFoundation.dylib'
	/private/var/containers/Bundle/Application/978800D2-D772-4B2F-8C6C-04B89F6BD138/xxx.app/Frameworks/libswiftAVFoundation.dylib: mmap() errno=1 validating first page of '/private/var/containers/Bundle/Application/978800D2-D772-4B2F-8C6C-04B89F6BD138/xxx.app/Frameworks/libswiftAVFoundation.dylib'
```

解决方案:
```
rm -rf "$(getconf DARWIN_USER_CACHE_DIR)/org.llvm.clang/ModuleCache"
rm -rf ~/Library/Developer/Xcode/DerivedData
rm -rf ~/Library/Caches/com.apple.dt.Xcode
```
