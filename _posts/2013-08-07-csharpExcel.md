---
layout: post
title: 'C# 輸出 Excel 方案選擇'
author: 'James Peng'
tags: ['WinForm']
---

## 1.使用 Microsoft.Office.Interop.Excel ##

使用內建Microsoft.Office.Interop.Excel命名空間，電腦裡要先安裝office

![](..\images\2013-08-07-csharpExcel\zeONwkE.png)


使用open source 的NPOI Library 免安裝office

官網：[http://npoi.codeplex.com/](http://npoi.codeplex.com/)

![](..\images\2013-08-07-csharpExcel\6Mr0E53.png)

![](..\images\2013-08-07-csharpExcel\yFcpjQS.png)

測試產生excel：

<script src="https://gist.github.com/jia-hong-peng/e4994ee9a364308942bd.js"></script>

![](..\images\2013-08-07-csharpExcel\A6xPoTY.png)

![](..\images\2013-08-07-csharpExcel\056C1ob.png)

產生成功。



----------

## 2.使用 NPIO ##

測試NPOI套現有 EXCEL表

![](..\images\2013-08-07-csharpExcel\zRvkoFC.png)

![](..\images\2013-08-07-csharpExcel\ovDZsWl.png)


只支援2007版的.xlsx
只支援2003版的 .xls

![](..\images\2013-08-07-csharpExcel\3fXDh1t.png)


<script src="https://gist.github.com/jia-hong-peng/7bdfd42744ba7b440b7c.js"></script>


----------


## 3.用open xml sdk ##

改用open xml sdk

![](..\images\2013-08-07-csharpExcel\sIr0PHC.png)

![](..\images\2013-08-07-csharpExcel\1gm3hhA.png)

Include dll找 C:\Program Files\Open XML SDK\V2.5\lib

![](..\images\2013-08-07-csharpExcel\rIzwIRv.png)

    using DocumentFormat.OpenXml;
    using DocumentFormat.OpenXml.Packaging;
    using DocumentFormat.OpenXml.Spreadsheet;


![](..\images\2013-08-07-csharpExcel\ggv48UM.png)

![](..\images\2013-08-07-csharpExcel\4c9ceqD.png)

![](..\images\2013-08-07-csharpExcel\ZcShloM.png)

測試套表：

![](..\images\2013-08-07-csharpExcel\PRmlqNz.png)

修改 G8 為 failed

<script src="https://gist.github.com/jia-hong-peng/506bc6e05b880919ebb6.js"></script>


修改成功。

![](..\images\2013-08-07-csharpExcel\iNcotoy.png)


----------

## 4.Project Source code ##
[https://github.com/jia-hong-peng/ExcelSample](https://github.com/jia-hong-peng/ExcelSample)


----------

## 5.參考 ##
- [http://msdn.microsoft.com/zh-tw/ee818993.aspx](http://msdn.microsoft.com/zh-tw/ee818993.aspx)
- [http://i.imgur.com/yFcpjQS.png](http://i.imgur.com/yFcpjQS.png)
- [http://tonyqus.sinaapp.com/archives/545](http://tonyqus.sinaapp.com/archives/545)
- [http://www.dotblogs.com.tw/chou/archive/2010/04/29/14912.aspx](http://www.dotblogs.com.tw/chou/archive/2010/04/29/14912.aspx)
- [http://www.cnblogs.com/pszw/archive/2012/02/07/2334679.html](http://www.cnblogs.com/pszw/archive/2012/02/07/2334679.html)
- [http://www.cnblogs.com/yjmyzz/archive/2011/06/18/2084391.html](http://www.cnblogs.com/yjmyzz/archive/2011/06/18/2084391.html)
- [http://blog.darkthread.net/post-2012-12-27-rptviewer-xlsx-issue-epplus-npoi-and-openxml.aspx](http://blog.darkthread.net/post-2012-12-27-rptviewer-xlsx-issue-epplus-npoi-and-openxml.aspx)
