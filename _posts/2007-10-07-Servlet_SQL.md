---
layout: post
title: 'Java Servlet 入門 SQL指令操作'
author: 'James Peng'
tags: ['Java']
---



## SQL基本簡易操作 - 查詢及讀取資料 ##


~~~java
DBConnection DBC = new DBConnection();
Connection con = null;    //連線
try{
    con = DBC.getConnection();    //※取得 Connection 詳情，見 JDBC 連線
    Statement stmt = con.createStatement();
    String strSQL = "select * from announce";
    ResultSet rs = stmt.executeQuery(strSQL);

    while( rs.next() ){
 	   String oneLoop = oneSample;
 	   oneLoop = oneLoop.replaceAll("__postDate__", rs.getString("dateStart") );
 	   oneLoop = oneLoop.replaceAll("__operateID__", rs.getString("anID") );
 	   oneLoop = oneLoop.replaceAll("__postTitle__", rs.getString("title") );

 	   output += oneLoop;
    }
    rs.close(); //關閉
    stmt.close();
} catch ( Exception e ) { }
~~~

