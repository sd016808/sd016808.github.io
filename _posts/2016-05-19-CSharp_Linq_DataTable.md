---
layout: post
title: 'C# Linq 查詢 DataTable'
author: 'James Peng'
tags: ['C#']
---
LINQ query on a DataTable

![](..\images\2016-05-19-CSharp_Linq_DataTable\BTkRb8i.png)


----------

You can't query against the DataTable's Rows collection, since DataRowCollection doesn't implement IEnumerable<T>. You need to use the AsEnumerable() extension for DataTable. 

AsEnumerable() returns IEnumerable<DataRow>. 

If you need to convert IEnumerable<DataRow> to a DataTable, use the CopyToDataTable() extension.

Like so:

~~~csharp
        private void button4_Click(object sender, EventArgs e)
        {
            
            if (SqlOperator.OpenDb(SqlCreateHelper.PROGRAM + ".db"))
            {
                DataTable dt = SqlOperator.Select("Member", "*");

                var find = from data in dt.AsEnumerable()
                           where data.Field<string>("Name_cht") == textBox_Name.Text
                           orderby data.Field<int>("Id") descending
                           select data;
           
                dataGridView1.DataSource = find.AsDataView();
            
            }
        }
~~~


----------

## 使用 Contains、EndsWith、StartsWith 發生 NullReferenceException  ##

![](..\images\2016-05-19-CSharp_Linq_DataTable\qWp3VZp.png)


解決辦法： 加上 判斷非null 條件

~~~csharp                          
where data.Field<string>("Name_cht") != null
~~~

----------



## 類似 T-SQL 中 LIKE '%string%' 的效果 ##

~~~csharp
        private void button4_Click(object sender, EventArgs e)
        {
            
            if (SqlOperator.OpenDb(SqlCreateHelper.PROGRAM + ".db"))
            {
                DataTable dt = SqlOperator.Select("Member", "*");

                var find = from data in dt.AsEnumerable()                           
                           where data.Field<string>("Name_cht") != null
                           where data.Field<string>("Name_cht").Trim().ToUpper().Contains(textBox_Name.Text.Trim().ToUpper())                           
                           select data;
           
                dataGridView1.DataSource = find.AsDataView();
            
            }
        }
~~~

----------


## 類似 T-SQL 中 LIKE '%string' 的效果 ##


~~~csharp
        private void button4_Click(object sender, EventArgs e)
        {
            
            if (SqlOperator.OpenDb(SqlCreateHelper.PROGRAM + ".db"))
            {
                DataTable dt = SqlOperator.Select("Member", "*");

                var find = from data in dt.AsEnumerable()                           
                           where data.Field<string>("Name_cht") != null
                           where data.Field<string>("Name_cht").Trim().ToUpper().EndsWith(textBox_Name.Text.Trim().ToUpper())                           
                           select data;
           
                dataGridView1.DataSource = find.AsDataView();
            
            }
        }
~~~

----------


## 類似 T-SQL 中 LIKE 'string%' 的效果 ##

~~~csharp
        private void button4_Click(object sender, EventArgs e)
        {
            
            if (SqlOperator.OpenDb(SqlCreateHelper.PROGRAM + ".db"))
            {
                DataTable dt = SqlOperator.Select("Member", "*");

                var find = from data in dt.AsEnumerable()                           
                           where data.Field<string>("Name_cht") != null
                           where data.Field<string>("Name_cht").Trim().ToUpper().StartsWith(textBox_Name.Text.Trim().ToUpper())                           
                           select data;
           
                dataGridView1.DataSource = find.AsDataView();
            
            }
        }
~~~

----------

## 參考： ##

- http://stackoverflow.com/questions/10855/linq-query-on-a-datatable
- https://dotblogs.com.tw/yc421206/archive/2011/08/15/33115.aspx
- http://msdn.microsoft.com/en-us/library/system.data.datarowextensions.aspx
- https://dotblogs.com.tw/terrychuang/2012/04/05/71297
