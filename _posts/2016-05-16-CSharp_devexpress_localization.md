---
layout: post
title: 'C# DevExpress LOCALIZATION SERVICE 本地化服務 Building Your Custom Assemblies'
author: 'James Peng'
tags: ['DevExpress']
---

## 問題 ##

使用 DevExpress 套件，建立預覽列印功能如下

語系切為 zh-TW 台灣

![](..\images\2016-05-16-CSharp_devexpress_localization\mWw46yK.png)

點選 列印後，跳出 第三方的列印介面

![](..\images\2016-05-16-CSharp_devexpress_localization\meYLriC.png)

發現某些套件內建的介面都是英文的

## 解法 ##

查了一下發現網路上有抓到

- DevExpressLocalizedResources_2015.2_zh-CN.zip
- DevExpressLocalizedResources_2015.2_zh-TW.zip

哪裡來的呢？

先註冊 DevExpress 網站

https://localization.devexpress.com/

接著 按下按鈕 「Add Translation」

![](..\images\2016-05-16-CSharp_devexpress_localization\2OvPokd.png)

選擇「版本」和「語系」

![](..\images\2016-05-16-CSharp_devexpress_localization\rC6ClsH.png)

接著按下 「modify」 

![](..\images\2016-05-16-CSharp_devexpress_localization\nVe417S.png)

例如我們要翻譯 「Printer name:」

![](..\images\2016-05-16-CSharp_devexpress_localization\UrrG7eb.png)


.NET PLATFORM 選擇 WinForms (不知道的話，選 All)， DISPLAY選 還沒翻譯過的 Non Translated，

接著搜尋框輸入 「Printer name:」 (或是輸入元件名稱 例如 PdfViewer)

然後可以參考建議翻譯 Suggested Translation 來翻譯，按下 「箭頭按鈕」 後， 按下 「Save」 就完成了。 

![](..\images\2016-05-16-CSharp_devexpress_localization\eA7q7OJ.png)

依此類推

全部改好後，按下 「Download」 會把dll 編譯好 寄到你的註冊信箱

![](..\images\2016-05-16-CSharp_devexpress_localization\KUwSqDy.png)

接著，就把不同語系 dll 解壓在 bin 裡 Debug 或 Release 對應的目錄下

DevExpressLocalizedResources_2015.2_zh-CN.zip 就放在目錄 「zh-CN」 裡

DevExpressLocalizedResources_2015.2_zh-TW.zip 就放在目錄 「zh-TW」 裡



----------

## 結果 ##

zh-TW

![](..\images\2016-05-16-CSharp_devexpress_localization\SoXspuv.png)

zh-CN

![](..\images\2016-05-16-CSharp_devexpress_localization\dC6ZGvb.png)

----------

## 參考: ##

- https://documentation.devexpress.com/#LocalizationService/CustomDocument16235
- http://localization.devexpress.com/
- https://www.evget.com/product/3269
- https://www.evget.com/article/2013/1/10/18412.html
- 
