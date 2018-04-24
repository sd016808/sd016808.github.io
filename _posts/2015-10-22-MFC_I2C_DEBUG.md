---
layout: post
title: '已在 LIBCMTD.lib(dbgdel.obj) 中定義過了'
author: 'James Peng'
tags: ['Visual C++']
---

~~~text
1>uafxcwd.lib(afxmem.obj) : error LNK2005: "void __cdecl operator delete(void *)" (??3@YAXPAX@Z) 已在 LIBCMTD.lib(dbgdel.obj) 中定義過了
1>C:\Users\jia-hong-peng\documents\visual studio 2013\Projects\IICBusControlGUI\Debug\IICBusControlGUI.exe : fatal error LNK1169: 找到有一或多個已定義的符號
~~~


## 解法: ##

修改project-> setting -> General-> Mircosoft Fountation

classes為Use mfc in a shard dll

分析：原來的是“使用windows庫”，這樣可能多次包含了庫。

技巧：查看搜索庫的順序.


----------


## For example ##

在頭文件test.h內已經包含了#include <iostream>

而在引用test.h的文件內又包含了#include <iostream>

這時編譯時會出現上述問題

將引用test.h的文件的include <iostream>刪除後則會出現同樣的編譯情況，但是並未報錯，所以我們要盡量避免重複包含庫文件。



----------

## 參考： ##

- http://blog.csdn.net/zhangweijiqn/article/details/9101257
