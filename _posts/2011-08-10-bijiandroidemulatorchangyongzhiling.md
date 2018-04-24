---
layout: post
title: '[筆記]Android emulator常用指令'
author: 'James Peng'
tags: ['Android']
---

Adb 全名是 Android Debug Bridge，是開發或使用 Android
時很常用到的工具。使用者可以從Android 官方站下載 SDK，在其中的
platform-tools (原本在 \\Tools) 中找到。

當機器上有打開 USB debug mode 時，使用者即可通過adb 進行各種 debug
、底層(linux user space)的 Android 功能。

主要用這篇是因為方便我以後查詢Android相關指令

> Android Debug Bridge version 1.0.26
>
> -d                            - directs command to the only connected
> USB device  
>                                  returns an error if more than one USB
> device is present.  
>  -e                            - directs command to the only running
> emulator.  
>                                  returns an error if more than one
> emulator is running.  
>  -s \<serial number\>            - directs command to the USB device
> or emulator with  
>                                  the given serial number. Overrides
> ANDROID\_SERIAL  
>                                  environment variable.  
>  -p \<product name or path\>     - simple product name like 'sooner',
> or  
>                                  a relative/absolute path to a
> product  
>                                  out directory like
> 'out/target/product/sooner'.
>
>                                  If -p is not specified, the
> ANDROID\_PRODUCT\_OUT
>
>                                  environment variable is used, which
> must  
>                                  be an absolute path.  
>  devices                       - list all connected devices  
>  connect \<host\>[:\<port\>]       - connect to a device via TCP/IP  
>                                  Port 5555 is used by default if no
> port number  
> is specified.  
>  disconnect [\<host\>[:\<port\>]]  - disconnect from a TCP/IP
> device.  
>                                  Port 5555 is used by default if no
> port number  
> is specified.  
>                                  Using this ocmmand with no additional
> arguments
>
>                                  will disconnect from all connected
> TCP/IP devices.
>
> device commands:  
>   adb push \<local\> \<remote\>    - copy file/dir to device  
>   adb pull \<remote\> [\<local\>]  - copy file/dir from device  
>   adb sync [ \<directory\> ]     - copy host-\>device only if
> changed  
>                                  (-l means list but don't copy)  
>                                  (see 'adb help all')  
>   adb shell                    - run remote shell interactively  
>   adb shell \<command\>          - run remote shell command  
>   adb emu \<command\>            - run emulator console command  
>   adb logcat [ \<filter-spec\> ] - View device log  
>   adb forward \<local\> \<remote\> - forward socket connections  
>                                  forward specs are one of:  
>                                    tcp:\<port\>  
>                                    localabstract:\<unix domain socket
> name\>  
>                                    localreserved:\<unix domain socket
> name\>  
>                                    localfilesystem:\<unix domain
> socket name\>  
>                                    dev:\<character device name\>  
>                                    jdwp:\<process pid\> (remote
> only)  
>   adb jdwp                     - list PIDs of processes hosting a JDWP
> transport
>
>   adb install [-l] [-r] [-s] \<file\> - push this package file to the
> device and install it  
>                                  ('-l' means forward-lock the app)  
>                                  ('-r' means reinstall the app,
> keeping its data)  
>                                  ('-s' means install on SD card
> instead of internal storage)  
>   adb uninstall [-k] \<package\> - remove this app package from the
> device  
>                                  ('-k' means keep the data and cache
> directories)  
>   adb bugreport                - return all information from the
> device  
>                                  that should be included in a bug
> report.
>
>   adb help                     - show this help message  
>   adb version                  - show version num
>
> DATAOPTS:  
>  (no option)                   - don't touch the data partition  
>   -w                           - wipe the data partition  
>   -d                           - flash the data partition
>
> scripting:  
>   adb wait-for-device          - block until device is online  
>   adb start-server             - ensure that there is a server
> running  
>   adb kill-server              - kill the server if it is running  
>   adb get-state                - prints: offline | bootloader |
> device  
>   adb get-serialno             - prints: \<serial-number\>  
>   adb status-window            - continuously print device status for
> a specified device  
>   adb remount                  - remounts the /system partition on the
> device read-write  
>   adb reboot [bootloader|recovery] - reboots the device, optionally
> into the bootloader or recovery program  
>   adb reboot-bootloader        - reboots the device into the
> bootloader  
>   adb root                     - restarts the adbd daemon with root
> permissions  
>   adb usb                      - restarts the adbd daemon listening on
> USB  
>   adb tcpip \<port\>             - restarts the adbd daemon listening
> on TCP on th  
> e specified port  
> networking:  
>   adb ppp \<tty\> [parameters]   - Run PPP over USB.  
>  Note: you should not automatically start a PPP connection.  
>  \<tty\> refers to the tty for PPP stream. Eg.
> dev:/dev/omap\_csmi\_tty1  
>  [parameters] - Eg. defaultroute debug dump local notty usepeerdns
>
> adb sync notes: adb sync [ \<directory\> ]  
>   \<localdir\> can be interpreted in several ways:
>
>   - If \<directory\> is not specified, both /system and /data
> partitions will be u  
> pdated.
>
>   - If it is "system" or "data", only the corresponding partition  
>     is updated.
>
> environmental variables:  
>   ADB\_TRACE                    - Print debug information. A comma
> separated list  
>  of the following values  
>                                  1 or all, adb, sockets, packets, rwx,
> usb, sync  
> , sysdeps, transport, jdwp  
>   ANDROID\_SERIAL               - The serial number to connect to. -s
> takes prior  
> ity over this if given.  
>   ANDROID\_LOG\_TAGS             - When used with the logcat option,
> only these de  
> bug tags are printed.
>
> D:\\Android\\apk\>

* * * * *

-   輸入「adb devices」列出所有模擬器代號，查看手機是否有正確連接

[![image](http://lh4.ggpht.com/-vDtsp3AWOQI/TkIAAkLvzMI/AAAAAAAAK3U/_re0K0lPS1U/image_thumb%25255B8%25255D.png?imgmax=800 "image")](http://lh6.ggpht.com/-9aUExjO8DDA/TkIAAMn_SSI/AAAAAAAAK3Q/WUKvqPj_p9g/s1600-h/image%25255B16%25255D.png)

* * * * *

-   輸入「adb
    shell」進入手機中開始下指令(把它想成手機中也有類似「命令提示字元」的環境)

[![image](http://lh3.ggpht.com/-JJZCtLEHk1U/TkIABrZuc0I/AAAAAAAAK3c/Ov-UxN1Ak_8/image_thumb%25255B13%25255D.png?imgmax=800 "image")](http://lh6.ggpht.com/-DtYMxdG_E50/TkIABLpfutI/AAAAAAAAK3Y/EwLmViuBFog/s1600-h/image%25255B29%25255D.png)

adb shell

也可以執行各種Linux的命令，其命令格式為：adb shell command

PS: 當 adb shell 之後提示字元為"\#"時，表示使用者為 root
(最大權限)，若是 "\$" 則是以shell 權限工作

 

adb shell ls 就是列出目錄

adb shell dmesg 會列印出Linux kernel log

adb shell cat /proc/kmsg 持續印出 kernel log (需要 root)

adb shell keyevent 1 輸入 keyevent，可輸入的內容參考 adb shell keyevent

> 當機器的 input device(touch panel, keypad) 失效時，可利用 adb
> 輸入keyevent，  
> 在開發階段也常用來驗證一些 input 方面的問題
>
> adb shell input keyevent 7    \# for key '0'  
> adb shell input keyevent 8    \# for key '1'  
> adb shell input keyevent 29    \# for key 'A'  
> adb shell input keyevent 54    \# for key 'B'
>
>   
> 輸入字串
>
> adb shell input text "ANDROID"
>
>    
> Keycode 列表  
> 00 -\>  "KEYCODE\_UNKNOWN"  
> 01 -\>  "KEYCODE\_MENU"  
> 02 -\>  "KEYCODE\_SOFT\_RIGHT"  
> 03 -\>  "KEYCODE\_HOME"  
> 04 -\>  "KEYCODE\_BACK"  
> 05 -\>  "KEYCODE\_CALL"  
> 06 -\>  "KEYCODE\_ENDCALL"  
> 07 -\>  "KEYCODE\_0"  
> 08 -\>  "KEYCODE\_1"  
> 09 -\>  "KEYCODE\_2"  
> 10 -\>  "KEYCODE\_3"  
> 11 -\>  "KEYCODE\_4"  
> 12 -?  "KEYCODE\_5"  
> 13 -\>  "KEYCODE\_6"  
> 14 -\>  "KEYCODE\_7"  
> 15 -\>  "KEYCODE\_8"  
> 16 -\>  "KEYCODE\_9"  
> 17 -\>  "KEYCODE\_STAR"  
> 18 -\>  "KEYCODE\_POUND"  
> 19 -\>  "KEYCODE\_DPAD\_UP"  
> 20 -\>  "KEYCODE\_DPAD\_DOWN"  
> 21 -\>  "KEYCODE\_DPAD\_LEFT"  
> 22 -\>  "KEYCODE\_DPAD\_RIGHT"  
> 23 -\>  "KEYCODE\_DPAD\_CENTER"  
> 24 -\>  "KEYCODE\_VOLUME\_UP"  
> 25 -\>  "KEYCODE\_VOLUME\_DOWN"  
> 26 -\>  "KEYCODE\_POWER"  
> 27 -\>  "KEYCODE\_CAMERA"  
> 28 -\>  "KEYCODE\_CLEAR"  
> 29 -\>  "KEYCODE\_A"  
> 30 -\>  "KEYCODE\_B"  
> 31 -\>  "KEYCODE\_C"  
> 32 -\>  "KEYCODE\_D"  
> 33 -\>  "KEYCODE\_E"  
> 34 -\>  "KEYCODE\_F"  
> 35 -\>  "KEYCODE\_G"  
> 36 -\>  "KEYCODE\_H"  
> 37 -\>  "KEYCODE\_I"  
> 38 -\>  "KEYCODE\_J"  
> 39 -\>  "KEYCODE\_K"  
> 40 -\>  "KEYCODE\_L"  
> 41 -\>  "KEYCODE\_M"  
> 42 -\>  "KEYCODE\_N"  
> 43 -\>  "KEYCODE\_O"  
> 44 -\>  "KEYCODE\_P"  
> 45 -\>  "KEYCODE\_Q"  
> 46 -\>  "KEYCODE\_R"  
> 47 -\>  "KEYCODE\_S"  
> 48 -\>  "KEYCODE\_T"  
> 49 -\>  "KEYCODE\_U"  
> 50 -\>  "KEYCODE\_V"  
> 51 -\>  "KEYCODE\_W"  
> 52 -\>  "KEYCODE\_X"  
> 53 -\>  "KEYCODE\_Y"  
> 54 -\>  "KEYCODE\_Z"  
> 55 -\>  "KEYCODE\_COMMA"  
> 56 -\>  "KEYCODE\_PERIOD"  
> 57 -\>  "KEYCODE\_ALT\_LEFT"  
> 58 -\>  "KEYCODE\_ALT\_RIGHT"  
> 59 -\>  "KEYCODE\_SHIFT\_LEFT"  
> 60 -\>  "KEYCODE\_SHIFT\_RIGHT"  
> 61 -\>  "KEYCODE\_TAB"  
> 62 -\>  "KEYCODE\_SPACE"  
> 63 -\>  "KEYCODE\_SYM"  
> 64 -\>  "KEYCODE\_EXPLORER"  
> 65 -\>  "KEYCODE\_ENVELOPE"  
> 66 -\>  "KEYCODE\_ENTER"  
> 67 -\>  "KEYCODE\_DEL"  
> 68 -\>  "KEYCODE\_GRAVE"  
> 69 -\>  "KEYCODE\_MINUS"  
> 70 -\>  "KEYCODE\_EQUALS"  
> 71 -\>  "KEYCODE\_LEFT\_BRACKET"  
> 72 -\>  "KEYCODE\_RIGHT\_BRACKET"  
> 73 -\>  "KEYCODE\_BACKSLASH"  
> 74 -\>  "KEYCODE\_SEMICOLON"  
> 75 -\>  "KEYCODE\_APOSTROPHE"  
> 76 -\>  "KEYCODE\_SLASH"  
> 77 -\>  "KEYCODE\_AT"  
> 78 -\>  "KEYCODE\_NUM"  
> 79 -\>  "KEYCODE\_HEADSETHOOK"  
> 80 -\>  "KEYCODE\_FOCUS"  
> 81 -\>  "KEYCODE\_PLUS"  
> 82 -\>  "KEYCODE\_MENU"  
> 83 -\>  "KEYCODE\_NOTIFICATION"  
> 84 -\>  "KEYCODE\_SEARCH"  
> 85 -\>  "TAG\_LAST\_KEYCODE"

* * * * *

-   如果在開啟了超過一個模擬器的狀態下，必須指定要針對哪個模擬器  
    adb -s emulator-5554 shell

[![image](http://lh5.ggpht.com/-UaVd4pjxcCE/TkIACf8FpFI/AAAAAAAAK3k/aFF6cf_tPxI/image_thumb%25255B23%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-qQXbdVu1g-0/TkIACNddu1I/AAAAAAAAK3g/BMRYBmZ8_QQ/s1600-h/image%25255B47%25255D.png)

\$adb devices (顯示目前有多少個模擬器正在執行)  
\$adb -s \<serialNumber\> \<command\> (指定模擬器來操作)  
adb -s emulator-5554 install email.apk

* * * * *

-   輸入「exit」進即可回cmd

[![image](http://lh3.ggpht.com/-aCB1md798Pg/TkIADfU_HkI/AAAAAAAAK3s/8xjnHHeG2Ls/image_thumb%25255B22%25255D.png?imgmax=800 "image")](http://lh3.ggpht.com/-ml_smxnLQt0/TkIACzQNgKI/AAAAAAAAK3o/4IkvUxf_chI/s1600-h/image%25255B46%25255D.png)

* * * * *

-   把檔案放到 emu 的 sdcard 或系統目錄

adb push "Sleep Away.mp3" "/sdcard/my\_song.mp3"

[![image](http://lh6.ggpht.com/-EdtyrklQF3M/TkIAERwiitI/AAAAAAAAK30/l26wTgRCWyQ/image_thumb%25255B15%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-HMgm5yqB3mg/TkIAD4ImbkI/AAAAAAAAK3w/AR9bfhjYWLg/s1600-h/image%25255B33%25255D.png)

檔案進去了

[![image](http://lh3.ggpht.com/-D8SshDQX2X4/TkIAFdA3EmI/AAAAAAAAK38/GaO0M37kYFM/image_thumb%25255B21%25255D.png?imgmax=800 "image")](http://lh3.ggpht.com/-DSv371fE7VU/TkIAE32ZLhI/AAAAAAAAK34/txrCuTRCxnc/s1600-h/image%25255B45%25255D.png)

adb push 001.jpg /sdcard (複製檔案到 /sdcard 目錄下)  
adb push pictures /sdcard (複製 picture 照片目錄到 /sdcard 目錄下)  
adb push mp3 /sdcard (複製 mp3 音樂目錄到 /sdcard 目錄下)  
adb shell (Android 模擬器啟動命令列模式)  
\#cd /sdcard (進入 /sdcard 目錄)  
\#ls (查看 SD 記憶卡中的檔案)

* * * * *

-   從 emu 把檔案 copy出來

adb pull "/sdcard/my\_song.mp3" "my\_song.mp3"

[![image](http://lh5.ggpht.com/-BNbMrz--3pw/TkIAGBbq-bI/AAAAAAAAK4E/ucHI17ZFRnw/image_thumb%25255B18%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-yErdzTO4YLw/TkIAF0COwuI/AAAAAAAAK4A/TLZkFwbWIqo/s1600-h/image%25255B40%25255D.png)

adb pull /sdcard/001.jpg . (下載 /sdcard 目錄下的檔案)  
adb pull /sdcard/pictures . (下載 sdcard 目錄下的 pictures 目錄)

* * * * *

-   安裝 apk 到 emu 上  
    adb install ES.FileExplorer\_1.4.8.9.apk

[![image](http://lh6.ggpht.com/-ZAp_r75mfUY/TkIAHcHuE0I/AAAAAAAAK4M/rlbDpgmY_EE/image_thumb%25255B20%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-0boim5tb5j8/TkIAGwC2rzI/AAAAAAAAK4I/9Tv2qdePmwI/s1600-h/image%25255B44%25255D.png)

就安裝了ES.FileExplorer\_1.4.8.9

[![image](http://lh5.ggpht.com/-9Y3YpjvBPNg/TkIAIoko2yI/AAAAAAAAK4U/0PDVdiizfPw/image_thumb%25255B26%25255D.png?imgmax=800 "image")](http://lh6.ggpht.com/-v5XZ9yM5QaY/TkIAHyKMbVI/AAAAAAAAK4Q/9YArmy0lvEE/s1600-h/image%25255B52%25255D.png)

adb install filename.apk (安裝filename.apk)  
adb install -r filename.apk (保留已設定資料，重新安裝filename.apk)  
adb -s emulator-5554 install filename.apk (指定安裝 APK 套件在 5554 的
Android 模擬器中)

安裝參數

"-r": 當已經安裝過舊版本的程式時，可以使用 -r 去覆蓋。

"-f": 強制安裝，通常在安裝程式時會遇到相容問題，可使用此參數解決。

* * * * *

-   移除 APK 應用程式

  
可以先到/data/data或data/app目錄下，查詢想移除的package名稱  
adb shell  
ls /data/data 或 /data/app (查詢 Package 名稱)  
exit

[![image](http://lh6.ggpht.com/-sEtPPhC8uGA/TkIAJyjMMKI/AAAAAAAAK4c/PNyFU77MeB8/image_thumb%25255B30%25255D.png?imgmax=800 "image")](http://lh5.ggpht.com/-iI5zJephPc4/TkIAJP6e7SI/AAAAAAAAK4Y/cN1I_EGOf9E/s1600-h/image%25255B60%25255D.png)

adb uninstall package (移除查詢到的 Package名稱)  
adb uninstall -k package (移除程式時，保留資料)  
此package名稱不是安裝APK套裝時的檔名或顯示在模擬器中的應用程式名稱

[![image](http://lh6.ggpht.com/-ziyffF3zPHA/TkIAK0qYPQI/AAAAAAAAK4k/n2dKL7dOD80/image_thumb%25255B29%25255D.png?imgmax=800 "image")](http://lh3.ggpht.com/--wWl3TF1nPA/TkIAKQ-zuUI/AAAAAAAAK4g/HooHKKMxPDM/s1600-h/image%25255B59%25255D.png)

* * * * *

-   adb shell 進入 Android 系統指令列模式常用指令

 

\$adb shell (進入 Android 系統指令列模式)  
\$ls  
\$dmesg (查看 Android Linux Kernel 運作訊息)  
ls - 顯示檔案目錄  
cd - 進入目錄  
rm - 刪除檔案  
mv - 移動檔案  
mkdir - 產生目錄  
rmdir - 刪除目錄

\$adb push \<file/dir\> (複製檔案到 SD 卡)  
adb push mp3 /sdcard  
\$adb pull \<file/dir\> . (從 Android 系統下載檔案)  
adb pull /data/app/com.android.email  
\$adb logcat (監控模擬器運作紀錄，以Ctrl + c 離開監控模式)  
\$adb bugreport (產生 adb 除錯報告)  
\$adb get-state (獲得 adb 伺服器運作狀態)  
\$adb start-server (啟動 adb 伺服器)  
\$adb kill-server (關掉 adb 伺服器)  
\$adb forward tcp:6100 tcp:7100 (更改模擬器網路 TCP 通訊埠)  
\$adb shell ps -x (顯示 Android 上所有正在執行的行程)  
\$adb version (顯示 adb 版本)  
\$adb help (顯示 adb 指令參數)

 

adb get-product 獲取設備型號  
adb get-serialno 獲取序列號

* * * * *

使用mksdcard指令模擬1GB的記憶卡  
mksdcard 1024M sacard.img

[![image](http://lh3.ggpht.com/-T321Xl1mFIU/TkIcEFDAvfI/AAAAAAAAK4s/eIKMPrz5B6g/image_thumb%25255B32%25255D.png?imgmax=800 "image")](http://lh6.ggpht.com/-nC2mrBdfkxY/TkIcDrY1U_I/AAAAAAAAK4o/O3gNlz2a-e0/s1600-h/image%25255B64%25255D.png)

模擬插入 SD 卡的模擬器  
emulator -sdcard sdcard.img

* * * * *

\# Emulator 命令列啟動參數  
emulator -timezone Asia/Taipei (指定時區)  
emulator -no-boo-anim (省略開機小機器人動畫畫面)  
emulator -scale auto (調整模擬器視窗大小)  
emulator - scale factor (factor: 0.1-3.0)  
emulator -dpi-device 300 (更改模擬器的解析度，default為 165dpi)  
emulator -skin \<skinID\> (更改模擬器顯示模式)  
emulator -help-keys (顯示鍵盤快速鍵說明)  
emulator -shell (相當於adb shell 功能)  
emulator -data data.img (使 /data 目錄使用 data.img 的檔案空間)  
emulator -sdcard sdcard.img (使 /sdcard 目錄使用 sdcard.img
的檔案空間)  
emulator -cache cache.img (瀏覽器暫存檔儲存空間)  
emulator -wipe-data (使模擬器恢復到原廠設定)  
emulator -help (顯示 emulator 指令參數)

 

\# Android模擬器命令列啟動模式  
在android-sdk-windows-1.1\\tools執行emulator以執行模擬器  
加上-skin參數，指定顯示模式為HVGA-L，則可轉為橫向  
emulator - skin HVGA-L (480\*320，水平顯示)  
emulator - skin HVGA-L (320\*480，垂直顯示，模擬器預設模式)  
emulator - skin HVGA-L (320\*240，水平顯示)  
emulator - skin HVGA-L (240\*320，垂直顯示)

* * * * *

其他：

1. 切換 Layout為 Landscape or Protrait: Ctrl + F11 or Ctrl + F12

2. 模擬網路 ON/OFF: F8

3. 模擬有電話打進來的情形: 開兩個模擬器即可互打，電話號碼就是模擬器上的
5554, 5556 etc。

* * * * *

參考：

-   官方的說明文件：
    <http://developer.android.com/intl/zh-TW/guide/developing/tools/adb.html>
-   <http://eric1300460.pixnet.net/blog/post/30372232>
-   <http://blog.aztaru.com/2010/02/24/%E7%AD%86%E8%A8%98android-emulator%E5%B8%B8%E7%94%A8%E6%8C%87%E4%BB%A4%E8%88%87%E6%8A%80%E5%B7%A7/>
-   <http://gfans.bryan.tw/2010/11/30/1361>
-   <http://huenlil.pixnet.net/blog/post/23271843>

