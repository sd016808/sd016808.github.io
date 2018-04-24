---
layout: post
title: 'ASP.NET:在虛擬主機中使用未安裝字型，C# 使用私有字型'
author: 'James Peng'
tags: ['ASP.NET']
---


在虛擬主機中想用非系統字型（不存在於 C:\WINNT\Fonts 中）

轉成圖片並顯示出來？


可以用System.Drawing.Text.PrivateFontCollection創建自己的私有字體


~~~csharp

//建立私有字型
PrivateFontCollection privateFontCollection = new PrivateFontCollection();

// Add three font files to the private collection.
privateFontCollection.AddFontFile(Server.MapPath("qqq.ttf"));
Font drawFont = new Font( privateFontCollection.Families[0], 30 );
~~~

----------

參考：

- http://www.blueshop.com.tw/board/show.asp?subcde=BRD20050223174559DPQ&fumcde=FUM20050124192253INM
- [HOW TO：建立私用字型集合 ](https://msdn.microsoft.com/zh-tw/library/y505zzfw(v=vs.100).aspx)
- 
