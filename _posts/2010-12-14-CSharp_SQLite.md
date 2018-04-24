---
layout: post
title: 'C# 使用 System.Data.SQLite'
author: 'James Peng'
tags: ['SQLite']
---

## System.Data.SQLite ## 


http://sqlite.phxsoftware.com/ 

檔案下載後, 解壓縮, 從中取得 System.Data.SQLite.dll 


## using ##

~~~csharp
using System.Data.SQLite;
~~~

~~~csharp
private SQLiteConnection conn;
private SQLiteCommand cmd;
~~~

----------

## 連接db ##

~~~csharp
conn = new SQLiteConnection("Data Source=c:\\test.db"); 
conn.Open(); 
~~~


----------

## INSERT / UPDATE: ##

~~~csharp
cmd = conn.CreateCommand(); 
cmd.CommandText = "INSERT INTO user(email,name) VALUES ('email','name')"; 
cmd.ExecuteNonQuery(); 

cmd.CommandText = "UPDATE userSET name = 'Codelicious' WHERE ID = 1"; 
cmd.ExecuteNonQuery(); 
~~~


----------


## SELECT ##

~~~csharp
cmd.CommandText = "SELECT ID, name FROM user"; 
SQLiteDataReader reader = cmd.ExecuteReader(); 
if (reader.HasRows) 
             { 
                 while (reader.Read()) 
                { 
                    Console.WriteLine("ID: " + reader.GetInt16(0)); 
                     Console.WriteLine("name: " + reader.GetString(1)); 
                 } 
             } 
~~~



----------

參考：

- 範例來自：http://www.javaeye.com/topic/114055
- http://mynotes.imyichang.com/note-sqlite/cshiyongsystemdatasqlite
- http://sqlite.phxsoftware.com/ 
