---
layout: post
title: 'ASP.NET:取得URL真實路徑'
author: 'James Peng'
tags: ['ASP.NET']
---



ASP.NET中獲取URL的方法

HttpContext.Current.Request.Url.ToString() 並不可靠。 

如果當前URL為 

    http://localhost/search.aspx?user=http://csharp.xdowns.com&amp;tag=%BC%BC%CA%F5 


通過HttpContext.Current.Request.Url.ToString()獲取到的卻是 


    http://localhost/search.aspxuser=http://csharp.xdowns.com&amp;tag=¼¼Êõ 


## 正確的方法是 ##
~~~csharp
HttpContext.Current.Request.Url.PathAndQuery 
~~~

~~~csharp
HttpContext.Current.Request.PhysicalPath.ToString();
~~~

----------


通過ASP.NET獲取URL地址方法 
如果測試的url地址是 http://www.test.com/testweb/default.aspx, 結果如下： 

~~~csharp
Request.ApplicationPath: /testweb 
Request.CurrentExecutionFilePath: /testweb/default.aspx 
Request.FilePath: /testweb/default.aspx 
Request.Path: /testweb/default.aspx 
Request.PhysicalApplicationPath: E:\WWW\testwebRequest.PhysicalPath: 
Request.PhysicalPath: E:\WWW\testweb\default.aspx 
Request.RawUrl: /testweb/default.aspx 
Request.Url.AbsolutePath: /testweb/default.aspx 
Request.Url.AbsoluteUrl: http://www.test.com/testweb/default.aspx 
Request.Url.Host: www.test.com 
Request.Url.LocalPath: /testweb/default.aspx
~~~

