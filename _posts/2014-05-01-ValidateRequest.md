---
layout: post
title: 'ASP.NET 出現「具有潛在危險 Request.Form 的值已從用戶端偵測到」錯誤解法'
author: 'James Peng'
tags: ['ASP.NET']
---

ASP.NET 出現「具有潛在危險 Request.Form 的值已從用戶端偵測到」錯誤解法

## 在asp.net 4.0以前(不含4.0)的版本 ##

1. 將該網頁加上ValidateRequest="false"屬性

## 在asp.net 4.0以上(含4.0)的版本二個步驟： ##

1. 將該網頁加上ValidateRequest="false"屬性
2. 在Web.config中


~~~csharp
    <system.web>
      <httpRuntime  requestValidationMode="2.0" />
    </system.web>
~~~


----------

參考：

- http://infofabwhat.blogspot.tw/2012/05/c-requestform.html
- http://samwang823.blogspot.com/2010/01/aspnet-40-request-validation.html 
- http://www.dotblogs.com.tw/pin0513/archive/2010/10/22/18522.aspx
