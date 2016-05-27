---
title: "CocoaPods trunk push 的坑"
date: 2016-05-27
---

### 坑一
```
[!] You (xxxEmail) are not allowed to push new versions for this pod. The owners of this pod are xxxEmail.
```
原因：该邮件地址不正确。第一次push所使用的邮箱跟现在的邮箱不一致，但是笔者已经弃用了之前的邮箱。
解决方案：重新注册原来的邮箱，然后 add-owner
```
pod trunk register Email@emila.com "UserName"
pod trunk add-owner 仓库名 ChakeryChen@gmail.com
```

### 坑二
```
[!] There was an error adding chakerychen@gamil.com to Chakery on trunk: getaddrinfo: nodename nor servname provided, or not known
```
解决方案：关闭代理，翻墙工具等，然后重试（多次重试无效，请修复DNS）
