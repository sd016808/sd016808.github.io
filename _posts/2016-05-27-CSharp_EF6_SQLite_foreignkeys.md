---
layout: post
title: 'C# 使用 EF6 於 SQLite 啟動 foreign key'
author: 'James Peng'
tags: ['Entity Framework']
---


SQLite 裡 default foreign key 是 關閉的，需要手動下SQL來 on 啟動

~~~text
PRAGMA foreign_keys = ON;
~~~

一般情況下，這樣就可以了

----------


但今天使用EF6時，無法下SQL字串，

當刪除 有設定 foreign_key 外鍵關連的資料，會失效 外鍵設定的資料都不會刪除

請在 app.config 裡，設定以下參數才會生效 

~~~text
data source=C:\Dbs\myDb.db;foreign keys=true;
~~~


----------

## 驗證 ##

刪除 id 為 1 的資料

![](..\images\2016-05-27-CSharp_EF6_SQLite_foreignkeys\VrY8i2q.png)

![](..\images\2016-05-27-CSharp_EF6_SQLite_foreignkeys\gNJjDgk.png)

~~~csharp
            using (EsiEntities db = new EsiEntities())
            {
                var entities = db.Table_ObjectType.Where(x => x.id == 1);

                foreach (var entity in entities)
                {
                    db.Table_ObjectType.Remove(entity);
                }
                                
                try
                {
                    int cnt = db.SaveChanges();
                }
                catch (Exception ex)
                {
                    Debug.WriteLine(ex.Message);
                }
            }
~~~

有設定外鍵的部份也會一併刪除

![](..\images\2016-05-27-CSharp_EF6_SQLite_foreignkeys\1VarJ3I.png)

![](..\images\2016-05-27-CSharp_EF6_SQLite_foreignkeys\rfPo1mJ.png)


----------

## 參考： ##

- https://msdn.microsoft.com/en-us/library/bb738533(v=vs.100).aspx
- http://a-jau.blogspot.tw/2013/08/centity-frame-work-connection-string.html
- http://blog.csdn.net/wusuopubupt/article/details/8817826
- https://msdn.microsoft.com/zh-tw/library/system.data.sqlclient.sqlconnectionstringbuilder(v=vs.110).aspx
- http://blog.miniasp.com/post/2015/11/23/How-Class-library-read-config-from-webconfig-or-appconfig-file.aspx
- http://stackoverflow.com/questions/7674318/cascade-on-delete-not-cascading-with-ef
- http://stackoverflow.com/questions/4254371/enabling-foreign-key-constraints-in-sqlite
