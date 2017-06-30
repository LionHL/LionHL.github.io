---
title: mac-xcrun-error
date: 2016-10-18 14:00:50
tags: [mac,git]
---
Mac升级系统后执行git命令，出现如下错误：
```error
    xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools),
    missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
```
<!--more--> 
解决方法：
在终端执行命令:
```npm
    xcode-select --install
```