---
layout: post
title: 'ASP.NET: 在Ubuntu Apache2 跑 mod-mono'
author: 'James Peng'
tags: ['ASP.NET']
---

1. 安裝mod-mono套件

> sudo apt-get install libapache2-mod-mono

2. 若你要使用asp.net 2.0要額外安裝以下套件(若不要就不用安裝)

> sudo apt-get install mono-apache-server2

3. 啟用模組

> sudo a2enmod mod\_mono

 

4.

這裡遇到一個問題，在安裝libapache2-mod-mono後會出現很久無法返回狀況，每次都是重啟來解決，重啟後，執行下一個命令時系統會提示

E:dpkg was interrupted ,you must manually run 'sudo dpkg --configure -a'
to correct the problem

造成這種提示原因就是剛才的安裝被中斷了,但是執行libapache2-mod-mono等好久都無法返回，  
所以只有  重開機 or   ssh進去 kill掉

ssh進去後

> root@james-VirtualBox:/home/james\# sudo apt-get install
> libapache2-mod-mono  
> E: 無法將 /var/lib/dpkg/lock 鎖定 - open (11: 資源暫時無法取得)  
> E: Unable to lock the administration directory (/var/lib/dpkg/), is
> another process using it?  
> <root@james-VirtualBox:/home/james>\#

同時間只能一個 apt-get 必須砍掉它

 

工作管理員

> ps -ax

 

7183 ?        S      0:01 apt-get install libapache2-mod-mono  
8337 pts/2    Ss+    0:00 /usr/bin/dpkg --status-fd 25 --configure
libmono-data  
8418 pts/2    S+     0:00 /usr/bin/perl -w /usr/share/debconf/frontend
/var/lib  
8425 pts/2    Z+     0:00 [mono-apache-ser] \<defunct\>  

 

關閉程式

> kill  -9 PID

 

kill -9 7183  
kill -9 8337  
kill -9 8418  
kill -9 8425

砍了四個，訊息變了

root@james-VirtualBox:/home/james\# sudo apt-get install
libapache2-mod-mono  
E: dpkg was interrupted, you must manually run 'sudo dpkg --configure
-a' to correct the problem.  

 

5.

移除 sudo dpkg -P  libapache2-mod-mono

重裝一次 sudo apt-get install mono-apache-server2

完成@@

 

5.

上傳了一個index.aspx文件到 /var/www/下面.運行時發現

> \<%@ Page Language="C\#" AutoEventWireup="true"
> CodeFile="index.aspx.cs" Inherits="index" %\>
>
> \<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
> "[http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"](http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd")\>
>
> \<html
> xmlns="[http://www.w3.org/1999/xhtml"](http://www.w3.org/1999/xhtml")\>  
> \<head runat="server"\>  
>     \<title\>\</title\>  
> \</head\>  
> \<body\>  
>     \<form id="form1" runat="server"\>  
>     \<div\>  
>     \<%  
>       Response.Write("test");   
>         %\>  
>     \</div\>  
>     \</form\>  
> \</body\>  
> \</html\>  

.net代碼根本沒執行,服務端控件基本無效

最後找到原因，mono沒有正確配置.net引擎目錄，當然不會執行.於是找到
/etc/apache2/mods-available/mod\_mono.conf 文件.

使用命令編輯

sudo vim /etc/apache2/mods-available/mod\_mono.conf

修改為:

AddType application/x-asp-net .aspx .ashx .asmx .ascx .asax .confi .ascx
.axd

DirectoryIndex index.aspx

MonoAutoApplication enabled

MonoServerPath "/usr/bin/mod-mono-server2"

Include /etc/mono-server2/mono-server2-hosts.conf

 

 

6. 重啟Apache  
sudo /etc/init.d/apache2 restart

7. 在 /var/www/目錄放置 asp.net 的專案或網頁,開啟流覽器鍵入  
<http://127.0.0.1/index.aspx> 即可看到正常顯示的 aspx 網頁

 

 

參考文件：

<http://www.mono-project.com/Mod_mono>

<https://help.ubuntu.com/community/ModMono>

<http://www.cnblogs.com/top5/archive/2011/02/23/1962504.html>

<http://alexzn168.wordpress.com/2011/03/17/%E5%9C%A8%EF%BD%95buntu-apache2-%E8%B7%91-mod-mono/>

