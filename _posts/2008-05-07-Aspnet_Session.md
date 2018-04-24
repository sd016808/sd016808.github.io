---
layout: post
title: 'ASP.NET:沒繼承 Page 的類別，如何使用 Session ?'
author: 'James Peng'
tags: ['ASP.NET']
---


在APP_Code 裡
沒繼承 Page 的類別.CS文件，無法直接使用 Session

改成下列語法：

~~~csharp
HttpContext.Current.Session["XX"];
~~~

Response也是哦

~~~csharp
HttpContext.Current.Response.Write("XX");
~~~

----------

參考：

- http://msdn.microsoft.com/library/cht/default.asp?url=/library/CHT/cpref/html/frlrfSystemWebHttpContextClassSessionTopic.asp
- http://blog.xuite.net/sunnysoap/r/15319502
