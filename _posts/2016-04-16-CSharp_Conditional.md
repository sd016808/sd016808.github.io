---
layout: post
title: 'C# 使用 Conditional 屬性設定 進行偵錯'
author: 'James Peng'
tags: ['C#']
---

## ＃if語法 ##

在專案開發初期，我們都會寫很多測試程式碼(Test Code)用來紀錄或顯示程式執行時期的狀態，雖然開發環境有中斷點 (Breakpoint) 可以使用，但程式部署到測試機或正式機時卻未必有開發工具可用，這時利用自己寫的測試程式碼就非常有用，但專案上線前若又需要把測試程式碼刪除頗為麻煩，今天我就打算分享一些很實務的偵錯開發技巧。

在 C# 中有個 #if 語法非常的實用

~~~csharp
#if DEBUG
            MessageBox.Show("YO~"); 
#endif
~~~

Solution Configuration (方案設置) 選單，預設就有 Debug 與 Release 兩個選項可用，當專案需要建置時可以利用這兩個選項進行快速切換，以產生不同的組件輸出。

![](..\images\2016-04-16-CSharp_Conditional\aut9QBc.png)

透過 #if 語法與 #endif 語法的包夾之後，你會發現在開發環境似乎沒什麼變化

![](..\images\2016-04-16-CSharp_Conditional\2U6mnCY.png)

選單切換至 Release 之後程式碼自動變「灰色」的了

![](..\images\2016-04-16-CSharp_Conditional\8r67uN3.png)

這也代表著切換至 Release 之後所建置 (Build) 出來的組件不會包含這些測試的程式碼。

----------


## Conditional 屬性設定 進行偵錯 ##

Conditional 屬性設定和 #if DEBUG 的方法有異曲同工之妙


## 命名空間 ##

~~~csharp
using System.Diagnostics;
~~~


## 增加一個Debug方法 ##

~~~csharp
        [Conditional("DEBUG")]
        private void Debug(string s)
        {
            MessageBox.Show(s);
        }
~~~


## 使用 ##

~~~csharp
        private void button1_Click(object sender, EventArgs e)
        {           
            Debug("DEBUG");
            MessageBox.Show("123");
        }
~~~

加了[Conditional("DEBUG")]屬性的方法

在Release的時候就不會呼叫他了

(正確一點說就是編譯的時候會自動忽略這個方法的呼叫, 不過方法本身還是會被編譯進組件裡)

## DEBUG 運行結果 ##

![](..\images\2016-04-16-CSharp_Conditional\Pdfu71U.png)

![](..\images\2016-04-16-CSharp_Conditional\JHkQhpO.png)

## Release 運行結果 ##

![](..\images\2016-04-16-CSharp_Conditional\RpOYQLO.png)

----------


## 參考： ##

- https://msdn.microsoft.com/zh-tw/library/aa664622%28VS.71%29.aspx?f=255&MSPPError=-2147217396
- http://blog.miniasp.com/post/2009/09/16/How-to-utilize-Debug-mode-to-develop-aspnet-and-other-net-applications.aspx
- https://dotblogs.com.tw/sam319/2010/06/15/15908
