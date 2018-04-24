---
layout: post
title: 'C# 連接 MySql'
author: 'James Peng'
tags: ['C#']
---

連結至 <http://www.mysql.org/downloads/connector/net/5.1.html>

  

下載Connector/Net 5.1組件安裝程式

  

下載檔案名稱預設為mysql-connector-net-5.1.3.zip

  
[![](http://bp3.blogger.com/_AnTT9cbXdqY/R00YMZ5vHZI/AAAAAAAAARY/vob3ESux-fw/s320/o_Ex3.1.png)](http://bp3.blogger.com/_AnTT9cbXdqY/R00YMZ5vHZI/AAAAAAAAARY/vob3ESux-fw/s1600-h/o_Ex3.1.png)  
  
台灣下載點：  
<http://ftp.ntu.edu.tw/pub/MySQL/Downloads/Connector-Net/mysql-connector-net-5.1.4.zip>  
  
安裝： Install MySQL Connector NET  
  
[![](http://bp0.blogger.com/_AnTT9cbXdqY/R00Yvp5vHaI/AAAAAAAAARg/4R8_Va3uvXI/s320/m0.PNG)](http://bp0.blogger.com/_AnTT9cbXdqY/R00Yvp5vHaI/AAAAAAAAARg/4R8_Va3uvXI/s1600-h/m0.PNG)  
[![](http://bp0.blogger.com/_AnTT9cbXdqY/R00Yvp5vHbI/AAAAAAAAARo/ukPFCNaBUNA/s320/m1.PNG)](http://bp0.blogger.com/_AnTT9cbXdqY/R00Yvp5vHbI/AAAAAAAAARo/ukPFCNaBUNA/s1600-h/m1.PNG)  
[![](http://bp1.blogger.com/_AnTT9cbXdqY/R00Yv55vHcI/AAAAAAAAARw/Cl7CuG-LdJc/s320/m2.PNG)](http://bp1.blogger.com/_AnTT9cbXdqY/R00Yv55vHcI/AAAAAAAAARw/Cl7CuG-LdJc/s1600-h/m2.PNG)  
[![](http://bp1.blogger.com/_AnTT9cbXdqY/R00Yv55vHdI/AAAAAAAAAR4/dzPS2ZeyC5A/s320/m3.PNG)](http://bp1.blogger.com/_AnTT9cbXdqY/R00Yv55vHdI/AAAAAAAAAR4/dzPS2ZeyC5A/s1600-h/m3.PNG)  
  
  
  
Copy MySql.Data.dll  
  
路徑：  
C:\\Program Files\\MySQL\\MySQL Connector Net 5.1.3\\Binaries\\.NET
2.0\\MySql.Data.dll  
copy到 「網頁目錄\\bin\\」之下  
  
修改C\#程式碼  
  
原本是用mssql  
現在把程式碼using部份改成  
//using System.Data.SqlClient;  
using MySql.Data.MySqlClient;  
  
SqlClient相關語法前面改加上My  
  
//SqlDataAdapter  
MySqlDataAdapter  
  
//SqlCommand  
MySqlCommand  
  
//連線字串改成以下  
string constr = "User Id=root;Host=localhost;Database

=qing;password=qing";  
 MySqlConnection mycn = new MySqlConnection(constr);

  
  
參考dll  
編譯時參考「MySql.Data.dll」  
/r:bin\\MySql.Data.dll  
  
 string constr = "User Id=root;Host=localhost;Database

=qing;password=qing";  
 MySqlConnection mycn = new MySqlConnection(constr);  
  
參考網頁：  
****[在 c\#
內連接mysql的方法](http://wane.ntit.edu.tw/?p=21 "Permanent Link:在 c# 內連接mysql的方法")  

