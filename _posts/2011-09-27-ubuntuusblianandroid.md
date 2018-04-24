---
layout: post
title: 'Ubuntu USB連Android'
author: 'James Peng'
tags: ['Android']
---

  
1.建立文件並且加入對應的內容
 
建立文件 vim /etc/udev/rules.d/11-android.rules  
文件內容 SUBSYSTEMS=="usb", SYSFS{idVendor}=="22b8",
SYSFS{idProduct}=="41db", MODE="0666"  
編完ctrl+c  
:wq!
  
建立文件 vim /etc/udev/rules.d/50-android.rules  
文件內容 SUBSYSTEMS=="usb", SYSFS{idVendor}=="0bb4", MODE="0666"
編完ctrl+c  
:wq!
 
建立文件 vim /etc/udev/rules.d/51-android.rules
文件內容 SUBSYSTEMS=="usb", ATTRS{idVendor}=="0bb4",
ATTRS{idProduct}=="0c02", MODE="0666"
編完ctrl+c  
:wq!  
  
2.更改文件權限  
  
sudo chmod a+rx /etc/udev/rules.d/11-android.rules
/etc/udev/rules.d/50-android.rules /etc/udev/rules.d/51-android.rules  
  
3.切換到放置Android的目錄下面  
 或是 設定path  
  
export PATH=\$PATH:/home/james/下載/android-ndk-r6b  
export
PATH=\$PATH:/home/james/下載/android-sdk-linux\_x86/platform-tools  
  
  
4.輸入以下 清除掉所有連線  
  
./adb kill-server  
  
5.確認連線狀況  
  
sudo ./adb devices  
  
6.如果出現device就代表已經成功了！  
  
參考來源：http://android-newbe.blogspot.com/2009/04/ubuntu-linux-gphone-android-driver.html  
  
  
參考：  
http://blog.yam.com/pigfly/article/29335620
