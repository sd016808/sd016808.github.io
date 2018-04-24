---
layout: post
title: 'SQLite 擷取字串轉數字'
author: 'James Peng'
tags: ['SQLite']
---

##  SQLite 擷取字串轉數字  ##

先看圖

![](..\images\2016-05-08-SQLite_substring_CAST\NSOQQbm.png)


資料庫裡的生日資料，竟然是中文字，如果要進行查詢，例如查詢出比1986還大的資料，怎麼做？

----------


## 1.切割字串 substr ##



~~~sql
substr(Birthday,1,4)
~~~

![](..\images\2016-05-08-SQLite_substring_CAST\NSOQQbm.png)


結果:

	1989


----------

## 2.把 字串 轉 數字 CAST ##

~~~sql
SELECT * FROM "Member"
where CAST( substr(Birthday,1,4)  as integer) > 1986
~~~



----------

參考：

- http://stackoverflow.com/questions/10413055/how-to-get-substring-in-sqlite
- http://tiebing.blogspot.tw/2011/07/sqlite-3-string-to-integer-conversion.html
