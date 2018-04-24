---
layout: post
title: 'VirtualBox建置 Ubuntu 10.04 LTS x64 Android source build 開發環境'
author: 'James Peng'
tags: ['Android']
---

[![image](http://lh5.ggpht.com/-A71ZAXXIvec/UQ0fNkodh6I/AAAAAAAAPQM/gmAYtkBc3SM/image_thumb%25255B57%25255D.png?imgmax=800 "image")](http://lh6.ggpht.com/-nHJqm48ZnZw/UQ0fNDZMzjI/AAAAAAAAPQE/UWADbdifqOo/s1600-h/image%25255B108%25255D.png)

 

下載VirtualBox並安裝：**VirtualBox 4.2.6 for Windows hosts
[x86/amd64](http://download.virtualbox.org/virtualbox/4.2.6/VirtualBox-4.2.6-82870-Win.exe)**

**官網：<https://www.virtualbox.org/wiki/Downloads>**

* * * * *

**安裝好後，請「**新增**」**

請務必用64bit，因為android 4.0必須64bit

[![image](http://lh3.ggpht.com/-tOzFboJf9rk/UQ0fOwqyKNI/AAAAAAAAPQc/QKWmVg2NIhM/image_thumb%25255B59%25255D.png?imgmax=800 "image")](http://lh5.ggpht.com/-YWsak3tKktY/UQ0fOfoO8lI/AAAAAAAAPQU/E62X7LwibQc/s1600-h/image%25255B112%25255D.png)

按照你電腦需求，建好vm後，右鍵「設定值」

剪貼簿請改「雙向」

[![image](http://lh6.ggpht.com/-n-bmijcgAAA/UQ0fQh7SZPI/AAAAAAAAPQs/JyM9Ie14QT8/image_thumb%25255B61%25255D.png?imgmax=800 "image")](http://lh5.ggpht.com/-70dUaw-UDjI/UQ0fP32E-7I/AAAAAAAAPQk/8CWJO5yPcFk/s1600-h/image%25255B116%25255D.png)

 

 

如果你ip分享器有dhcp會配ip，請改「橋接」，可以透過win7網芳傳輸檔案。

[![image](http://lh3.ggpht.com/-EQeDsXGBsNY/UQ0fSNnlJSI/AAAAAAAAPQ8/JZK_nMnemZY/image_thumb%25255B29%25255D.png?imgmax=800 "image")](http://lh5.ggpht.com/-O-pXOWnLmsU/UQ0fRV9ukpI/AAAAAAAAPQ0/1OD-W9GUQUM/s1600-h/image%25255B58%25255D.png)

 

掛載
[ubuntu-10.04.4-dvd-amd64.iso](http://cdimage.ubuntu.com/releases/lucid/release/ubuntu-10.04.4-dvd-amd64.iso)（
4.3G）

在這抓：<http://cdimage.ubuntu.com/releases/lucid/release/>

因為如果用ubuntu 12.04LTS 編譯會一堆錯誤，太麻煩了，不如用10.04LTS
簡單多了。

安裝好後，請務必安裝Guest Additions，剪貼簿才能用，畫面解析度才能正常。

[![image](http://lh5.ggpht.com/-EW9v3eUGfOg/UQ0fTWVGfcI/AAAAAAAAPRM/sw28awJvv9M/image_thumb%25255B63%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-aEUyKt3Fp0c/UQ0fSv-k1hI/AAAAAAAAPRE/S4nhmjHV0_4/s1600-h/image%25255B120%25255D.png)

* * * * *

先更新ubuntu：

[![image](http://lh5.ggpht.com/-QMjqKG4W9P0/UQ0fUjddLKI/AAAAAAAAPRc/kjIApyBB2cI/image_thumb%25255B11%25255D.png?imgmax=800 "image")](http://lh3.ggpht.com/-9-Csp6SbE98/UQ0fUCOl5pI/AAAAAAAAPRU/tp7EcrqT0Tc/s1600-h/image%25255B21%25255D.png)

點選「設定」把位於台灣的伺服器改「主要伺服器」，因為台灣的伺服器很爛常常掛掉。

[![image](http://lh6.ggpht.com/-0dJyLaFpM4E/UQ0fWGRkqEI/AAAAAAAAPRs/yVztoDTw95U/image_thumb%25255B15%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-VBP4yTZOu7A/UQ0fVTWxr5I/AAAAAAAAPRk/GLmGhl_TjbI/s1600-h/image%25255B30%25255D.png)

全勾選

[![image](http://lh3.ggpht.com/-WyAdFlSk0p4/UQ0fXpOjz1I/AAAAAAAAPR8/bP61tOx6Nio/image_thumb%25255B17%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-SKENdR4WfSo/UQ0fW4IbstI/AAAAAAAAPR0/3PtvKrVJenw/s1600-h/image%25255B34%25255D.png)

[![image](http://lh6.ggpht.com/-x6uTX0OzFLY/UQ0fY3yVR1I/AAAAAAAAPSM/E0PVq5vj9iY/image_thumb%25255B7%25255D.png?imgmax=800 "image")](http://lh6.ggpht.com/-M7HWtnve_uE/UQ0fYT87AVI/AAAAAAAAPSE/Fs7hOkyAOvY/s1600-h/image%25255B15%25255D.png)

* * * * *

因為在虛擬機器裡，所以安裝samba

> apt-get install samba samba-common
>
> sudo apt-get install smbfs

網路檔案分享的方式如下

需要去軟體中心下載搜索關鍵詞“Samba”，下載安裝如下圖形中的客戶端軟體。

[![image](http://lh6.ggpht.com/-GlepHA-gLWY/UQ0fbFS8eEI/AAAAAAAAPSc/hK1mXVvds5g/image_thumb%25255B33%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-BGJB_1uuf1s/UQ0fZ_Vb18I/AAAAAAAAPSU/5CO8vteN7Lg/s1600-h/image%25255B64%25255D.png)

[![image](http://lh6.ggpht.com/-QCsq2_64nQg/UQ0fdIE89FI/AAAAAAAAPSs/nEwBtMnYbhQ/image_thumb%25255B37%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-qUfdDrJoo1o/UQ0fcPcWzpI/AAAAAAAAPSk/QcwOR9YRQWw/s1600-h/image%25255B70%25255D.png)

打開後出現如下界面：

[![image](http://lh6.ggpht.com/-qFlf4ZHN_Yw/UQ0ffc-HHWI/AAAAAAAAPS8/K-EhOlwm-y8/image_thumb%25255B39%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-vPXYK_Mf-4I/UQ0fefZR-3I/AAAAAAAAPS0/XlB4GO74Iak/s1600-h/image%25255B74%25255D.png)

刪除沒用的共享項目，點擊“添加”按鈕，設置你的參數

[![image](http://lh4.ggpht.com/-9m81mU6nVlE/UQ0fhDdmvfI/AAAAAAAAPTM/NNnaZKEPSwM/image_thumb%25255B42%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-MbVQ8RMupSY/UQ0fgENaGnI/AAAAAAAAPTE/sBpBhyMPwT8/s1600-h/image%25255B81%25255D.png)[![image](http://lh6.ggpht.com/-wDpSvAAvgJc/UQ0fjc4rNeI/AAAAAAAAPTc/r-7zQSe-omw/image_thumb%25255B43%25255D.png?imgmax=800 "image")](http://lh3.ggpht.com/-WFe4F0GYtZ0/UQ0fiIFhaNI/AAAAAAAAPTU/x51Wjqcc6dg/s1600-h/image%25255B82%25255D.png)

注意：

1、共享名是在Windows下顯示的文件夾名稱，會在映射磁盤的時候用到

2、記得在第二頁“訪問”勾選訪問的用戶

利用這個圖形Samba軟件都配置完成之後，重啟Samba即可。

> sudo nmbd restart 
>
> sudo smbd restart 

看一下VM的ip是多少

[![image](http://lh3.ggpht.com/-GNQ6BwqMs1c/UQ0fl6Z-c7I/AAAAAAAAPTs/pfSX1VcurOI/image_thumb%25255B46%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-27ZyG7hqwJE/UQ0fkj9gI4I/AAAAAAAAPTk/cvD4GcGGUPU/s1600-h/image%25255B87%25255D.png)

 

[![image](http://lh3.ggpht.com/-rPOZpltZyJg/UQ0fn-k-sdI/AAAAAAAAPT8/QtVvCLUYgyI/image_thumb%25255B48%25255D.png?imgmax=800 "image")](http://lh5.ggpht.com/-2DFE7xvgvhg/UQ0fmzAYdLI/AAAAAAAAPT0/7r055fIvMus/s1600-h/image%25255B91%25255D.png)

在Win7中的運行輸入\\\\Ip地址\\共享名即可以訪問目錄

[![image](http://lh5.ggpht.com/-3faPlEl8OJ8/UQ0fpNgfVgI/AAAAAAAAPUM/amVo7dSCaEM/image_thumb%25255B50%25255D.png?imgmax=800 "image")](http://lh6.ggpht.com/-4INBVmfamlo/UQ0fobwT1FI/AAAAAAAAPUE/FsLWdEtjAzM/s1600-h/image%25255B95%25255D.png)

[![image](http://lh4.ggpht.com/-6LkZH9xZOkc/UQ0fqmdwufI/AAAAAAAAPUc/TNDkI0eMDbw/image_thumb%25255B52%25255D.png?imgmax=800 "image")](http://lh5.ggpht.com/-m9Y-SMd7Djc/UQ0fp7UoHrI/AAAAAAAAPUQ/MUOEvGcWd4Q/s1600-h/image%25255B99%25255D.png)

搞定。

* * * * *

source build 環境套件安裝：

移除openjdk

> \$ sudo su
>
> \$ apt-get purge openjdk\*

[![image](http://lh4.ggpht.com/-Y5tXFEZt4VE/UQ0fsZSBDUI/AAAAAAAAPUs/PfqjO5KjaZg/image_thumb%25255B3%25255D.png?imgmax=800 "image")](http://lh6.ggpht.com/-h_e2jYoepeM/UQ0frblHLYI/AAAAAAAAPUk/R_bkCM406Ec/s1600-h/image%25255B7%25255D.png)

ja輸入y

 

1. 手動安裝JDK 6

請至[Java官網](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
下載
[jdk-6uXX-linux-x64.bin](http://www.oracle.com/technetwork/java/javase/downloads/jdk-6u32-downloads-1594644.html)。並執行下列命令

> sudo su  
> chmod 777 jdk-6u38-linux-x64.bin
>
> sudo cp jdk-6u38-linux-x64.bin /opt/
>
> cd /opt/   
> ./jdk-6u38-linux-x64.bin
>
> chmod 777 -R jdk1.6.0\_38/

變更個人帳號的環境變數

修改profile檔案，加入相關的JAVA環境變數。

> \$ sudo gedit /etc/profile

於檔案尾端加入：

> \#set java environment  
> JAVA\_HOME=/opt/jdk1.6.0\_38  
> export JRE\_HOME=/opt/jdk1.6.0\_38/jre  
> export CLASSPATH=.:\$JAVA\_HOME/lib:\$JRE\_HOME/lib:\$CLASSPATH  
> export PATH=\$JAVA\_HOME/bin:\$JRE\_HOME/bin:\$PATH

變更全局環境變數：

> sudo gedit /etc/environment

PATH增加

> :/opt/jdk1.6.0\_38/bin:/opt/jdk1.6.0\_38/jre:/opt/jdk1.6.0\_38/:/opt/jdk1.6.0\_38/lib

注意不是取代喔！

例如：

> PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games"

變成

> PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/opt/jdk1.6.0\_38/bin:/opt/jdk1.6.0\_38/jre:/opt/jdk1.6.0\_38/:/opt/jdk1.6.0\_38/lib"

 

儲存後，關閉終端機命令列，重開終端機，驗證一下目前java 環境版本：

> java -version

[![image](http://lh4.ggpht.com/-o7A18HZ7QY4/UQ0ft9wLfxI/AAAAAAAAPU8/m-1rQ9TxL0U/image_thumb%25255B5%25255D.png?imgmax=800 "image")](http://lh3.ggpht.com/-FHWs2zDrWoI/UQ0ftPid43I/AAAAAAAAPU0/jB5s_BA6kAo/s1600-h/image%25255B11%25255D.png)

``

``

* * * * *

安裝依賴包：

官方給出的是：

> sudo apt-get install git-core gnupg flex bison gperf build-essential
> \\  
>   zip curl zlib1g-dev libc6-dev lib32ncurses5-dev ia32-libs \\  
>   x11proto-core-dev libx11-dev lib32readline5-dev lib32z-dev \\  
>   libgl1-mesa-dev g++-multilib mingw32 tofrodos python-markdown \\  
>   libxml2-utils xsltproc

* * * * *

安裝最新版Git

> sudo add-apt-repository ppa:git-core/ppa
>
> sudo apt-get update  
> sudo apt-get install git

* * * * *

**讓ubuntu使用usb：**

編輯usb存取權限

> `sudo gedit /etc/udev/rules.d/51-android.rules`

`加入`

> \# adb protocol on passion (Nexus One)  
> SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4e12",
> MODE="0600", OWNER="\<username\>"  
> \# fastboot protocol on passion (Nexus One)  
> SUBSYSTEM=="usb", ATTR{idVendor}=="0bb4", ATTR{idProduct}=="0fff",
> MODE="0600", OWNER="\<username\>"  
> \# adb protocol on crespo/crespo4g (Nexus S)  
> SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4e22",
> MODE="0600", OWNER="\<username\>"  
> \# fastboot protocol on crespo/crespo4g (Nexus S)  
> SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4e20",
> MODE="0600", OWNER="\<username\>"  
> \# adb protocol on stingray/wingray (Xoom)  
> SUBSYSTEM=="usb", ATTR{idVendor}=="22b8", ATTR{idProduct}=="70a9",
> MODE="0600", OWNER="\<username\>"  
> \# fastboot protocol on stingray/wingray (Xoom)  
> SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="708c",
> MODE="0600", OWNER="\<username\>"  
> \# adb protocol on maguro/toro (Galaxy Nexus)  
> SUBSYSTEM=="usb", ATTR{idVendor}=="04e8", ATTR{idProduct}=="6860",
> MODE="0600", OWNER="\<username\>"  
> \# fastboot protocol on maguro/toro (Galaxy Nexus)  
> SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4e30",
> MODE="0600", OWNER="\<username\>"  
> \# adb protocol on panda (PandaBoard)  
> SUBSYSTEM=="usb", ATTR{idVendor}=="0451", ATTR{idProduct}=="d101",
> MODE="0600", OWNER="\<username\>"  
> \# fastboot protocol on panda (PandaBoard)  
> SUBSYSTEM=="usb", ATTR{idVendor}=="0451", ATTR{idProduct}=="d022",
> MODE="0600", OWNER="\<username\>"  
> \# usbboot protocol on panda (PandaBoard)  
> SUBSYSTEM=="usb", ATTR{idVendor}=="0451", ATTR{idProduct}=="d00f",
> MODE="0600", OWNER="\<username\>"  
> \# usbboot protocol on panda (PandaBoard ES)  
> SUBSYSTEM=="usb", ATTR{idVendor}=="0451", ATTR{idProduct}=="d010",
> MODE="0600", OWNER="\<username\>"  
> \# adb protocol on grouper (Nexus 7)  
> SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4e42",
> MODE="0600", OWNER="\<username\>"  
> \# fastboot protocol on grouper (Nexus 7)  
> SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4e40",
> MODE="0600", OWNER="\<username\>"  

**新建.rules文件**

> sudo gedit /etc/udev/rules.d/50-android.rules

輸入如下內容：

> SUBSYSTEM=="usb",ATTR{idVendor}=="0bb4",MODE="0666"

儲存。

重啟udev

> sudo /etc/init.d/udev restart

透過virtualbox 使用 USB 裝置

[![image](http://lh4.ggpht.com/-dH3pehKel8Y/UQ0fvZQRtNI/AAAAAAAAPVM/el3KV4bG19M/image_thumb%25255B104%25255D.png?imgmax=800 "image")](http://lh5.ggpht.com/-bp3BS_ovLk0/UQ0fuit1oXI/AAAAAAAAPVE/bO2d740CxlQ/s1600-h/image%25255B203%25255D.png)

右鍵

[![image](http://lh3.ggpht.com/-l_6SrM5snqg/UQ0fwVF4BII/AAAAAAAAPVc/7cLjc8A3v3M/image_thumb%25255B103%25255D.png?imgmax=800 "image")](http://lh3.ggpht.com/-pycjBruviNs/UQ0fv6xq5sI/AAAAAAAAPVU/03dxbdlzy_M/s1600-h/image%25255B200%25255D.png)

選擇android裝置

[![image](http://lh4.ggpht.com/-hm75srHiFds/UQ0fx7BTkOI/AAAAAAAAPVs/HGZE7ZSOQ6U/image_thumb%25255B106%25255D.png?imgmax=800 "image")](http://lh3.ggpht.com/-19fvZN8jg48/UQ0fxKGNUYI/AAAAAAAAPVk/Qt8_gpcAZpE/s1600-h/image%25255B207%25255D.png)

查看usb設備，終端機輸入

> lsusb

[![image](http://lh3.ggpht.com/-jJi8-r6OjMc/UQ0fzeG4C0I/AAAAAAAAPV8/9nJm4CLjEWU/image_thumb%25255B108%25255D.png?imgmax=800 "image")](http://lh6.ggpht.com/-tVYoVZbwZ6w/UQ0fyjxKlhI/AAAAAAAAPV0/ttPY3iUKE2w/s1600-h/image%25255B211%25255D.png)

出現了，我的手機

Bus 001 Device 002: ID 04e8:6860 Samsung Electronics Co., Ltd

參考：<http://source.android.com/source/initializing.html>

* * * * *

開始下載source tree

> mkdir \~/bin
>
> PATH=\~/bin:\$PATH
>
> curl <https://dl-ssl.google.com/dl/googlesource/git-repo/repo> \>
> \~/bin/repo
>
> chmod a+x \~/bin/repo
>
> repo init -u <https://android.googlesource.com/platform/manifest> -b
> android-4.0.1\_r1

輸入名字留白enter鍵、輸入email留白enter鍵、y、y

> repo sync

[![image](http://lh4.ggpht.com/-uDltcX-0anc/UQ0f0zAjhJI/AAAAAAAAPWM/T8szBE-d-Fw/image_thumb%25255B21%25255D.png?imgmax=800 "image")](http://lh5.ggpht.com/-LV5lsBudZRU/UQ0f0DR_Y8I/AAAAAAAAPWE/T2zXfdJFsgk/s1600-h/image%25255B42%25255D.png)

參考：<http://source.android.com/source/downloading.html>

* * * * *

編譯

> . build/envsetup.sh
>
> lunch full-eng
>
> make –j8

成功，這個時候直接運行emulator，會自動加載你剛編出來的img並開機啟動，然後就可以用啦

> emulator

[![image](http://lh6.ggpht.com/-9dHmOeQonEM/UQ0f2W3bSoI/AAAAAAAAPWc/OrpqWsgv7no/image_thumb%25255B23%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-30qxt5btuj0/UQ0f1lEcYiI/AAAAAAAAPWU/KXvJcGFdGso/s1600-h/image%25255B46%25255D.png)

參考：

<http://source.android.com/source/building.html>

* * * * *

下載Eclipse

請到官網： <http://www.eclipse.org/downloads/>

下載「[Eclipse Classic 4.2.1, 182 MB Linux 64
Bit版](http://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/juno/SR1/eclipse-java-juno-SR1-linux-gtk-x86_64.tar.gz)」

下載成功，獲得.tar.gz文件

解壓.tar.gz文件

> tar -zxvf eclipse-SDK-4.2.1-linux-gtk-x86\_64.tar.gz

 

* * * * *

下載ADT (Android Developer Tools)

請到官網：<http://developer.android.com/sdk/index.html>

ADT跟SDK Tools有什麼不一樣？  
簡單來說ADT Bundle就是google把Android Developer Tools如 Eclipse 和相關
SDK
Tools套件全部包起來。因為我們自己下載最新版的eclipse所以只需要選擇Linux版本下載SDK
Tools「
[android-sdk\_r21.0.1-linux.tgz](http://dl.google.com/android/android-sdk_r21.0.1-linux.tgz)」

下載成功後，獲得.tgz文件

解壓.tgz文件

> tar -zxvf android-sdk\_r21.0.1-linux.tgz

運行 更新Android SDK

> /android-sdk-linux/tools/android

[![image](http://lh3.ggpht.com/-R2ziPty65yQ/UQ0f3jqLpZI/AAAAAAAAPWs/wsTu5mAY6as/image_thumb%25255B65%25255D.png?imgmax=800 "image")](http://lh5.ggpht.com/-1lcCCiTOn2E/UQ0f3HVjOcI/AAAAAAAAPWk/tPrlHDoF3X0/s1600-h/image%25255B124%25255D.png)

勾選Tools和最新版本的Andr​​oid API，Extra按需要勾選，Install安裝

* * * * *

更新Eclipse：

運行 Eclipse

> /eclipse/eclipse

選單，Help -\> check for upfates

[![image](http://lh4.ggpht.com/-QPgaBCs0vmk/UQ0f5BVWIaI/AAAAAAAAPW8/xrAXji7Ema0/image_thumb%25255B70%25255D.png?imgmax=800 "image")](http://lh5.ggpht.com/-R4M_nehbuys/UQ0f4eiegzI/AAAAAAAAPW0/3gi9Hfyvtlk/s1600-h/image%25255B137%25255D.png)

如果出現

訊息「There are mp update sites to search. do you wish to open the 
“Available Software Sites” preferences?」按下yes

[![image](http://lh5.ggpht.com/-BYNe3DNCT9M/UQ0f6a6qPUI/AAAAAAAAPXM/e9s-AWnI4Co/image_thumb%25255B71%25255D.png?imgmax=800 "image")](http://lh3.ggpht.com/-AcsVFsVAi2s/UQ0f5xUaqPI/AAAAAAAAPXE/fzuESQaw99I/s1600-h/image%25255B138%25255D.png)

add兩個網址

> http://download.eclipse.org/releases/juno
>
> http://download.eclipse.org/eclipse/updates/4.2

[![image](http://lh5.ggpht.com/-Afe52MdpS74/UQ0f7q6xSiI/AAAAAAAAPXc/B5I5O1aWNaI/image_thumb%25255B72%25255D.png?imgmax=800 "image")](http://lh3.ggpht.com/-cDjHnMs3qog/UQ0f7ItReyI/AAAAAAAAPXU/kV0et8Ho8IY/s1600-h/image%25255B139%25255D.png)

[![image](http://lh5.ggpht.com/-XKfePPVKSX8/UQ0f8074VCI/AAAAAAAAPXs/-se8w6Is_XM/image_thumb%25255B75%25255D.png?imgmax=800 "image")](http://lh6.ggpht.com/-FbGhOnM12b0/UQ0f8ehr1_I/AAAAAAAAPXk/tieBdS35TlU/s1600-h/image%25255B144%25255D.png)

[![image](http://lh4.ggpht.com/-rtS6W7fdh58/UQ0f-B-WH3I/AAAAAAAAPX8/NS-t8nbDtmI/image_thumb%25255B73%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-Q_Wi0Qjjr9I/UQ0f9nYgvDI/AAAAAAAAPX0/TopFaY51CA4/s1600-h/image%25255B140%25255D.png)  

ok 重新更新 即可。

* * * * *

安裝ADT：

選單，Help -\> Install New Program

[![image](http://lh3.ggpht.com/-dBzrwUlPbUM/UQ0f_09-sAI/AAAAAAAAPYM/FzpCs88BFOw/image_thumb%25255B77%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-XB1oqb8h4IM/UQ0f_Fs66yI/AAAAAAAAPYE/AYKGJI9IS4g/s1600-h/image%25255B148%25255D.png)

點擊add按鈕，在loaction文字輸入欄中輸入地址：

> <https://dl-ssl.google.com/android/eclipse/>

[![image](http://lh6.ggpht.com/-4G2Y6HF0W9g/UQ0gBHz41jI/AAAAAAAAPYc/w7F3yl9aRH4/image_thumb%25255B79%25255D.png?imgmax=800 "image")](http://lh5.ggpht.com/--WkuLXWUMl8/UQ0gASiiNYI/AAAAAAAAPYU/4fjqmTruGl4/s1600-h/image%25255B152%25255D.png)

點擊OK按鈕

[![image](http://lh3.ggpht.com/-mcvhY427Fh8/UQ0gCHGxktI/AAAAAAAAPYs/JzmchosQGa0/image_thumb%25255B81%25255D.png?imgmax=800 "image")](http://lh6.ggpht.com/-638Zf0eBwuw/UQ0gBpJSIkI/AAAAAAAAPYk/wS3wZDukqpw/s1600-h/image%25255B156%25255D.png)

點Select All，點Next，點Next

[![image](http://lh3.ggpht.com/-JBDRYUARF9I/UQ0gDs7jkZI/AAAAAAAAPY8/CP8uDD4mhUI/image_thumb%25255B83%25255D.png?imgmax=800 "image")](http://lh5.ggpht.com/-4bSvRTUjkfM/UQ0gCxhYZgI/AAAAAAAAPY0/BAKg0R0PEqQ/s1600-h/image%25255B160%25255D.png)

選 accept，點 finish

[![image](http://lh4.ggpht.com/-qRfXJS9DB8M/UQ0gEwlr7QI/AAAAAAAAPZM/yoBTrNix0M8/image_thumb%25255B85%25255D.png?imgmax=800 "image")](http://lh6.ggpht.com/-o-RePOxeSME/UQ0gEPeQDcI/AAAAAAAAPZE/eRoo14S-0aI/s1600-h/image%25255B164%25255D.png)

成功安裝ADT，提示重啟Eclipse，yes

[![image](http://lh3.ggpht.com/-VoWQVsioBhw/UQ0gGLe5YUI/AAAAAAAAPZc/oycoj5Hv294/image_thumb%25255B87%25255D.png?imgmax=800 "image")](http://lh5.ggpht.com/-D6uQKzQmNiA/UQ0gFjDRbyI/AAAAAAAAPZU/5xKaWScn7ZU/s1600-h/image%25255B168%25255D.png)

會跳出 要指定sdk所在位址

[![image](http://lh6.ggpht.com/-aHdsh9IZDaI/UQ0gH7uN5NI/AAAAAAAAPZs/eN8Tafemie0/image_thumb%25255B89%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-_d94ZvehJBI/UQ0gHMM-qEI/AAAAAAAAPZk/nLNOOq_PVWs/s1600-h/image%25255B172%25255D.png)

點選open preferences

[![image](http://lh3.ggpht.com/-JRgTfR8vhOE/UQ0gJN2R4zI/AAAAAAAAPZ8/o1-BQF5KDYE/image_thumb%25255B91%25255D.png?imgmax=800 "image")](http://lh5.ggpht.com/-xYVzk5tK8HM/UQ0gIqfZupI/AAAAAAAAPZ0/UiNRes_96iM/s1600-h/image%25255B176%25255D.png)

點proceed

[![image](http://lh3.ggpht.com/-tJTfCabTJv4/UQ0gKaqk4zI/AAAAAAAAPaM/ayJc9UOPdhQ/image_thumb%25255B93%25255D.png?imgmax=800 "image")](http://lh5.ggpht.com/-FOD4nZdkj6o/UQ0gJ5HkkVI/AAAAAAAAPaE/snGEXbX95MU/s1600-h/image%25255B180%25255D.png)

由於我們之前已經下載了SDK，所以選擇“Using existing
SDKs”，選擇SDK所在目錄

[![image](http://lh4.ggpht.com/-pQQVWo7P-JU/UQ0gL663j4I/AAAAAAAAPac/jnZ_LZKrpPw/image_thumb%25255B95%25255D.png?imgmax=800 "image")](http://lh3.ggpht.com/-EtKoyvrlp6w/UQ0gLL89k5I/AAAAAAAAPaU/Vh20Y8xJME4/s1600-h/image%25255B184%25255D.png)

Next，Finish，這樣，ADT就安裝完成了。

* * * * *

**創建AVD**

Windows -\>"Android Virtual Device Manager"

[![image](http://lh4.ggpht.com/-gRKPc6SzyT8/UQ0gNXQtD1I/AAAAAAAAPas/1tL1SPS3fec/image_thumb%25255B97%25255D.png?imgmax=800 "image")](http://lh6.ggpht.com/-WrHrnc2czm0/UQ0gMgye44I/AAAAAAAAPak/bJYuehgdqeA/s1600-h/image%25255B188%25255D.png)

現在還沒有任何虛擬機，點擊New按鈕我們新建一個

[![image](http://lh4.ggpht.com/-q1d5LUTxISg/UQ0gO_lH3RI/AAAAAAAAPa8/DffSNZN8qkA/image_thumb%25255B99%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-trNgEuOFXAM/UQ0gOFj-ZAI/AAAAAAAAPa0/aa4sbdXDIq8/s1600-h/image%25255B192%25255D.png)

創建成功後，我們就有了一個名為AVD4.1.2的虛擬機，來運行程序

[![image](http://lh4.ggpht.com/-i8MejDZlRII/UQ0gQL8SmLI/AAAAAAAAPbM/ajBGv4e4Bds/image_thumb%25255B101%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-8uS-ampv2NI/UQ0gPsYcGOI/AAAAAAAAPbE/yMDIDU4KTBU/s1600-h/image%25255B196%25255D.png)

完成！

* * * * *

**設定NDK環境：**

下載NDK

官方網站：<http://developer.android.com/tools/sdk/ndk/index.html>

選擇Linux 32/64-bit
(x86)版本下載「[android-ndk-r8d-linux-x86.tar.bz2](http://dl.google.com/android/ndk/android-ndk-r8d-linux-x86.tar.bz2)」

下載成功，獲得.tar.bz2文件

獲得.tar.bz2文件先改權限：

> chmod 777 android-ndk-r8d-linux-x86.tar.bz2  

解壓.tar.bz2文件

> tar -jxvf  android-ndk-r8d-linux-x86.tar.bz2

改權限：

> chmod 777 -R android-ndk-r8d  

配置Eclipse插件

打開Eclipse，設置NDK路徑，Window -\> Preferences -\> Android -\> NDK

[![image](http://lh4.ggpht.com/-9PUen1VQEVU/UQ0gRyIfqEI/AAAAAAAAPbc/4OFU7ZuGTrw/image_thumb%25255B110%25255D.png?imgmax=800 "image")](http://lh6.ggpht.com/-uKMgXVh6R4w/UQ0gQ4-xALI/AAAAAAAAPbU/-0OcjXQ6j2M/s1600-h/image%25255B215%25255D.png)

右鍵點擊項目，Android Tools -\> Add Native Support

[![image](http://lh6.ggpht.com/-D7m3oXwME3U/UQ0gTUF_XsI/AAAAAAAAPbs/Q1Ngcn0JUqs/image_thumb%25255B112%25255D.png?imgmax=800 "image")](http://lh3.ggpht.com/--0C6QuTKOv0/UQ0gSq2vnCI/AAAAAAAAPbk/uiJuL9b59eU/s1600-h/image%25255B219%25255D.png)  
前提：Eclipse請先成功安裝ADT

* * * * *

加入sdk和ndk全局環境變數：

變更個人帳號的環境變數

修改profile檔案，加入相關的JAVA環境變數。

> \$ sudo gedit /etc/profile

於PATH增加（注意不是取代喔！）

> :/home/dev/Download/android-sdk-linux/platform-tools  
> :/home/dev/Download/android-ndk-r8d

例如：

> \#set java environment  
> JAVA\_HOME=/opt/jdk1.6.0\_38  
> export JRE\_HOME=/opt/jdk1.6.0\_38/jre  
> export CLASSPATH=.:\$JAVA\_HOME/lib:\$JRE\_HOME/lib:\$CLASSPATH  
> **export SDKPATH=/home/dev/Download/android-sdk-linux/platform-tools  
> export NDKPATH=/home/dev/Download/android-ndk-r8d**  
> export
> PATH=\$JAVA\_HOME/bin:\$JRE\_HOME/bin:\$PATH:\$SDKPATH:\$NDKPATH

 

變更全局環境變數：

> sudo gedit /etc/environment

PATH增加

> :/home/dev/Download/android-sdk-linux/platform-tools:/home/dev/Download/android-ndk-r8d

注意不是取代喔！

儲存後，關閉終端機命令列，重開終端機

* * * * *

參考：

-   <http://www.cnblogs.com/dyingbleed/archive/2012/09/27/2704800.html>
-   <http://www.cnblogs.com/dyingbleed/archive/2012/09/27/2705907.html>
-   <http://www.cnblogs.com/dyingbleed/archive/2012/10/07/2714023.html>

