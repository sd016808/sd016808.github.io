---
layout: post
title: 'XP環境的IIS安裝設定 PHP 5.2.0'
author: 'James Peng'
tags: ['IIS']
---

連線至PHP官方網站下載頁面。  
<http://www.php.net/downloads.php>  
  
點擊PHP 5.2.0 zip package檔案連結。  

### 

[![](http://bp0.blogger.com/_AnTT9cbXdqY/R01mDp5vHeI/AAAAAAAAASA/15IYTh134Xs/s320/fetch.php.gif)](http://bp0.blogger.com/_AnTT9cbXdqY/R01mDp5vHeI/AAAAAAAAASA/15IYTh134Xs/s1600-h/fetch.php.gif)  
  
點選tw.php.net或tw2.php.net  
[![](http://bp0.blogger.com/_AnTT9cbXdqY/R01mkp5vHfI/AAAAAAAAASI/zRTNBW9ii_Y/s320/fetch.php.gif)](http://bp0.blogger.com/_AnTT9cbXdqY/R01mkp5vHfI/AAAAAAAAASI/zRTNBW9ii_Y/s1600-h/fetch.php.gif)  
  
按【儲存檔案】鈕。  
[![](http://bp2.blogger.com/_AnTT9cbXdqY/R01mxJ5vHgI/AAAAAAAAASQ/SpDa7GfVYxI/s320/fetch.php.gif)](http://bp2.blogger.com/_AnTT9cbXdqY/R01mxJ5vHgI/AAAAAAAAASQ/SpDa7GfVYxI/s1600-h/fetch.php.gif)  
  
開始下載。  
[![](http://bp3.blogger.com/_AnTT9cbXdqY/R01m6Z5vHhI/AAAAAAAAASY/7dkcN2nEAGg/s320/fetch.php.gif)](http://bp3.blogger.com/_AnTT9cbXdqY/R01m6Z5vHhI/AAAAAAAAASY/7dkcN2nEAGg/s1600-h/fetch.php.gif)  
  
下載完成。  
[![](http://bp2.blogger.com/_AnTT9cbXdqY/R01nCJ5vHiI/AAAAAAAAASg/ASeCgtHueEc/s320/fetch.php.gif)](http://bp2.blogger.com/_AnTT9cbXdqY/R01nCJ5vHiI/AAAAAAAAASg/ASeCgtHueEc/s1600-h/fetch.php.gif)  
  
解壓縮到【c:\\php】  
[![](http://bp3.blogger.com/_AnTT9cbXdqY/R01nKZ5vHjI/AAAAAAAAASo/JwQYjIoluBw/s320/fetch.php.gif)](http://bp3.blogger.com/_AnTT9cbXdqY/R01nKZ5vHjI/AAAAAAAAASo/JwQYjIoluBw/s1600-h/fetch.php.gif)  
  
找到【c:\\php\\php.ini-dist】  
[![](http://bp0.blogger.com/_AnTT9cbXdqY/R01nSp5vHkI/AAAAAAAAASw/hu0w8U3av64/s320/fetch.php.gif)](http://bp0.blogger.com/_AnTT9cbXdqY/R01nSp5vHkI/AAAAAAAAASw/hu0w8U3av64/s1600-h/fetch.php.gif)  
  
重新命名為【php.ini】。  
[![](http://bp2.blogger.com/_AnTT9cbXdqY/R01naJ5vHlI/AAAAAAAAAS4/LBO7QriLKSo/s320/fetch.php.gif)](http://bp2.blogger.com/_AnTT9cbXdqY/R01naJ5vHlI/AAAAAAAAAS4/LBO7QriLKSo/s1600-h/fetch.php.gif)  
  
將 php.ini 複製到【C:\\WinNT 】底下。  
[![](http://bp2.blogger.com/_AnTT9cbXdqY/R01njJ5vHmI/AAAAAAAAATA/SoWJfvX8Rac/s320/fetch.php.gif)](http://bp2.blogger.com/_AnTT9cbXdqY/R01njJ5vHmI/AAAAAAAAATA/SoWJfvX8Rac/s1600-h/fetch.php.gif)  
  
打開【Internet 服務管理員】，在Internet Information
Services底下「預設的網站」上按右鍵，點選「內容」。  
[![](http://bp3.blogger.com/_AnTT9cbXdqY/R01nsZ5vHnI/AAAAAAAAATI/fpCD0zmaagE/s320/fetch.php.gif)](http://bp3.blogger.com/_AnTT9cbXdqY/R01nsZ5vHnI/AAAAAAAAATI/fpCD0zmaagE/s1600-h/fetch.php.gif)  
  
點「主目錄」標籤下的【設定】鈕。  
[![](http://bp2.blogger.com/_AnTT9cbXdqY/R01n1J5vHoI/AAAAAAAAATQ/1kglGJDyp9M/s320/fetch.php.gif)](http://bp2.blogger.com/_AnTT9cbXdqY/R01n1J5vHoI/AAAAAAAAATQ/1kglGJDyp9M/s1600-h/fetch.php.gif)  
  
按【新增】鈕。  
[![](http://bp1.blogger.com/_AnTT9cbXdqY/R01n955vHpI/AAAAAAAAATY/YVghQl8gQ-k/s320/fetch.php.gif)](http://bp1.blogger.com/_AnTT9cbXdqY/R01n955vHpI/AAAAAAAAATY/YVghQl8gQ-k/s1600-h/fetch.php.gif)  
  
執行檔輸入框內輸入「C:\\PHP\\php5isapi.dll」，副檔名輸入框內輸入「.php」，  
動作與指令：選【限制於】填入「GET,HEAD,POST,TRACE」，按【確定】鈕。  
[![](http://bp2.blogger.com/_AnTT9cbXdqY/R01oJJ5vHqI/AAAAAAAAATg/UZBDpjFWFW8/s320/fetch.php.gif)](http://bp2.blogger.com/_AnTT9cbXdqY/R01oJJ5vHqI/AAAAAAAAATg/UZBDpjFWFW8/s1600-h/fetch.php.gif)  
  
新增.php應用程式對應後的畫面如下，按下【確定】鈕即可完成PHP
5.2.0的安裝。  
[![](http://bp0.blogger.com/_AnTT9cbXdqY/R01oSp5vHrI/AAAAAAAAATo/4xTKAouNGNk/s320/fetch.php.gif)](http://bp0.blogger.com/_AnTT9cbXdqY/R01oSp5vHrI/AAAAAAAAATo/4xTKAouNGNk/s1600-h/fetch.php.gif)  
  
順便新增個預設index.php。  
[![](http://bp1.blogger.com/_AnTT9cbXdqY/R01oa55vHsI/AAAAAAAAATw/gAAm0W3JUmQ/s320/fetch.php.gif)](http://bp1.blogger.com/_AnTT9cbXdqY/R01oa55vHsI/AAAAAAAAATw/gAAm0W3JUmQ/s1600-h/fetch.php.gif)  
  
  
最後試試 index.php  
  
原始碼貼上：  
  
  
\<?  
  
phpinfo();  
  
?\>  
  
[![](http://bp0.blogger.com/_AnTT9cbXdqY/R01ptp5vHtI/AAAAAAAAAT4/FXgYfV86chc/s320/%E6%9C%AA%E5%91%BD%E5%90%8D.PNG)](http://bp0.blogger.com/_AnTT9cbXdqY/R01ptp5vHtI/AAAAAAAAAT4/FXgYfV86chc/s1600-h/%E6%9C%AA%E5%91%BD%E5%90%8D.PNG)  
  
 2003請對php5isapi.dll這個檔案按右鍵，  
選擇”內容”⇒“安全性”⇒“加入users這個群組，  
使其有讀取，與執行兩種權限”⇒“確定”。

