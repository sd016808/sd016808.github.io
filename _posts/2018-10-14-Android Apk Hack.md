---
layout: post
title: 'Android APK Modify'
author: 'Harris'
tags: ['Android']
---
這篇文章主要是紀錄修改Android APK的過程，以[超異域公主連結 Re:Dive](https://zh.wikipedia.org/wiki/%E8%B6%85%E7%95%B0%E5%9F%9F%E5%85%AC%E4%B8%BB%E9%80%A3%E7%B5%90_Re:Dive)這款遊戲為範例

事前準備工具：
1. APK Easy Tool: 用來Decompiled/Compiled/Signed APK檔案
2. Hxd: 用來檢視及修改遊戲資料(Hex Data)
3. IDA Pro v7.0: 用來檢視遊戲的IL程式碼
4. IL2CppDumper v3.3.13: 用來取得關鍵程式碼的Method Name以及對應的Address
5. ILSpy v4.0.0.4319: 用來查詢關鍵程式碼的Address(可不使用)
6. 任意一款Android遊戲的APK

[下載連結](https://drive.google.com/open?id=1ZcfmuYVa8QLl8rzZCn2FSx4CVSC6EzFq)

方便的網站

---
Step 1. 打開APK Easy Tool並選擇要修改的APK檔案

Step 2. 點選Decompiled解開APK的內容

Step 3. 點選Decompiled APK Directory打開解開的內容資料夾

Step 4. 複製global-metadata.dat以及libil2cpp.so到IL2CppDumper的根目錄下

Step 5. 打開Il2CppDumper.exe，並依序載入libil2cpp.so以及global-metadata.dat檔案

Step 6. 選擇Mode3 Auto(Advanced)，並等待Dump完成

Step 7. 完成後會在IL2CppDumper的根目錄下找到Dump.cs檔案，打開後可以看到一堆C#的程式碼

Step 8. 找到想要修改的程式碼後，記下對應的Address，如下圖：我們找到一個檢查是不是模擬器的判斷Method，對應的Address為0x21C254C

Step 9. 打開IDA Pro並載入APK，此時要等待一段很長的時間，等待IDA解析完成，建議暫時關掉左方的Function Window會加快解析的速度，解析完成後點選工具列的Windows，Reset desktop可以把Function Windows顯示回來

Steop 10. 


![](http://www.aspphp.online/bianchen/UploadFiles_4619/201701/2017011818474575.png)

i would like to disable the menu display on right click on column of grid control.

Here is some sample code:

~~~csharp
DataExchangeGridView.OptionsMenu.EnableColumnMenu = false;
~~~


