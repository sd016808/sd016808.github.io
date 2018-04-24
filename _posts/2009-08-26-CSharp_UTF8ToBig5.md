---
layout: post
title: 'C# UTF-8 轉換 BIG5'
author: 'James Peng'
tags: ['C#']
---

## C# UTF-8 轉換 BIG5 ##

~~~csharp

  /// 轉換BIG5 
  /// &lt;/summary&gt; 
  /// &lt;param name=”strUtf”&gt;輸入UTF-8&lt;/param&gt; 
  /// &lt;returns&gt;&lt;/returns&gt; 
  public string ConvertBig5(string strUtf) 
  { 
   Encoding utf81 = Encoding.GetEncoding(”utf-8″); 
   Encoding big51 = Encoding.GetEncoding(”big5″); 
   Response.ContentEncoding = big51; 
   byte [] strUtf81 = utf81.GetBytes(strUtf.Trim()); 
   byte [] strBig51 = Encoding.Convert(utf81, big51, strUtf81); 
   char[] big5Chars1 = new char[big51.GetCharCount(strBig51, 0, strBig51.Length)]; 
   big51.GetChars(strBig51, 0, strBig51.Length, big5Chars1, 0); 
   string tempString1 = new string(big5Chars1); 
   return tempString1; 
  } 
~~~


----------

參考：

- http://chrisbalboa.pixnet.net/blog/post/27631770
