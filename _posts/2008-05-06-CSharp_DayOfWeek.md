---
layout: post
title: 'C# 取得今天星期幾'
author: 'James Peng'
tags: ['C#']
---

DateTime.DayOfWeek 屬性

DayOfWeek 列舉的常數，表示一週天數。 

這個屬性值的範圍從 0 開始 (表示星期日) 到 6 (表示星期六)

~~~csharp
  DateTime dt = new DateTime(2009, 12, 17); 
  string tmp = dt.DayOfWeek.ToString();//tmp = Thursday 
  string tmp2 = dt.DayOfWeek.ToString("d");//tmp2 = 4
~~~


----------

參考：

- http://iambigd.blogspot.com/2009/12/c.html
