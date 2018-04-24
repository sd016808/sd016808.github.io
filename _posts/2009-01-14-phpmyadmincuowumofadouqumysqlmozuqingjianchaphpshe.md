---
layout: post
title: 'phpMyAdmin - 錯誤 無法讀取 mysql 模組,請檢查 PHP 設定 - 說明文件'
author: 'James Peng'
tags: ['IIS']
---

備註：若發生「無法載入mysql模組」  
[![](http://bp0.blogger.com/_AnTT9cbXdqY/R04X4Z5vHyI/AAAAAAAAAUg/RDNGrTN3r88/s320/p1.PNG)](http://bp0.blogger.com/_AnTT9cbXdqY/R04X4Z5vHyI/AAAAAAAAAUg/RDNGrTN3r88/s1600-h/p1.PNG)  
步驟一：  
將php.ini裡頭extension的  
extension=php\_gd2.dll  
extension=php\_mbstring.dll  
extension=php\_mysql.dll  
拿掉「;」啟用  
步驟二：  
再指定了extension 的正確位址(這是我安裝的位置)  
extension\_dir="c:\\php\\ext"  
步驟三：  
方法一：  
把以下copy到C:\\WINNT\\system32  
C:\\php\\ext\\php\_mysqli.dll  
C:\\php\\libmysql.dll  
方法二：  
把你的 PHP5 安裝路徑及底下的 ext 目錄加到系統的 PATH 環境變數下。  
如：PATH=c:\\php\\;c:\\php\\ext\\;  
這樣就不用複製檔案，而且 PHP 版本更新也不成問題。  
重開機 即可

