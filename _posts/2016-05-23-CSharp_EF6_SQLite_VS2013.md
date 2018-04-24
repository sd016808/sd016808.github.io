---
layout: post
title: 'C# 使用 EF6 於 SQLite (VS2013 .net 4.0)'
author: 'James Peng'
tags: ['Entity Framework']
---

開發環境:
- Visual Studio 2013 x86
- .Net Framework 4.0 x86
- 資料庫：SQlite x86

## 下載 ##

先到 SQLite 官方網站 https://system.data.sqlite.org/index.html/doc/trunk/www/downloads.wiki

1. 下載 Setups for 32-bit Windows (.NET Framework 4.5.1)
- [sqlite-netFx451-setup-bundle-x86-2013-1.0.101.0.exe (10.31 MiB)](https://system.data.sqlite.org/downloads/1.0.101.0/sqlite-netFx451-setup-bundle-x86-2013-1.0.101.0.exe)

2. 下載 [Entity Framework 6 Tools for Visual Studio 2012 & 2013](https://www.microsoft.com/en-us/download/details.aspx?id=40762) 



----------

## 安裝 Setups for 32-bit Windows ##

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\SxiFeIz.png)

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\zUpUoea.png)

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\gN9gW3f.png)

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\JzwDr2D.png)

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\r5Hx2SJ.png)

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\MzfJBeY.png)

這裡要等幾分鐘，請耐心等候，等到天荒地老

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\Zmncrhu.png)

完成


----------

## 安裝 Entity Framework 6 Tools for Visual Studio 2012 & 2013 ##

還有一個

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\ZLuoCm1.png)

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\AFTO3pk.png)

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\QgluGX1.png)


----------

## Database First 建立 ADO.NET 實體資料模型 ##

我建立了一個專案 使用 .net Framework 4 (為了相容xp)

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\H30B3SC.png)

先安裝 sqlite

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\RRL2tDW.png)

新增一個 「ADO.NET 實體資料模型」，名稱輸入 「model資料表名稱」 ，按下 「新增」

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\98lpWUs.png)


選擇 「來自資料庫的 EF Designer 中模型」

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\CBK3IeM.png)


![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\v8FhRZF.png)


資料來源 選 sqlite

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\63kFWFo.png)


按下 「Browser」 選取 sqlite 資料庫，測試連接成功，按下 「確定」

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\1QSy94d.png)

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\oTje59C.png)

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\sJ38t5V.png)

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\2jfLBLJ.png)

ADO.NET 實體資料模型 建立完成

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\j0X8QZl.png)

專案裡 就多出 EDMX 檔案

----------

## 參考： ##

- https://system.data.sqlite.org/index.html/doc/trunk/www/downloads.wiki
- http://www.petekcchen.com/2013/08/working-with-sqlite-on-entity-framework.html
- https://msdn.microsoft.com/zh-tw/library/bb386870(v=vs.90).aspx
