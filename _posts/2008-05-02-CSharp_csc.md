---
layout: post
title: 'C# 透過 CSC.EXE 批次檔編譯原始碼成 DLL'
author: 'James Peng'
tags: ['C#']
---

C#編譯批次檔(compile.bat) 直接放在目錄內就可以用了


compile.bat

~~~text
@if not exist bin\ (@echo ※注意※bin資料夾不存在，自動建立)
@if not exist bin\ (@mkdir bin)
@if exist source\ (@C:\WINDOWS\Microsoft.NET\Framework\v2.0.50727\csc.exe /t:library /r:bin\MySql.Data.dll /out:bin\ServiceSample.dll /recurse:source\*.cs)
@if not exist source\ (@echo ※注意※找不到原始碼資料夾)
@if not exist source\ (@pause)
@if errorlevel 1 (@pause)
~~~


----------


編譯指令

~~~text
csc.exe /t:library /out:YourProject.dll /recurse:*.cs
~~~

使用 /recurse 可建立資料夾，方便整理 .cs 檔


----------

參考：

- [MSDN /recurse (在子目錄中尋找原始程式檔)](https://msdn.microsoft.com/zh-tw/Library/8t9te37d(v=vs.80).aspx)
