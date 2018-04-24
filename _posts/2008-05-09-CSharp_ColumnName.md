---
layout: post
title: 'C# 取得 DataTables 欄位名稱'
author: 'James Peng'
tags: ['C#']
---

## DataColumn ##

- AllowDBNull 取得或設定值，指出對於屬於資料表的資料列而言，這個資料行中是否允許 Null 值。
- AutoIncrement 取得或設定值，指出對於加入至資料表的新資料列而言，該資料行是否自動遞增資料行的值。
- Caption 取得或設定資料行的標題。
- ColumnName 取得或設定在 DataColumnCollection 中的資料行名稱。
- DataType 取得或設定儲存在資料行中的資料型別。
- DefaultValue 在建立新資料列時，取得或設定資料行的預設值。
- MaxLength 取得或設定文字資料行的最大長度。
- Ordinal 取得在 DataColumnCollection 集合中的資料行位置。
- ReadOnly 取得或設定值，指出是否資料列一加入至資料表，就允許變更資料行。
- Unique 取得或設定值，指出在資料行之每個資料列中的值是否必須是唯一的。

----------


## 範例語法： ##

~~~csharp
s = s.Replace("__範例1__", ""+ds.Tables["t"].Columns[i].ColumnName );
s = s.Replace("__範例2__", ""+ds.Tables["t"].Columns[i].DataType );
~~~


----------

## 全部讀出欄位名稱： ##

~~~csharp
string temp= "";
foreach (DataColumn Columns in ds.Tables["t"].Columns)
{
    temp = temp + Columns.ColumnName + "," ;
}
~~~

----------

參考：

- [MSDN DataColumn 類別，表示在 DataTable 中資料行的結構描述 (Schema)。](http://msdn2.microsoft.com/zh-tw/library/system.data.datacolumn_members%28VS.80%29.aspx)
