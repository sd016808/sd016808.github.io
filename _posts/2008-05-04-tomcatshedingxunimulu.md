---
layout: post
title: 'Tomcat 設定虛擬目錄'
author: 'James Peng'
tags: ['Java']
---

前置作業 - 安裝Tomcat  
  
第1步：  
  
官方網站：<http://tomcat.apache.org/>  
  
[![](http://bp3.blogger.com/_AnTT9cbXdqY/RzpT2TAOUJI/AAAAAAAAAC4/PV0GLtQdn-M/s320/t1.PNG)](http://bp3.blogger.com/_AnTT9cbXdqY/RzpT2TAOUJI/AAAAAAAAAC4/PV0GLtQdn-M/s1600-h/t1.PNG)  
  
  
第2步：  
  
抓下win安裝檔  
[apache.ntu.edu.tw/tomcat/tomcat-6/v6.0.14/bin/apache-tomcat-6.0.14.exe](http://apache.ntu.edu.tw/tomcat/tomcat-6/v6.0.14/bin/apache-tomcat-6.0.14.exe)  
  
[![](http://bp1.blogger.com/_AnTT9cbXdqY/RzpVyzAOUKI/AAAAAAAAADA/rTGscNR0LnU/s320/%E6%9C%AA%E5%91%BD%E5%90%8D321.PNG)](http://bp1.blogger.com/_AnTT9cbXdqY/RzpVyzAOUKI/AAAAAAAAADA/rTGscNR0LnU/s1600-h/%E6%9C%AA%E5%91%BD%E5%90%8D321.PNG)  
  
第3步：  
  
安裝完成  
  
[![](http://bp3.blogger.com/_AnTT9cbXdqY/RzpWoTAOULI/AAAAAAAAADI/qhdsRGa3cL4/s320/t22.PNG)](http://bp3.blogger.com/_AnTT9cbXdqY/RzpWoTAOULI/AAAAAAAAADI/qhdsRGa3cL4/s1600-h/t22.PNG)  
  
虛擬目錄設置開始  
  
第1步：  
  
開啟 Server.xml  
[![](http://bp3.blogger.com/_AnTT9cbXdqY/RzpX9TAOUMI/AAAAAAAAADQ/iI3-wYvSfLc/s320/t3.PNG)](http://bp3.blogger.com/_AnTT9cbXdqY/RzpX9TAOUMI/AAAAAAAAADQ/iI3-wYvSfLc/s1600-h/t3.PNG)  
  
第2步：  
找到 host 之間  
  
[![](http://bp1.blogger.com/_AnTT9cbXdqY/RzpZVzAOUNI/AAAAAAAAADY/Kwsd8__3NXk/s320/t4.PNG)](http://bp1.blogger.com/_AnTT9cbXdqY/RzpZVzAOUNI/AAAAAAAAADY/Kwsd8__3NXk/s1600-h/t4.PNG)  
  
第3步：  
加入\<Context debug="0"
docBase="D:\\\\ServletSample\_v1\\ServletSample\_v1" path="/csie" /\>  
[![](http://bp2.blogger.com/_AnTT9cbXdqY/Rzpf7DAOUOI/AAAAAAAAADg/yGPxHZy8zvM/s320/t5.PNG)](http://bp2.blogger.com/_AnTT9cbXdqY/Rzpf7DAOUOI/AAAAAAAAADg/yGPxHZy8zvM/s1600-h/t5.PNG)  
  
第4步：  
瀏覽器打上對應網址 http://127.0.0.1:8080/csie 就出現了  
[![](http://bp2.blogger.com/_AnTT9cbXdqY/RzpgmDAOUPI/AAAAAAAAADo/lj0pP6mUry0/s320/t6.PNG)](http://bp2.blogger.com/_AnTT9cbXdqY/RzpgmDAOUPI/AAAAAAAAADo/lj0pP6mUry0/s1600-h/t6.PNG)  
  
其他方法 - 放到預設路徑  
  
第1步：  
開啟路徑  
C:\\Program Files\\Apache Software Foundation\\Tomcat 6.0\\webapps  
[![](http://bp2.blogger.com/_AnTT9cbXdqY/RzpjSDAOUQI/AAAAAAAAADw/PrPpyKAcMxc/s320/t7.PNG)](http://bp2.blogger.com/_AnTT9cbXdqY/RzpjSDAOUQI/AAAAAAAAADw/PrPpyKAcMxc/s1600-h/t7.PNG)  
  
第2步：  
把整個資料夾 csie 放 webapps 底下  
[![](http://bp3.blogger.com/_AnTT9cbXdqY/RzpvysrlhyI/AAAAAAAAAD4/F8QijxV_XLQ/s320/t8.PNG)](http://bp3.blogger.com/_AnTT9cbXdqY/RzpvysrlhyI/AAAAAAAAAD4/F8QijxV_XLQ/s1600-h/t8.PNG)  
  
第3步：  
瀏覽器打上對應網址 http://127.0.0.1:8080/csie 就出現了  
[![](http://bp2.blogger.com/_AnTT9cbXdqY/RzpgmDAOUPI/AAAAAAAAADo/lj0pP6mUry0/s320/t6.PNG)](http://bp2.blogger.com/_AnTT9cbXdqY/RzpgmDAOUPI/AAAAAAAAADo/lj0pP6mUry0/s1600-h/t6.PNG)
