---
layout: post
title: 'C# 執行外部執行檔'
author: 'James Peng'
tags: ['C#']
---

~~~csharp
System.Diagnostics.ProcessStartInfo p = new System.Diagnostics.ProcessStartInfo();
p.WorkingDirectory = Server.MapPath("~/"); //執行檔路徑
p.FileName="compile.bat"; //執行檔名
System.Diagnostics.Process.Start(p);
~~~

----------

參考：

- http://delphi.ktop.com.tw/board.php?cid=169&amp;fid=1220&amp;tid=69645
