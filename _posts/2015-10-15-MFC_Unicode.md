---
layout: post
title: 'MFC Unicode'
author: 'James Peng'
tags: ['Visual C++']
---

## 使用非Unicode開發的程式 ##

使用Win7正體中文作業系統，執行日文軟體的畫面，都是亂碼。

![](..\images\2015-10-15-MFC_Unicode\gE2jcvV.jpg)

整個功能表都是亂碼。

![](..\images\2015-10-15-MFC_Unicode\hNMRfV4.jpg)

使用Win7正體中文作業系統，執行大陸的軟體，畫面顯示的不是簡體而是亂碼。

![](..\images\2015-10-15-MFC_Unicode\HjbntTu.png)

同樣的外國人用英文版的作業系統，執行我們台灣的報稅系統，同樣是亂碼。

![](..\images\2015-10-15-MFC_Unicode\VcgP2wI.png)

國產軟體，也同樣是亂碼。

![](..\images\2015-10-15-MFC_Unicode\LSvkLFn.jpg)

-----------------

## 非Unicode開發的程式，沒救了？ ##

因為開發者不懂，只好讓使用者自救安裝 Microsoft AppLocale 來解決亂碼的問題。

下載點：http://www.microsoft.com/zh-tw/download/details.aspx?id=13209

![](http://i.imgur.com/w7Hm1Dq.gif)

每支程式透過 Microsoft AppLocale 來開啟

![](..\images\2015-10-15-MFC_Unicode\HjbntTu.png)

就變成

![](..\images\2015-10-15-MFC_Unicode\gadEZkn.png)

但也有失敗的機率，是個治標不治本的方法。


-----------------

## Unicode簡介 ## 


| 古代 | 現代 |
| -- | -- |
| ASCII字元集 | Unicode字元集 |
| 常規字元集 | 寬字元集 |
| 8位元寬 | 16位元寬 |
| ASCII的擴展自身定義字元非常雜亂 | 只有一個字元集 |
| 佔用記憶體小 | Unicode字串佔用的記憶體是ASCII字串的兩倍 |


PS.寬字元不一定是Unicode。Unicode是一種寬字元集。然而，因為本文的焦點是MFC而不是理論，所以我將把寬字元和Unicode作為同義語。


-----------------


## 請使用Unicode開發的程式  ##


您還可在字串前面使用L字首，來表示它們應解釋為寬字元。如下所示：

~~~cpp
CString str = L"Hello!";
~~~

在第一個引號前面的大寫字母L（代表「long」）。這將告訴編譯器該字串按寬字元保存，即每個字元佔用2個位元組。


也可以用_T來保證相容性，MFC 支援 ASCII 和 Unicode 兩種字元類型，用 _T 可以保證從 ASCII 編碼類型轉換到 Unicode 編碼類型的時候，程式不需要修改。

~~~cpp
CString str = _T("Hello!");
~~~

它不是函數, 它是一個 Windows 的 macro, 隸屬於 generic-text mapping 的範圍，如果是個 Unicode 的 project, _T("") 會變成 L""

~~~cpp
m_Status = L"";
~~~

如果不是 Unicode 的 project, _T("") 會變成 "", 所以整個句子:

~~~cpp
m_Status = ""; 
~~~

前者是一個 Unicode 的 (空) 字串.


-----------------

## 延伸閱讀：##

* http://msdn2.microsoft.com/en-us/library/szdfzttz.aspx
* http://msdn2.microsoft.com/en-us/library/c426s321.aspx
