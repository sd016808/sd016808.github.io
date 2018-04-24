---
layout: post
title: 'WIN7 開啟被群組原則禁用的 USB 裝置 '
author: 'James Peng'
tags: ['Windows']
---

存取公司 USB 時，出現下圖：

![](..\images\2016-05-12-WIN7_HowToUseGroupPolicyToEnableUSBdrives\cSCbDAJ.png)

這是被 群組原則(Group Policy) 禁止寫入 USB 裝置


## 解法： ##

找到機碼：

~~~text
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\RemovableStorageDevices
~~~   

![](..\images\2016-05-12-WIN7_HowToUseGroupPolicyToEnableUSBdrives\mQtvKLW.png)

把裡面的 刪了

![](..\images\2016-05-12-WIN7_HowToUseGroupPolicyToEnableUSBdrives\CGtNgif.png)

搞定...記得重開機

## 參考 ##
http://www.cievo.sk/2011/11/02/how-to-set-run-this-program-as-administrator-via-registry/

  

