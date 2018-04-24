---
layout: post
title: 'C# 使用 EF6 於 SQLite (VS2015 .net 4.6.2)'
author: 'James Peng'
tags: ['Entity Framework']
---

開發環境:
- Visual Studio 2015 x86
- .Net Framework 4.6.2 x86
- 資料庫：SQlite x86

## 下載 ##

先到 SQLite 官方網站 https://system.data.sqlite.org/index.html/doc/trunk/www/downloads.wiki

## 下載 Setups for 32-bit Windows (.NET Framework 4.6) ##

![](..\images\2017-07-20-CSharp_EF6_SQLite_VS2015\dZ0QZ39.png)

- [sqlite-netFx46-setup-bundle-x86-2015-1.0.105.2.exe (16.94 MiB)](https://system.data.sqlite.org/downloads/1.0.105.2/sqlite-netFx46-setup-bundle-x86-2015-1.0.105.2.exe)

----------

## 安裝 Setups for 32-bit Windows ##

![](..\images\2017-07-20-CSharp_EF6_SQLite_VS2015\3AIWrA0.png)

![](..\images\2017-07-20-CSharp_EF6_SQLite_VS2015\eTtv6ig.png)

![](..\images\2017-07-20-CSharp_EF6_SQLite_VS2015\YPjHfFX.png)

![](..\images\2017-07-20-CSharp_EF6_SQLite_VS2015\pUXS647.png)

![](..\images\2017-07-20-CSharp_EF6_SQLite_VS2015\sBtLYBI.png)

這裡要等幾分鐘，請耐心等候，等到天荒地老

![](..\images\2017-07-20-CSharp_EF6_SQLite_VS2015\027j5M4.png)

完成


----------

## Database First 建立 ADO.NET 實體資料模型 ##

我建立了一個專案 使用 .net Framework 4.6.2

![](..\images\2017-07-20-CSharp_EF6_SQLite_VS2015\xdf4CZG.png)

在專案裡安裝 sqlite

~~~text
Install-Package System.Data.SQLite
~~~

![](..\images\2017-07-20-CSharp_EF6_SQLite_VS2015\kwGoTSC.png)

新增一個 「ADO.NET 實體資料模型」，名稱輸入 「model資料表名稱」 ，按下 「新增」

![](..\images\2017-07-20-CSharp_EF6_SQLite_VS2015\WYTxpx8.png)


選擇 「來自資料庫的 EF Designer 中模型」

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\CBK3IeM.png)


![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\v8FhRZF.png)


資料來源 選 sqlite

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\63kFWFo.png)


按下 「Browser」 選取 sqlite 資料庫，測試連接成功，按下 「確定」

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\1QSy94d.png)

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\oTje59C.png)

![](..\images\2016-05-23-CSharp_EF6_SQLite_VS2013\sJ38t5V.png)

ADO.NET 實體資料模型 建立完成

![](..\images\2017-07-20-CSharp_EF6_SQLite_VS2015\PcrwI03.png)

專案裡 就多出 EDMX 檔案

----------


## 撈出資料測試 ##

~~~csharp
        private void button1_Click(object sender, EventArgs e)
        {
            
            using (schemaEntities1 db = new schemaEntities1())
            {
                dataGridView1.DataSource = db.DatabaseInfo.ToList();
            }
        }
~~~


![](..\images\2017-07-20-CSharp_EF6_SQLite_VS2015\AHOOaio.png)


## 參考： ##

- https://system.data.sqlite.org/index.html/doc/trunk/www/downloads.wiki
- https://www.nuget.org/packages/System.Data.SQLite/
