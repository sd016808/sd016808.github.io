---
layout: post
title: 'E/PackageManager: Package  has mismatched uid: on disk, in settings'
author: 'James Peng'
tags: ['Android']
---

  
 錯誤訊息  

> 02-01 00:02:07.265: E/PackageManager(154): Package jia-hong-peng.github.io has
> mismatched uid: 10036 on disk, 10038 in settings02-01 00:02:08.565:
> E/DefContainer(564): Couldn't copy file: /data/local/tmp/xxxx.apk

  

1. 打開終端,Terminal

**adb shell**

  

2. 用cat查看一下

**cat /data/system/uiderrors.txt**

  

你應該可以在最後看到剛才那個關閉的應用程序的錯誤信息。

往往都是uid的錯誤，顯示Package xxx has mismatched uid: 100XX on disk,
100xx in settings.

> 2/1/11 12:09 AM: No settings file; creating initial state  
> 2/1/11 12:02 AM: Package jia-hong-peng.github.io has mismatched uid: 10036 on
> disk,  
> 10037 in settings  
> 2/1/11 12:02 AM: Package  jia-hong-peng.github.io  has mismatched uid: 10036
> on disk,  
> 10038 in settings  
> 2/1/11 12:06 AM: Package  jia-hong-peng.github.io  has mismatched uid: 10036
> on disk,  
> 10039 in settings  
> 2/1/11 12:06 AM: Package  jia-hong-peng.github.io  has mismatched uid: 10036
> on disk,  
> 10040 in settings  
> 2/1/11 12:06 AM: Package  jia-hong-peng.github.io  has mismatched uid: 10036
> on disk,  
> 10041 in settings  
> 2/1/11 1:03 AM: Package  jia-hong-peng.github.io  has mismatched uid: 10036 on
> disk, 1  
> 0042 in settings  
> 2/1/11 12:01 AM: Package  jia-hong-peng.github.io  has mismatched uid: 10036
> on disk,  
> 10037 in settings  
> 2/1/11 12:02 AM: Package  jia-hong-peng.github.io  has mismatched uid: 10036
> on disk,  
> 10038 in settings  
> \#

3. 找到錯誤了就好辦了，su獲得root權限

**su**

  

4.接著 把xml抓下來

**adb poll /data/system/packages.xml**

  
 找到Package  xxx所在的XML節點，把uid改成100XX（on
disk的那個）。【在舊uid上dw刪除，然後輸入你正確的】保存  
  
  
 5.傳回去  
 **adb push packages.xml  /data/system/packages.xml**  
  
  
 6. reboot重啟機器。  
  
  
  
起來後你再試試這個程序，是不是可以執行了？  

4。 輸入：WQ並退出VI。

![](http://www.google.com/uds/css/small-logo.png)

