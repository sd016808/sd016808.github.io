---
layout: post
title: '找不到 dxtrans.h'
author: 'James Peng'
tags: ['Visual C++']
---

兩個解決辦法：

## 方法一：  ##

把 qedit.h 裡面跟 dxtrans.h 還有 IDXEffect 相關的東西先 comment out 掉。 

~~~cpp
//#include "dxtrans.h" // -- Line 498 
// IDxtCompositor : public IDXEffect // -- Line 837 
// IDxtAlphaSetter : public IDXEffect // -- Line 1151 
// IDxtJpeg : public IDXEffect // -- Line 1345 
// IDxtKey : public IDXEffect // -- Line 1735
~~~

----------

## 方法二： ##

另外一個需要 comment out 掉比較少行的方法是： 

在 qedit.h 裡面，comment out 掉以下這一行。 

~~~cpp
//#include "dxtrans.h" // -- Line 498 
~~~

然後在 #include 前面加入以下定義： 

~~~cpp
#define __IDxtCompositor_INTERFACE_DEFINED__ 
#define __IDxtAlphaSetter_INTERFACE_DEFINED__ 
#define __IDxtJpeg_INTERFACE_DEFINED__ 
#define __IDxtKey_INTERFACE_DEFINED__
~~~


----------

reference: 

- http://hi.baidu.com/qinpc/blog/item/b7a7b5b7991f30f131add18c.html
- http://victorgau.blogspot.tw/2009/12/dxtransh-hmmm.html

