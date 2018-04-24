---
layout: post
title: 'Java Servlet 入門 JDBC-ODBC'
author: 'James Peng'
tags: ['Java']
---

這是之前用 JavaSwing 連 Mssql 時摸索的

沒記錄到 JDBC.jar 使用方式，還需補充，及 Web server 使用方式

MsSQL 連線 2000 是OK的，2005沒測試過


----------

## Tomcat - JDBC USE ##

將 jdbc.jar 檔，放置 {你的目錄}\WEB-INF\lib 下，重新動 Tomcat 即可



----------

## 設定值 -- 把設定值拉至外邊，使得不用修改 DB連線字串，便於修改 ##

~~~java
    String SQLServerIP = "IPaddress"; 		   //DB的位置，本機可使用 localhost
    String SQLServerPort = ":port"; 			   //帶":"的PortNumber，MSSQL使用預設Port(1433)可空白
 	  	  	  	  	  	  	  	  	  	  // MySQL 預設Port ( 3306 )
    String Database = "DatabaseName"; 	    //你的DB名稱
    String UserID = "YourID";	                   //你的DB登入帳號
    String UserPassword = "YourPassw"; 	   //你的DB登入密碼
~~~


###  Note | JAVA | JDBC - MySQL DataBase ###

[JDBC-下載位置]

http://dev.mysql.com/downloads/connector/j/5.0.html

下載後解壓縮，取得 [ mysql-connector-java-5.0.7-bin.jar ]

兩種方式均相同：

方式一：

~~~java
Class.forName("com.mysql.jdbc.Driver");
String connectionUrl = "jdbc:mysql://"+SQLServerIP+SQLServerPort+"/"+Database+"?user="+UserID+"&password="+UserPassword;
con = DriverManager.getConnection(connectionUrl);
~~~


方式二：

~~~java
 	   Class.forName("com.mysql.jdbc.Driver");
 	   String connectionUrl = "jdbc:mysql://"+SQLServerIP+SQLServerPort+"/"+Database ;
 	   con = DriverManager.getConnection(connectionUrl,UserID,UserPassword);
~~~


----------


###  Note | JAVA | JDBC - MicroSoft DataBase  ###

 [JDBC-下載位置]

http://www.microsoft.com/downloads/details.aspx?familyid=E22BC83B-32FF-4474-A44A-22B6AE2C4E17&displaylang=zh-tw

下載後解壓縮，取得 [ sqljdbc.jar ]




~~~java
// 使用 Access 直接 連線
String conDriver = "sun.jdbc.odbc.JdbcOdbcDriver";
String connectionUrl = "jdbc:odbc:DRIVER=Microsoft Access Driver (*.mdb);DBQ=F:/Database/Database.mdb"; // Access 實體路徑
Class.forName( conDriver );
con = DriverManager.getConnection(connectionUrl,UserID,Password);// 使用 SQL JDBC 連線
// 使用 jdbc 要先設環境變數 classpath
// 2005 sqljdbc  (path 只有 sqljdbc.jar)
// Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
// 2000 jdbc 	 (path 要三個 msbase.jar、mssqlserver.jar、msutil.jar)
// Class.forName("com.microsoft.jdbc.sqlserver.SQLServerDriver");
// ※使用 SQL server 2005 注意 TCP/IP 是否開啟，及 PortNumber 是否有設定(預設可能為空值非1433)
Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
String connectionUrl = "jdbc:sqlserver://"+SQLServerIP+SQLServerPort+";database="+Database+";user="+UserID+";password="+UserPassword;
con = DriverManager.getConnection(connectionUrl);

~~~


----------

參考書籍 - (JSP 2.0 技術手冊)
