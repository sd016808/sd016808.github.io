---
layout: post
title: 'LoadLibrary() 出現 Error code 127'
author: 'James Peng'
tags: ['Visual C++']
---

## 錯誤 ##

    LoadLibrary() 出現 Error code 127

## 解法 ##

~~~cpp
hLoadDll = LoadLibrary(LibFileName);
~~~

改成

~~~cpp
hLoadDll = LoadLibraryEx(LibFileName, NULL, DONT_RESOLVE_DLL_REFERENCES);
~~~

