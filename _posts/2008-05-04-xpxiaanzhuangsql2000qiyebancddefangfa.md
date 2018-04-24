---
layout: post
title: 'XP下安裝SQL2000企業版CD的方法'
author: 'James Peng'
tags: ['MSSQL']
---

SQL2000企業版光碟只適用於WIN2000、2003作業系統，XP通常是無法安裝  
若是你又非用不可，而且不想重灌作業系統，也沒有別的版本的SQL  
這裡介紹一個XP下安裝SQL2000企業版CD的方法，以供參考\~  
  
步驟如下：  
  
一．在SQL伺服器的安裝光碟中找到MSDE這個目錄。  
  
[![](http://bp2.blogger.com/_AnTT9cbXdqY/R1OZRpjHjCI/AAAAAAAAAW0/2sUszyXS27E/s320/fetch.php.gif)](http://bp2.blogger.com/_AnTT9cbXdqY/R1OZRpjHjCI/AAAAAAAAAW0/ssasScDIf48/s1600-R/fetch.php.gif)  
  
並且點擊setup.exe安裝它，過程簡單直接就ＯＫ了  
[![](http://bp0.blogger.com/_AnTT9cbXdqY/R1ObPJjHjDI/AAAAAAAAAW8/W7evY2Uev_k/s320/fetch.php.gif)](http://bp0.blogger.com/_AnTT9cbXdqY/R1ObPJjHjDI/AAAAAAAAAW8/TC4lz-R3Uow/s1600-R/fetch.php.gif)  
  
二. 重新開機WINDOWS  
[![](http://bp3.blogger.com/_AnTT9cbXdqY/R1ObW5jHjEI/AAAAAAAAAXE/rnKufXNY2eE/s320/fetch.php.gif)](http://bp3.blogger.com/_AnTT9cbXdqY/R1ObW5jHjEI/AAAAAAAAAXE/btLb-n_PvTE/s1600-R/fetch.php.gif)  
  
這下就可以看到SQL服務的圖示出現了。  
[![](http://bp0.blogger.com/_AnTT9cbXdqY/R1ObeJjHjFI/AAAAAAAAAXM/eIxPzEiUFL0/s320/fetch.php.gif)](http://bp0.blogger.com/_AnTT9cbXdqY/R1ObeJjHjFI/AAAAAAAAAXM/t3JFhiDU83Q/s1600-R/fetch.php.gif)  
  
三. 再拿出SQL伺服器版的安裝光碟，直接安裝客戶端工具  
（這個不要多說吧？最簡單的方法就是直接點擊光碟根目錄下的autorun.exe)  
[![](http://bp1.blogger.com/_AnTT9cbXdqY/R1OccZjHjGI/AAAAAAAAAXU/m9BTtwC2EJQ/s320/fetch.php.gif)](http://bp1.blogger.com/_AnTT9cbXdqY/R1OccZjHjGI/AAAAAAAAAXU/wg0xyPQhWeo/s1600-R/fetch.php.gif)  
  
根據提示安裝，檢測過程中知道作業系統不是SERVER版，會提示只能安裝客戶端工具。（哈，伺服端剛剛偷偷裝了）  
[![](http://bp1.blogger.com/_AnTT9cbXdqY/R1OcnZjHjHI/AAAAAAAAAXc/4288nd-4oCw/s320/fetch.php.gif)](http://bp1.blogger.com/_AnTT9cbXdqY/R1OcnZjHjHI/AAAAAAAAAXc/OnPf3SNpnmA/s1600-R/fetch.php.gif)  
  
安裝時 跳出「先前的程式安裝在安裝機制上建立了擱置檔案作業」  
[![](http://bp0.blogger.com/_AnTT9cbXdqY/R1OcvJjHjII/AAAAAAAAAXk/RLa0iWQO4_8/s320/fetch.php.gif)](http://bp0.blogger.com/_AnTT9cbXdqY/R1OcvJjHjII/AAAAAAAAAXk/j3RX9yyUhN4/s1600-R/fetch.php.gif)  
  
在[執行]中輸入regedit打開註冊表編輯器  
[![](http://bp0.blogger.com/_AnTT9cbXdqY/R1Oc5JjHjJI/AAAAAAAAAXs/azt9naRss50/s320/fetch.php.gif)](http://bp0.blogger.com/_AnTT9cbXdqY/R1Oc5JjHjJI/AAAAAAAAAXs/GSN0ERElXmU/s1600-R/fetch.php.gif)  
  
請直接刪掉這個機碼，就可以繼續安裝了  
HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Session
Manager\\PendingFileRenameOperations  
[![](http://bp1.blogger.com/_AnTT9cbXdqY/R1OdIZjHjKI/AAAAAAAAAX0/GdLHwwAyPwc/s320/fetch.php.png)](http://bp1.blogger.com/_AnTT9cbXdqY/R1OdIZjHjKI/AAAAAAAAAX0/Zp7r1HsKEnQ/s1600-R/fetch.php.png)  
  
安裝好了  
[![](http://bp1.blogger.com/_AnTT9cbXdqY/R1OdwZjHjLI/AAAAAAAAAX8/s0eeXIY5eP0/s320/fetch.php.png)](http://bp1.blogger.com/_AnTT9cbXdqY/R1OdwZjHjLI/AAAAAAAAAX8/MVl6iIj5QGk/s1600-R/fetch.php.png)  
  
我直接打開 Enterprise Manager 可以登入sa  
[![](http://bp2.blogger.com/_AnTT9cbXdqY/R1OeJpjHjMI/AAAAAAAAAYE/Xd6i4JDjSCY/s320/fetch.php.png)](http://bp2.blogger.com/_AnTT9cbXdqY/R1OeJpjHjMI/AAAAAAAAAYE/2t37AwHM-jw/s1600-R/fetch.php.png)  
  
成功!!  
  
  
附錄  
  
若是打開 Enterprise Manager，試用SA用戶登陸失敗的話  
這只要對系統登陸檔稍加修改就可以啦：  
  
找到[HKEY\_LOCAL\_MACHINE\\SOFTWARE\\MICROSOFT\\MSSQLSERVER\\MSSQLSERVER]  
  
[![](http://bp2.blogger.com/_AnTT9cbXdqY/R1OeUpjHjNI/AAAAAAAAAYM/_V013s8gG5o/s320/fetch.php.png)](http://bp2.blogger.com/_AnTT9cbXdqY/R1OeUpjHjNI/AAAAAAAAAYM/qO4HbHrT5bE/s1600-R/fetch.php.png)  
  
這個項裡面有一個鍵值LoginMode  
預設值是1，現在將值改為2，重新啟動電腦。  
  
再打開Enterprise Manager，再連接試試，是不是OK了呢！
