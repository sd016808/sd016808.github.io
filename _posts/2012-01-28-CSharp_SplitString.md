---
layout: post
title: 'C#的 Split 不支援 string ?'
author: 'James Peng'
tags: ['C#']
---

System裡的Char Class的split沒有支援string作為delimiter 
 
可以用

~~~csharp
System.Text.RegularExpressions.Regex regex = new System.Text.RegularExpressions.Regex("字串flag"); 
string[] words = regex.Split(fullString);
~~~


----------

參考：

- http://msdn.microsoft.com/zh-tw/library/b873y76a(VS.80).aspx
