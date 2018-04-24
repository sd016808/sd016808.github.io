---
layout: post
title: 'C# 複製到剪貼簿'
author: 'James Peng'
tags: ['C#']
---

## C# 複製到剪貼簿 ##

Clipboard 類別提供了一些方法，可以用來與 Windows 作業系統的剪貼簿功能互動。

~~~csharp
            Clipboard.SetData(DataFormats.Text, strOutput);
            MessageBox.Show("Copy to Clipboard Done!");
~~~


----------


參考：

- https://msdn.microsoft.com/zh-tw/library/637ys738(v=vs.110).aspx
