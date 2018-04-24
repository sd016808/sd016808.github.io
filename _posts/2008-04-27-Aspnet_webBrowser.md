---
layout: post
title: 'ASP.NET 使用 webBrowser 錯誤'
author: 'James Peng'
tags: ['ASP.NET']
---

![](..\images\2008-04-27-Aspnet_webBrowser\B5hoO6F.png)


## 錯誤訊息： ##

    無法產生 ActiveX 控制項 '8856f961-340a-11d0-a96b-00c04fd705a2'，因為目前的執
    行緒不是在單一執行緒 Apartment。
    描述: 在執行目前 Web 要求的過程中發生未處理的例外情形。請檢閱堆疊追蹤以取得錯
    誤的詳細資訊，以及在程式碼中產生的位置。
    
    例外詳細資訊: System.Threading.ThreadStateException: 無法產生 ActiveX 控制項
    '8856f961-340a-11d0-a96b-00c04fd705a2'，因為目前的執行緒不是在單一執行緒
    Apartment。
    
    
    [ThreadStateException: 無法產生 ActiveX 控制項
    '8856f961-340a-11d0-a96b-00c04fd705a2'，因為目前的執行緒不是在單一執行緒
    Apartment。]
    System.Windows.Forms.WebBrowserBase..ctor(String clsidString) +3653451
    System.Windows.Forms.WebBrowser..ctor() +54


----------

## 解決方法： ##

ASP.NET 應用程式應該將 @ Page 指示詞的 ASPCompat 屬性設定為 true，強迫網頁由 STA 執行緒集區服務。


~~~csharp
<%@ Page Language="C#" AutoEventWireup="true" CodeBehind="xxx.aspx.cs" Inherits="xxx.xxx" AspCompat="true" %>
~~~

