---
layout: post
title: 'C# 將 HTML 轉 PDF'
author: 'James Peng'
tags: ['C# 3rd-Party Library']
---

C# 將 HTML 轉 PDF 可以透過第三方套件 NReco.PdfGenerator 簡單辦到，只需要一行code

## 下載套件 並 參考 ##

![](..\images\2016-04-03-CSharp_HtmlToPdf\ynpptPk.png)

https://pdfgenerator.codeplex.com/


![](..\images\2016-04-03-CSharp_HtmlToPdf\DDamu2W.png)

## 使用方法 ##

嘗試去讀取 <span style="color:red;">Yahoo首頁</span> 轉成 PDF 

~~~csharp
            new NReco.PdfGenerator.HtmlToPdfConverter().
            GeneratePdfFromFile(@"https://tw.yahoo.com/", null,
            AppDomain.CurrentDomain.BaseDirectory + "output.pdf");
~~~

## 結果:  ##

非常真神奇，這樣就完工了...。

![](..\images\2016-04-03-CSharp_HtmlToPdf\yP8L45s.png)


## 參考： ##

- https://pdfgenerator.codeplex.com/
- http://www.nrecosite.com/pdf_generator_net.aspx
- https://www.nuget.org/packages/NReco.PdfGenerator/
- http://no2don.blogspot.com/2016/02/c-nrecopdfgenerator-htmlpdf.html#more
