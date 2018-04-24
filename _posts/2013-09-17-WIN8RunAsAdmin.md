---
layout: post
title: 'WIN8所有程式 自動使用Admin權限開啟'
author: 'James Peng'
tags: ['Windows']
---

~~~text
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers
~~~   

## Under specific registry key ##

1. Create **REG_SZ** value with name as full path to executable (if path contains spaces, do not surround it with quotes)
2. Data for this value must contain string **RUNASADMIN**

![](..\images\2013-09-17-WIN8RunAsAdmin\qtbmogb.png)

## 結果 ##

![](..\images\2013-09-17-WIN8RunAsAdmin\gWANpky.png)

## 參考 ##
http://www.cievo.sk/2011/11/02/how-to-set-run-this-program-as-administrator-via-registry/

  

