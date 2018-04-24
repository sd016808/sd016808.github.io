---
layout: post
title: 'ASP.NET: Request.ServerVariables 檢視伺服器環境變數的值'
author: 'James Peng'
tags: ['ASP.NET']
---


~~~csharp

Request.ServerVariables("Url") ;//返回服務器地址
Request.ServerVariables("Path_Info") ;//客戶端提供的路徑信息
Request.ServerVariables("Appl_Physical_Path") ;//與應用程序元數據庫路徑相應的物理路徑
Request.ServerVariables("Path_Translated") ;//通過由虛擬至物理的映射後得到的路徑
Request.ServerVariables("Script_Name") ;//執行腳本的名稱
Request.ServerVariables("Query_String");//查詢字符串內容
Request.ServerVariables("Http_Referer") ;//請求的字符串內容
Request.ServerVariables("Server_Port") ;//接受請求的服務器端口號
Request.ServerVariables("Remote_Addr") ;//發出請求的遠程主機的IP地址
Request.ServerVariables("Remote_Host") ;//發出請求的遠程主機名稱
Request.ServerVariables("Local_Addr") ;//返回接受請求的服務器地址
Request.ServerVariables("Http_Host") ;//返回服務器地址
Request.ServerVariables("Server_Name");//服務器的主機名、DNS地址或IP地址
Request.ServerVariables("Request_Method");//提出請求的方法比如GET、HEAD、POST等等
Request.ServerVariables("Server_Port_Secure");//如果接受請求的服務器端口為安全端口時，則為1，否則為0
Request.ServerVariables("Server_Protocol");//服務器使用的協議的名稱和版本
Request.ServerVariables("Server_Software");//應答請求並運行網關的服務器軟件的名稱和版本
Request.ServerVariables("All_Http");//客戶端發送的所有HTTP標頭，前綴HTTP_
Request.ServerVariables("All_Raw");//客戶端發送的所有HTTP標頭,其結果和客戶端發送時一樣，沒有前綴HTTP_
Request.ServerVariables("Appl_MD_Path");//應用程序的元數據庫路徑
Request.ServerVariables("Content_Length");//客戶端發出內容的長度
Request.ServerVariables("Https");//如果請求穿過安全通道（SSL），則返回ON如果請求來自非安全通道，則返回OFF
Request.ServerVariables("Instance_ID");// IIS實例的ID號
Request.ServerVariables("Instance_Meta_Path");//響應請求的IIS實例的元數據庫路徑
Request.ServerVariables("Http_Accept_Encoding");//返回內容如：gzip,deflate
Request.ServerVariables("Http_Accept_Language");//返回內容如：en-us
Request.ServerVariables("Http_Connection");//返回內容：Keep-Alive Request.ServerVariables("Http_Cookie")
Request.ServerVariables("Http_User_Agent");//返回內容：Mozilla/4.0(compatible;MSIE6.0;WindowsNT5.1;SV1)
Request.ServerVariables("Https_Keysize");//安全套接字層連接關鍵字的位數，如128
Request.ServerVariables("Https_Secretkeysize");//服務器驗證私人關鍵字的位數如1024
Request.ServerVariables("Https_Server_Issuer");//服務器證書的發行者字段
Request.ServerVariables("Https_Server_Subject");//服務器證書的主題字段
Request.ServerVariables("Auth_Password");//當使用基本驗證模式時，客戶在密碼對話框中輸入的密碼
Request.ServerVariables("Auth_Type");//是用戶訪問受保護的腳本時，服務器用於檢驗用戶的驗證方法
Request.ServerVariables("Auth_User");//代證的用戶名Request.ServerVariables("Cert_Cookie")唯一的客戶證書ID號
Request.ServerVariables("Cert_Flag");//客戶證書標誌，如有客戶端證書，則bit0為0如果客戶端證書驗證無效，bit1被設置為1
Request.ServerVariables("Cert_Issuer");//用戶證書中的發行者字段
Request.ServerVariables("Cert_Keysize");//安全套接字層連接關鍵字的位數，如128
Request.ServerVariables("Cert_Secretkeysize");//服務器驗證私人關鍵字的位數如1024
Request.ServerVariables("Cert_Serialnumber");//客戶證書的序列號字段
Request.ServerVariables("Cert_Server_Issuer");//服務器證書的發行者字段
Request.ServerVariables("Cert_Server_Subject");//服務器證書的主題字段
Request.ServerVariables("Cert_Subject");//客戶端證書的主題字段
Request.ServerVariables("Content_Type");//客戶發送的form內容或HTTPPUT的數據類型

~~~


----------


## 用法 ##

~~~csharp
string s =Request.ServerVariables["HTTP_ACCEPT_LANGUAGE"].ToString(); Response.Write(s);
~~~
