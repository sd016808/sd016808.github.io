---
layout: post
title: 'Ubuntu LAMP 基本架站'
author: 'James Peng'
tags: ['Linux']
---

首先先安裝apache2

sudo apt-get install apache2

輸入http://網址 （http://127.0.0.1/)

如果看到 It works! 代表安裝成功

 

sudo /etc/init.d/apache2 restart //重啟APACHE

> 錯誤 apache2: Cou ld not reli ably determine the server's fully
> qualified domain name, using 127.0.1.1 for ServerName  
> 解決辦法  
> 編輯/etc/apache2/httpd.conf文件，加入  
> ServerName localhost  
> 或者  
> ServerName 127.0.0.1

* * * * *

安裝php的話可以用來架設 blog

sudo apt-get install php5 libapache2-mod-php5 php5-mysql php5-gd

sudo /etc/init.d/apache2 restart //重啟APACHE

> 以下可選擇安裝：  
> sudo apt-get install php5-curl  
> sudo apt-get install php5-xmlrpc  
> sudo apt-get install php5-imagick  
> sudo apt-get install php5-cgi  
> sudo apt-get install php5-fpm  
> sudo apt-get install php5-memcache  
> sudo apt-get install php5-xdebug

* * * * *

放一個 php 的網頁到主機去

sudo vim /var/www/phpinfo.php

鍵盤上面的 i 進入編輯模式

之後輸入 \<? phpinfo();?\>  
輸入完成以後就按下 鍵盤上面的 ESC 鍵

之後輸入 :wq 存檔離開

開啟瀏覽器來測試 php 是否正常

http://網址/phpinfo.php（ http://127.0.0.1/phpinfo.php ）

* * * * *

安裝mysql部份

sudo apt-get install mysql-server mysql-client phpmyadmin

sudo /etc/init.d/apache2 restart //重啟APACHE  

> 打開<http://localhost/phpmyadmin/>這個鏈接出現 Not Found
>
> sudo ln -s /usr/share/phpmyadmin/ /var/www/
>
> phpmyadmin的默認安裝路徑不是在/var/www/（/var/www/是你的web服務站點的根目錄），所以建一個軟連接就可以了。
>
> 上述命令是在/var/www/下建一個phpmyadmin的軟鏈接。

 

> mysqladmin -u root password db\_user\_password  
> \#db\_user\_password替換為密碼

安裝完成！

 

參考：<http://blog.jsdan.com/735>

<http://blog.phpxx.net/?p=196>

