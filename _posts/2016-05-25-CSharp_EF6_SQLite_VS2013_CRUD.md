---
layout: post
title: 'C# 使用 EF6 於 SQLite 的 查詢 新增 刪除 修改'
author: 'James Peng'
tags: ['Entity Framework']
---


# 查詢 #

## 全部資料顯示 到 dataGrid ##

~~~csharp
            using (TerribleDontAskEntities db = new TerribleDontAskEntities())
            {
                dataGridView1.DataSource = db.Member.ToList();
            }
~~~

![](..\images\2016-05-25-CSharp_EF6_SQLite_VS2013_CRUD\d66hK4a.png)


----------

## 條件查詢 + 自定義 dataGrid 欄位名稱 ##

~~~csharp
            using (TerribleDontAskForDeltaEntities db = new TerribleDontAskForDeltaEntities())
            {
                // some LINQ query
                var query = from data in db.Member
                            where data.Name_cht == "彭嘉宏"
                            select new
                            {
                                名字 = data.Name_cht,
                                英文名 = data.Name_eng,
                            };
                dataGridView1.DataSource =  query.ToList();
            }
~~~

![](..\images\2016-05-25-CSharp_EF6_SQLite_VS2013_CRUD\CW0Ptin.png)


----------

## 新增 ##

~~~csharp
            using (EsiEntities db = new EsiEntities())
            {
                db.Table_ObjectType.Add(new Table_ObjectType { Index = "1", Name = "Iverson.Hong", Type = "11" , BitSize = "555" });
                db.Table_ObjectType.Add(new Table_ObjectType { Index = "2", Name = "Michael Jordan", Type = "22", BitSize = "555" });

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

----------


## 刪除 ##

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

----------

## 修改 ##

~~~
    using (testefEntities db = new testefEntities())
    {
        var entity = db.Table1.Find(2);
        if (entity != null)
        {
            entity.Name = entity.Name + "(Taiwan)";
            try
            {
                int cnt = db.SaveChanges();
            }
            catch (Exception ex)
            {
                Debug.WriteLine(ex.Message);
            }
        }
    }
~~~

----------


## 參考： ##

- http://iverson127.github.io/CSharp_EF6_SQLite_Operator/

