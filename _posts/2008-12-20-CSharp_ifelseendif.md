---
layout: post
title: 'C# 前置處理器指示詞 #if、#else、#endif'
author: 'James Peng'
tags: ['C#']
---

![](..\images\2008-12-20-CSharp_ifelseendif\WmluzLn.png)

~~~csharp
#if NET1 
[assembly: AssemblyProduct("肥豬程式 (.NET Framework 1.1)")] 
#else 
[assembly: AssemblyProduct("肥豬程式 (.NET Framework 2.0/3.x)")] 
#endif
~~~

以上程式碼，編譯時用來判斷版本。

#if 可讓您開始一個條件指示詞，測試一或多個符號是否評估為 true。
如果評估為 true，編譯器便會評估介於 #if 和與其最接近的 #endif 指示詞之間的所有程式碼。例如，

----------

參考：

- http://msdn.microsoft.com/zh-tw/library/4y6tbswk(VS.80).aspx
