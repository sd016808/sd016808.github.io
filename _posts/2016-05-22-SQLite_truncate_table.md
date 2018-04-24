---
layout: post
title: 'SQLite 如何 Truncate Table'
author: 'James Peng'
tags: ['SQLite']
---

在 SQLite 中，並沒有 TRUNCATE TABLE 命令，


使用 SQLite 的 DELETE 命令從已有的表中刪除全部的資料

然後 auto_increment 重新歸零計數 ，透過 刪除 sqlite_sequence裡的資料

也可以使用DROP TABLE命令刪除整個表，然後再重新建一遍。


不過一定有類似的指令可完成相同的工作，經過爬文和測試後，

確認下列語法可完成相同的目的

~~~text
DELETE FROM your_table;    
DELETE FROM sqlite_sequence WHERE name = 'your_table';
~~~


----------

參考：

- http://stackoverflow.com/questions/3443630/reset-the-row-number-count-in-sqlite3-mysql
- http://cym6112.blogspot.tw/2011/08/sqlitetruncate-table.html
