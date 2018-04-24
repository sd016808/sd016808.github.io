---
layout: post
title: 'C# 預處理器指令 #region'
author: 'James Peng'
tags: ['C#']
---

## #region 是 C# 預處理器指令。 ##

 
    #region 是一個分塊預處理命令，它主要是用於編輯器代碼的分塊，在編譯時會被自動刪除。
    
    #region 可讓您指定在使用 Visual Studio 程式碼編輯器的大綱功能時，可以展開或摺疊的程式碼區塊。

----------


## 例如：  ##

~~~csharp
#region MyClass definition 
public class MyClass  
{ 
    static void Main()  
    { 
    } 
} 
#endregion
~~~


----------

## 備註： ##

    #region 區塊必須以 #endregion 指示詞結束。
    #region 區塊不能與 #if 區塊重疊。但是 #region 區塊可以巢狀出現在 #if 區塊中，而 #if 區塊可以巢狀出現在 #region 區塊中。

----------

參考：

- http://msdn.microsoft.com/zh-tw/library/9a1ybwek(VS.80).aspx
