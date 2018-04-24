---
layout: post
title: 'C# 加殼的方法'
author: 'James Peng'
tags: ['Visual Studio extension for C#']
---

因為 C# 使用 Reflector 可以輕易的將 .NET 程式進行反向工程！

加殼壓縮是一種對EXE檔案的數據壓縮及加密保護，可以將EXE檔案壓縮成自我解壓檔案，並能隱藏解壓進程。

最近上網找了下給 C#程序加殼的方法，找到了一些方法，自己試了一下，感覺還挺不錯的。以下是在網上找到的，在這裡整理一下，以後要用到的時候也方便。 

## 1.反射加殼  ##

- 把 exe程式檔名改為「命名空間.程序.exe」
- 新增一個CMD專案，複製程式「命名空間.程序.exe」到專案文件中
- 設置成為「嵌入式資源」。
- main裡使用以下程式碼：

~~~csharp
    Stream sr = Assembly.GetExecutingAssembly().GetManifestResourceStream("命名空間.程序.exe"); 
    byte[] fileBytes = new byte[sr.Length]; 
    sr.Read(fileBytes, 0, (int)sr.Length -1); 
    Assembly assembly = Assembly.Load(fileBytes); 
    MethodInfo mi = assembly.EntryPoint; 
    mi.Invoke(null, null);
~~~

編譯運行這個後，再用 Reflector 查看就看不到原始碼。不過還是可以用反射脫殼破解的，這個我就不太懂了 

## 2.使用Sixxpack  ##

Sixxpack 是一個 .Net EXE 檔案壓縮工具。經 Sixxpack 壓縮過的檔案，執行時與壓縮前相比沒有任何區別，壓縮比最大可達80%。，壓縮完之後就編譯不出源文件了。用Reflector查看的話都是actmp.dll的信息。不過最近看cnblogs裡有人給出了破解這個的方法，有興趣的可以去找找看。

![](..\images\2012-08-20-CSharp_Sixxpack\pDnlZ3S.png)

[下載](https://drive.google.com/file/d/0B50p9ExIX1S7NllndW1CbkluZEE/view?usp=sharing)

執行環境：Microsoft .Net Framework 2.0

把這兩種方法結合起來使用還是挺有意思的，起碼增加了別人破解的難度~~~呵呵


----------

參考：

- http://www.lwolf.cn/blog/article/code/csharp-shell%20.htm
- http://dl.onlinedown.net/soft/56183.htm
