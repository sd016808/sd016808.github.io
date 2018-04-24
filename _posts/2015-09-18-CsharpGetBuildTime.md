---
layout: post
title: 'C#中取得最後編譯時間'
author: 'James Peng'
tags: ['C#']
---


在實際的專案開發中，我們常常需要記錄某個Project的最後編譯時間，

原來在C++中，我們有個__DATE__，__TIME__，__FILE__，__LINE__這樣的Macros獲得最後編譯時間，例如：

~~~cpp
CString m_strBuildCode = (CString)__DATE__ + _T(" ") + (CString)__TIME__;
~~~

但是在C#中，可以用以下語法...

C#獲得最後編譯時間：

~~~csharp
System.IO.File.GetLastWriteTime(this.GetType().Assembly.Location)
~~~


C#獲取程序集的版本號：

~~~csharp
string ver = System.Reflection.Assembly.GetExecutingAssembly().GetName().Version.ToString();
~~~


參考：

[http://tangzhongxin.blog.163.com/blog/static/89219612010113173146319/](http://tangzhongxin.blog.163.com/blog/static/89219612010113173146319/)
