---
layout: post
title: 'DevExpress 禁止 Treelist 拖曳 欄位 header 造成錯誤'
author: 'James Peng'
tags: ['DevExpress']
---

I want to prevent users from dragging and dropping columns . How can I enable or disable column drag and drop?


![](https://i.imgur.com/ifmAMK3.png)

This can be done using the GridView's OptionsCustomization.AllowColumnMoving property.

~~~csharp
treeList1.OptionsCustomization.AllowColumnMoving = false;
~~~

## reference： ##

- https://www.devexpress.com/Support/Center/Question/Details/Q22786/column-drag-and-drop
