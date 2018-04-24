---
layout: post
title: 'SQLite 使用 Foreign Key'
author: 'James Peng'
tags: ['SQLite']
---

## 安裝SQLite ##

在Package Manager Console中輸入: 

~~~text
Install-Package System.Data.SQLite
~~~

## FOREIGN KEY 外鍵 ##

外鍵是一個(或多個)指向其它資料表中主鍵的欄位，它限制欄位值只能來自另一個資料表的主鍵欄位，用來確定資料的參考完整性 (Referential Integrity)。 


----------


## ON UPDATE, ON DELETE ##

在建Foreign Key時,遇到ON UPDATE,ON DELETE
需要設置CASCADE | SET NULL | NO ACTION | RESTRICT

這些屬性的意義為 :


- CASCADE 會將有所關聯的紀錄行也會進行刪除或修改。
- SET NULL 會將有所關聯的紀錄行設定成 NULL。
- NO ACTION 有存在的關聯紀錄行時，會禁止父資料表的刪除或修改動作。
- RESTRICT 與 NO ACTION 相同。


----------

## SQLite Foreign Key Support ##

跟據官方說明SQLite是從3.6.19之後的版本有開始支援Foreign Key

SQLite 在設定完 foreign key 之後，對 foreign key 的限制卻沒有生效

所以必須在每次的 DB connection 時，都需要做一次這個動作：

~~~text
PRAGMA foreign_keys = ON;
~~~



----------

##  ##

- 要先建立 table
- 開啟 Foreign Key
- 

~~~csharp
        public Form1()
        {
            if (SqlOperator.OpenDb(SqlCreateHelper.PROGRAM))
            {
                CREATE_TABLE_ObjectType();
                CREATE_TABLE_PropertyType();
                SqlOperator.Execute(@"PRAGMA foreign_keys = ON;");
            }
            InitializeComponent();
        }



        private void button1_Click(object sender, EventArgs e)
        {
            if (SqlOperator.OpenDb(SqlCreateHelper.PROGRAM))
            {
                SqlOperator.Execute(@"DELETE FROM Table_ObjectType where id >= 0");
            }
        }



        public static void CREATE_TABLE_ObjectType()
        {
            SqlOperator.Execute(@"

			PRAGMA foreign_keys = OFF;
			
			CREATE TABLE 'Table_ObjectType' (
			'id'  INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
			'Index'  TEXT NOT NULL,
			'Name'  TEXT NOT NULL,
			'Comment'  TEXT,
			'Type'  TEXT NOT NULL,
			'BitSize'  TEXT NOT NULL,
			'Flags_Access'  TEXT,
			'Flags_Category'  TEXT,
			'Flags_PdoMapping'  TEXT,
			'Flags_SafetyMapping'  TEXT,
			'Flags_Attribute'  TEXT,
			'Flags_Transition'  TEXT,
			'Flags_SdoAccess'  TEXT,
			'Flags_Backup'  TEXT,
			'Flags_Setting'  TEXT
			);
			
			
			INSERT INTO 'Table_ObjectType' ('Index','Name','Type','BitSize') VALUES ('1', '1', '2','2');

			");
        }

        public static void CREATE_TABLE_PropertyType()
        {
            SqlOperator.Execute(@"

			PRAGMA foreign_keys = OFF;
			
			CREATE TABLE 'Table_PropertyType' (
			'id'  INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
			'id_ObjectType'  INTEGER,
			'Name'  TEXT,
			'Value'  TEXT,
			'Desc'  TEXT,
			CONSTRAINT 'fkey0' FOREIGN KEY ('id_ObjectType') REFERENCES 'Table_ObjectType' ('id') ON DELETE CASCADE ON UPDATE CASCADE
			);
			
			");
        }
~~~


----------

## SqlOperator.cs ##

- connection 只 new 一次

~~~csharp
using System;
using System.Collections.Generic;
using System.Data;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Common;
using System.Data.SQLite;
using System.Collections;

namespace SQLiteTest
{
    public class SqlOperator
    {
        private static SQLiteConnection connection = null;
        private static SQLiteCommand command;  

        public static bool OpenDb(string strDbName)
        {
            if (connection != null)
            {
                return true;
            }
            else
            {
                try
                {
                    connection = new SQLiteConnection("Data source=" + strDbName);
                    connection.Open();
                    return true;
                }
                catch
                {
                    return false;
                }
            }
            
        }

        public static void CloseDb()
        {
            if (connection != null)
            {
                connection.Close();
                connection = null;
            }            
        }

        public static bool Execute(string strSQL)
        {
            return ExecuteNonQuery(strSQL);
        }

        public static DataTable Select(string strTable, string strSelect)
        {
            string strFormatTemp = "SELECT " + strSelect + " FROM " +strTable+"; ";
            DataTable tb = Table(strFormatTemp);            
            return tb;
        }

        public static DataTable Select(string strTable, string strSelect, string strWhere)
        {
            string strFormatTemp = "SELECT " + strSelect + " FROM " + strTable + "  WHERE " + strWhere +"; ";
            DataTable tb = Table(strFormatTemp);
            return tb;
        }

        public static bool Update(string strTable, string strSet, string strWhere)
        {            
            string strFormatTemp = "UPDATE '" + strTable +"' SET "+ strSet +" WHERE "+ strWhere +"; ";
            if (connection != null)
            {
                if (ExecuteNonQuery(strFormatTemp)) return true;
                else return false;
            }
            else return false;
        }

        public static bool Update(string strTable, string strSet)
        {
            string strFormatTemp = "UPDATE '" + strTable + "' SET " + strSet  + "; ";
            if (connection != null)
            {
                if (ExecuteNonQuery(strFormatTemp)) return true;
                else return false;
            }
            else return false;
        }

        public static bool Insert(string strTable, string strKey, string strValue)
        {
            string strFormatTemp = "INSERT INTO '" + strTable +"' ("+ strKey +") VALUES("+ strValue +");" ;
            if (connection != null)
            {
                if (ExecuteNonQuery(strFormatTemp)) return true;
                else return false;
            }
            else return false;
        }

        public static bool Insert(string strTable, string strValue)
        {
            string strFormatTemp = "INSERT INTO '"+ strTable +"' VALUES(" + strValue+ ");";
            if (connection != null)
            {
                if (ExecuteNonQuery(strFormatTemp)) return true;
                else return false;
            }
            else return false;
        }

        public static bool Delete(string strTable, string strWhere)
        {

            string strFormatTemp = "DELETE FROM '"+ strTable +"' WHERE "+ strWhere +";";
            if (connection != null)
            {
                if (ExecuteNonQuery(strFormatTemp)) return true;
                else return false;
            }
            else return false;
        }



        //執行SQL
        private static bool ExecuteNonQuery(string SQLstr)
        {
            if (connection == null) throw new ArgumentNullException("connection");
            try
            {

                SQLiteCommand command = connection.CreateCommand();
                command.CommandText = SQLstr;
                //PrepareCommand(cmd, trans.Connection, trans, cmdType, cmdText, commandParameters);
                int val = command.ExecuteNonQuery();
                command.Parameters.Clear();//清除參數,以便再次使用.

                return (val > 0) ? true : false;
            }
            catch (Exception e)
            {
                //throw new Exception(e.Message);
                return false;
            }
        }


        //執行SQL
        private static string RunValue(string SQLstr)
        {
            try
            {

                SQLiteCommand command = connection.CreateCommand();
                command.CommandText = SQLstr;
                return "" + command.ExecuteScalar();
            }
            catch (Exception e)
            {
                //throw new Exception(e.Message);
                return (new Exception(e.Message)).ToString();
            }
        }

        //回傳DataTable
        public static DataTable Table(string SQLstr)
        {
            try
            {
                DataTable dt = new DataTable();

                SQLiteCommand command = connection.CreateCommand();
                command.CommandText = SQLstr;
                               
                SQLiteDataAdapter adapter = new SQLiteDataAdapter();
                adapter.SelectCommand = command;
                adapter.Fill(dt);
                return dt;
            }
            catch (Exception e)
            {
                throw new Exception(e.Message);
            }
        }

        //回傳DataTable
        private static DataTable Table(string SQLstr, SQLiteCommand cmd)
        {
            try
            {
                DataTable dt = new DataTable();

                SQLiteDataAdapter adapter = new SQLiteDataAdapter();
                adapter.SelectCommand = command;
                adapter.Fill(dt);
                return dt;
            }
            catch (Exception e)
            {
                throw new Exception(e.Message);
            }
        }

    }

 
}
~~~


----------

## 寫入測試資料 ##

~~~text
INSERT INTO "Table_ObjectType" ("Index","Name","Type","BitSize") VALUES ('1', '1', '2','2');
INSERT INTO "main"."Table_PropertyType" ("id_ObjectType","Name","Value","Desc") VALUES (1, 1, 1,  1);
INSERT INTO "main"."Table_PropertyType" ("id_ObjectType","Name","Value","Desc") VALUES (1, 1, 1,  2);
INSERT INTO "main"."Table_PropertyType" ("id_ObjectType","Name","Value","Desc") VALUES (1, 1, 1,  3);
INSERT INTO "main"."Table_PropertyType" ("id_ObjectType","Name","Value","Desc") VALUES (1, 1, 1,  4);
INSERT INTO "main"."Table_PropertyType" ("id_ObjectType","Name","Value","Desc") VALUES (1, 1, 1,  5);
INSERT INTO "main"."Table_PropertyType" ("id_ObjectType","Name","Value","Desc") VALUES (1, 1, 1,  6);
INSERT INTO "main"."Table_PropertyType" ("id_ObjectType","Name","Value","Desc") VALUES (1, 1, 1,  7);
~~~


----------

## 刪除測試 ##

![](..\images\2016-05-21-SQLite_ForeognKey\O9U7x71.png)

![](..\images\2016-05-21-SQLite_ForeognKey\28iAbx6.png)

![](..\images\2016-05-21-SQLite_ForeognKey\4li9gPC.png)

![](..\images\2016-05-21-SQLite_ForeognKey\hGWCQJ3.png)

![](..\images\2016-05-21-SQLite_ForeognKey\WuwvgZQ.png)

----------

參考：

- http://a-jau.blogspot.tw/2012/09/foreign-keycascade-set-null-no-action.html
- http://webdesign.kerthis.com/sql/sql_foreign_key
- http://blog.carlcarl.me/1404/sqlite_foreign_key_support/
