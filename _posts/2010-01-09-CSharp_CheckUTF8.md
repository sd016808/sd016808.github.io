---
layout: post
title: 'C# 判斷文件編碼 區別 UTF-8 和 BIG5'
author: 'James Peng'
tags: ['C#']
---

用C#涉及到一些讀取 txt文本文件的操作，但是一個編碼問題就困惑了我好久。如果編碼選的不對，會造成亂碼。之前轉載的一片文章提出了一種解決方法，就是用new StreamReader(file, Encoding.Default)。這種方法解決了大部分問題，但是測試中發現對於有的UTF-8文件依然會造成亂碼（中文windows環境）。

於是上網搜索解決方案。大多數是說UTF-8有特殊的前導碼EF BB BF，只要認出這個就能判定是UTF-8編碼了。但是我測試的一個文件發現前面並

 沒有這些前導碼啊…於是繼續搜索……

發現UTF8文件都有一個3字節的頭，為“EF BB BF”(稱為BOM--Byte Order Mark)，判斷這個頭信息不就可以解決了嗎？

後來發現，不是所有的UTF8編碼的文件都有BOM信息，

沒有BOM信息只有通過逐個字節比較的方式才能解決。

----------


## 判斷是否為UTF-8編碼的概率 ##

~~~csharp
int utf8_probability(byte[] rawtext) 
  { 
   int score = 0; 
   int i, rawtextlen = 0; 
   int goodbytes = 0, asciibytes = 0;

   // Maybe also use UTF8 Byte Order Mark: EF BB BF

   // Check to see if characters fit into acceptable ranges 
   rawtextlen = rawtext.Length; 
   for (i = 0; i < rawtextlen; i++) 
   { 
    if ((rawtext[i] & (byte)0x7F) == rawtext[i]) 
    { // One byte 
     asciibytes++; 
     // Ignore ASCII, can throw off count 
    } 
    else 
    { 
     int m_rawInt0 = Convert.ToInt16(rawtext[i]); 
     int m_rawInt1 = Convert.ToInt16(rawtext[i+1]); 
     int m_rawInt2 = Convert.ToInt16(rawtext[i+2]);

     if (256-64 <= m_rawInt0 && m_rawInt0 <= 256-33 && // Two bytes 
      i+1 < rawtextlen && 
      256-128 <= m_rawInt1 && m_rawInt1 <= 256-65) 
     { 
      goodbytes += 2; 
      i++; 
     } 
     else if (256-32 <= m_rawInt0 && m_rawInt0 <= 256-17 && // Three bytes 
      i+2 < rawtextlen && 
      256-128 <= m_rawInt1 && m_rawInt1 <= 256-65 && 
      256-128 <= m_rawInt2 && m_rawInt2 <= 256-65) 
     { 
      goodbytes += 3; 
      i+=2; 
     } 
    } 
   }

   if (asciibytes == rawtextlen) { return 0; }

   score = (int)(100 * ((float)goodbytes/(float)(rawtextlen-asciibytes)));

   // If not above 98, reduce to zero to prevent coincidental matches 
   // Allows for some (few) bad formed sequences 
   if (score > 98) 
   { 
    return score; 
   } 
   else if (score > 95 && goodbytes > 30) 
   { 
    return score ; 
   } 
   else 
   { 
    return 0; 
   }

  }
~~~



----------


## 用法 ##

~~~csharp
Encoding encode;
StreamReader srtest = new StreamReader(file.FullName,Encoding.Default);
int p = utf8_probability(Encoding.Default.GetBytes(srtest.ReadToEnd()));
if( p>80 )
    encode = Encoding.GetEncoding(65001);//utf8
else
    encode = Encoding.Default;
srtest.Close();
~~~



----------

參考：

- http://blog.csdn.net/zdg/archive/2005/01/29/272643.aspx
- http://blog.csdn.net/forsiny/archive/2009/11/15/4813107.aspx
- http://dev.csdn.net/Develop/article/10/10961.shtm
- http://dev.csdn.net/Develop/article/10/10962.shtm
- 
