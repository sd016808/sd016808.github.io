---
layout: post
title: 'C# TextBox 將游標移至最後一個字元'
author: 'James Peng'
tags: ['WinForm']
---


~~~csharp
TextBox.SelectionStart = TextBox.Text.Length;
TextBox.ScrollToCaret();
TextBox.Focus();
~~~



參考：

- http://www.dotblogs.com.tw/brian/archive/2014/02/17/144018.aspx
