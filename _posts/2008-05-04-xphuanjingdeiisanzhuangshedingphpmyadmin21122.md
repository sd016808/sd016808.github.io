---
layout: post
title: 'XP環境的IIS安裝設定phpMyAdmin-2.11.2.2'
author: 'James Peng'
tags: ['IIS']
---

官方網站下載：  
<http://www.phpmyadmin.net/home_page/downloads.php>  
  
[![](http://bp0.blogger.com/_AnTT9cbXdqY/R02Abp5vHuI/AAAAAAAAAUA/g4CknqDEg0E/s320/p1.PNG)](http://bp0.blogger.com/_AnTT9cbXdqY/R02Abp5vHuI/AAAAAAAAAUA/g4CknqDEg0E/s1600-h/p1.PNG)  
  
下載 all-languages-utf-8-only.zip  
  
  
安裝步驟  
  
 1. 下載並解壓縮 all-languages-utf-8-only.zip 至網頁資料夾中。  
 2. 將目錄名稱更改為 phpmyadmin，以方便日後版本升級並符合易記原則。  
 3. 安裝完成，這也是很簡單的。  
  
[![](http://bp2.blogger.com/_AnTT9cbXdqY/R02CHJ5vHvI/AAAAAAAAAUI/CP5w77oB9fw/s320/p2.PNG)](http://bp2.blogger.com/_AnTT9cbXdqY/R02CHJ5vHvI/AAAAAAAAAUI/CP5w77oB9fw/s1600-h/p2.PNG)  
  
  
設定步驟  
config.default.php 設定  
  
路徑  
D:\\WEB路徑\\phpMyAdmin\\libraries\\config.default.php  
  
[![](http://bp2.blogger.com/_AnTT9cbXdqY/R02ClJ5vHwI/AAAAAAAAAUQ/3c5w3zkZX4U/s320/p3.PNG)](http://bp2.blogger.com/_AnTT9cbXdqY/R02ClJ5vHwI/AAAAAAAAAUQ/3c5w3zkZX4U/s1600-h/p3.PNG)  
  
用記事本開啟 config.default.php，並按照以下原則進行修改。  
  
設定說明：  
  
 \* 搜尋字串\$cfg['Servers'][\$i]['auth\_type']。  
 \* 將其後的設定值由'config'變更為'http'。  
  
[![](http://bp2.blogger.com/_AnTT9cbXdqY/R02C7J5vHxI/AAAAAAAAAUY/M7jtgFAuKVE/s320/p4.PNG)](http://bp2.blogger.com/_AnTT9cbXdqY/R02C7J5vHxI/AAAAAAAAAUY/M7jtgFAuKVE/s1600-h/p4.PNG)  
  
  
基本操作說明  
  
 1. 以瀏覽器開啟 http://localhost/phpmyadmin/。  
 2. 帳號名稱為
root，密碼與安裝時設定的一樣，若未設定密碼，則保留空白即可。  
 3. 若能正確登入並看到管理畫面，表示 Apache、php、MySQL 以及 phpMyAdmin
皆已正確安裝完成。  
  
若要新增或變更密碼，請注意密碼雜湊必須選擇“MySQL 4.0
相容”，否則登出後可能無法再登入使用 phpMyAdmin。  
  
請牢記資料庫帳號密碼，在需要資料庫的各類架站軟體設定皆會用到。  
  
  
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
