---
layout: post
title: 'ubuntu編譯Android入門：環境建置'
author: 'James Peng'
tags: ['Android']
---

![Android Mascot](http://source.android.com/images/home-bugdroid.png)

 

### 環境建置

這篇"入門" 將介紹如何建置工作環境，如何利用 git 取得 Android
Source，以及如何編譯抓下來的的Source Code。 要編譯 Android Source
需要使用Linux或Mac OS。目前不支援Windows。

**Note*: source 的大小約為 2.6GB。您將需要空出10GB來完成編譯。*

* * * * *

### 建立一個 Linux編譯環境

*Note: 也可以在Linux虛擬機器編譯Android，需要至少8GB的RAM
/SWAP和12GB以上的硬碟空間。*

一般來說，你將需要：

-   Ubuntu (10.04 and later)

-   Python 2.4 - 2.7， 您可以從
    [python.org](http://www.python.org/download/) 取得。

-   JDK 6 若你想編譯 Gingerbread (2.3) 或 未來更新的版本；而 JDK 5 是給
    Froyo(2.2) 或 更舊版本，您可以從
    [java.sun.com](http://java.sun.com/javase/downloads/) 取得。

-   Git 1.5.4 或 更新版本。 您可以從
    [git-scm.com](http://git-scm.com/download)取得。

 

 

* * * * *

#### 安裝 JDK

Java 6： 若你想編譯 Gingerbread (2.3) 或 未來更新的版本

> `$ sudo add-apt-repository "deb http://archive.canonical.com/ lucid partner" `
>
> `$ sudo add-apt-repository "deb-src http://archive.canonical.com/ubuntu lucid partner" `
>
> `$ sudo apt-get update `
>
> `$ sudo apt-get install sun-java6-jdk `

 

Java 5: 是給 Froyo(2.2) 或 更舊版本

> `$ sudo add-apt-repository "deb http://archive.ubuntu.com/ubuntu dapper main multiverse" `
>
> `$ sudo add-apt-repository "deb http://archive.ubuntu.com/ubuntu dapper-updates main multiverse" `
>
> `$ sudo apt-get update `
>
> `$ sudo apt-get install sun-java5-jdk `

* * * * *

#### 安裝所需的套件

> `$ sudo apt-get install git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev libc6-dev lib32ncurses5-dev ia32-libs x11proto-core-dev libx11-dev lib32readline5-dev lib32z-dev libgl1-mesa-dev g++-multilib mingw32 tofrodos `

[若無法安裝套件]

****

* * * * *

#### 設定 USB 存取 （有root權限 跳過）

在 GNU/linux 系統 (尤其在 Ubuntu 系統)，普通user預設不能直接存取
USB設備的，所以需要設定才能存取。

建議的方法是建立一個檔案 `/etc/udev/rules.d/51-android.rules` (as the
root user) ，並複製下列指令於其中。將實際用戶名稱 去取代\<username\>。

 

> `# adb protocol on passion (Nexus One) SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4e12", MODE="0600", OWNER="<username>" `
>
> `# fastboot protocol on passion (Nexus One) SUBSYSTEM=="usb", ATTR{idVendor}=="0bb4", ATTR{idProduct}=="0fff", MODE="0600", OWNER="<username>" `
>
> `# adb protocol on crespo (Nexus S) SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4e22", MODE="0600", OWNER="<username>" `
>
> `# fastboot protocol on crespo (Nexus S) SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4e20", MODE="0600", OWNER="<username>" `

新規則將於下一次生效，因此有必要拔掉USB並插回電腦。

以上只適用於 Ubuntu 8.04.x​​ LTS 和 10.04.x​​
LTS。其他版本的Ubuntu或其他變種的GNU / Linux 可能會有不同設定。

* * * * *

###  

### 下一篇： [下載Android原始碼]

 

* * * * *

參考：  
<http://source.android.com/source/initializing.html>

