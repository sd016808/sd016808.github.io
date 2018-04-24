---
layout: post
title: 'Windows2003遠端連線數滿了解決方法'
author: 'James Peng'
tags: ['Windows']
---

Windows 2003
遠端使用者同時最大連線數通常是2人，若有使用者登入使用後，忘了登出就會佔用一個連線數，當連線數滿了就無法登入了，但你又不再機器旁，解決方法很簡單：

IP位址 /console

範例： 127.0.0.1 /console

![image](http://lh4.ggpht.com/_AnTT9cbXdqY/SUspw9e9tqI/AAAAAAAAGFg/vibMsJSS59E/image%5B5%5D.png?imgmax=800 "image")

/console 的連線意義是去搶下那台電腦原本用 local
螢幕可以看到的畫面的控制權。所以這樣子來算的話，可以說 Windows 2000
Server 或 Windows Server 2003 有 3 個連線數。

