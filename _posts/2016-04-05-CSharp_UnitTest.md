---
layout: post
title: 'C# 透過 Unit Test Generator 快速為專案加入單元測試程式碼'
author: 'James Peng'
tags: ['Visual Studio extension for C#']
---

今天要跟各位介紹的，是由微軟的 Visual Studio ALM Rangers 小組所開發出來的 Visual Studio 擴充功能包 Unit Test Generator。

只要在 Visual Studio 2012 以上版本的 TOOLS –> Extensions and Updates 視窗中針對 Online 分類搜尋 "Unit Test" 關鍵字，就可以找到這個擴充包來安裝囉!!

![](..\images\2016-04-05-CSharp_UnitTest\hIdv7iH.png)


----------

## 使用方法: ##

![](..\images\2016-04-05-CSharp_UnitTest\GjhIO9j.png)


點選 "Generate Unit Test" 之後~~~ 

專案、測試類別和方法的骨架在幾秒內就全部生出來囉!!!!~~


----------

## 有些函式不會出現的原因 ##

![](..\images\2016-04-05-CSharp_UnitTest\GCqQSAi.png)

CLASS 沒 public

![](..\images\2016-04-05-CSharp_UnitTest\hCKo7ar.png)

----------

參考：

- https://dotblogs.com.tw/ouch1978/2013/10/30/vs-unit-test-generator-extension

