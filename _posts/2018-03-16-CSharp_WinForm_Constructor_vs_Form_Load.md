---
layout: post
title: 'C# 使用 Form constructor vs WinForm Form_Load 執行順序'
author: 'James Peng'
tags: ['WinForm']
---


The order of events and what you can do in events is very important in Windows applications.

~~~text
Events traced
Form - Client Size Changed : 8/14/2010 10:40:28 AM
Form - Control Added - button1 : 8/14/2010 10:40:29 AM
Form - Constructor : 8/14/2010 10:40:29 AM
Form - Handle Created : 8/14/2010 10:40:29 AM
Form - Invalidated : 8/14/2010 10:40:29 AM
Form - Form Load event : 8/14/2010 10:40:29 AM
Form - Loaded : 8/14/2010 10:40:29 AM
Form - Create Control : 8/14/2010 10:40:29 AM
Form - OnActivated : 8/14/2010 10:40:29 AM
Form - Shown : 8/14/2010 10:40:29 AM
Form - OnPaint : 8/14/2010 10:40:29 AM
Form - Invalidated : 8/14/2010 10:40:29 AM
Form - OnPaint : 8/14/2010 10:40:29 AM
~~~

## 參考： ##

- http://www.jerry.nl/publicaties/artikelen/weblog-single/winforms-load-vs-shown-events-stack-overflow.html
- https://stackoverflow.com/questions/397121/load-vs-shown-events-in-windows-forms/3484040#3484040
