---
layout: post
title: 'C# 與 EF6 於 SQLite 用 Add 時，取得 Sid'
author: 'James Peng'
tags: ['Entity Framework']
---

RepositoryBase 裡，使用 泛型 所以，使用 dynamic 動態宣告 rowData

直接使用 rowData.id.ToString() 編譯器不檢查 dynamic 所以可以過

欄位對應有id這欄位 就沒問題，但萬一沒有 就會報錯了，所以使用上有風險，但很方便....


## RepositoryBase.cs 裡，加入 ##


~~~csharp
        public virtual int? AddThenGetSequenceId(T entity)
        {
            dynamic rowData = dbSet.Add(entity);
            int rtn = dbContext.SaveChanges();

            // 因為Update 單一model需要先關掉validation，因此重新打開
            if (dbContext.Configuration.ValidateOnSaveEnabled == false)
            {
                dbContext.Configuration.ValidateOnSaveEnabled = true;
            }

            if (rtn == 0) // insert fail
                return null;

            if (rowData.id == null)
                return null;

            int returnValue;
            bool rst = int.TryParse(rowData.id.ToString(), out returnValue);

            if (!rst)
                return null;

            return returnValue;
        }
~~~

