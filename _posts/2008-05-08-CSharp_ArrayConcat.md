---
layout: post
title: 'C# 合併陣列'
author: 'James Peng'
tags: ['C#']
---

## 用法 ##

~~~csharp
Num1.Concat(Num2);
~~~

----------


## 範例 ##

~~~csharp
for (int i = 1; i &lt;= 12; i++) 
{ 
    periodMonth = GetPeriod(i, year); 
    if (periodYear == null) 
    { 
        periodYear = periodMonth; 
    } 
    else 
    { 
        periodYear = periodYear.Concat(periodMonth).ToArray(); 
    } 
}
~~~


----------

參考：

- https://msdn.microsoft.com/zh-tw/library/2e06zxh0(v=vs.94).aspx
