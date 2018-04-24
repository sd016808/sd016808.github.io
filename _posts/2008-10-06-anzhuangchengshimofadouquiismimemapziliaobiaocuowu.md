---
layout: post
title: '安裝程式無法讀取 IIsMimeMap 資料表。 錯誤碼為 -2147024893。'
author: 'James Peng'
tags: ['MSSQL']
---

移除Reporting Services 發生錯誤  
[![image](http://lh6.ggpht.com/pompom.new/SOoLhwmghtI/AAAAAAAAFXc/igxD_Sy4za0/image_thumb%5B1%5D.png?imgmax=800 "image")](http://lh3.ggpht.com/pompom.new/SOoLgVODSUI/AAAAAAAAFXY/JjNhhzIsDq4/s1600-h/image%5B3%5D.png)

參考：<http://support.microsoft.com/kb/910544/zh-tw>

方法一：  
開始 –\> 執行  
msiexec /fav SqlRun\_RS.msi

[![image](http://lh3.ggpht.com/pompom.new/SOoLkaaz6iI/AAAAAAAAFXk/CdTn35b4nzA/image_thumb%5B3%5D.png?imgmax=800 "image")](http://lh3.ggpht.com/pompom.new/SOoLi1Tx3UI/AAAAAAAAFXg/YqzM5E_iN8Y/s1600-h/image%5B7%5D.png)

 

方法二：

重新利用 Reporting Services 組態工具為 SQL Server 2005 Reporting
Services 建立虛擬目錄。 在您重新建立虛擬目錄， 您可以解除安裝或修復的
SQL Server 2005 Reporting Services 安裝成功。

[![image](http://lh4.ggpht.com/pompom.new/SOoLmVn0KSI/AAAAAAAAFXs/yuwQG03CT-4/image_thumb%5B14%5D.png?imgmax=800 "image")](http://lh5.ggpht.com/pompom.new/SOoLldnF5JI/AAAAAAAAFXo/6Ui2lK1iiSw/s1600-h/image%5B23%5D.png)

[![image](http://lh6.ggpht.com/pompom.new/SOoLpCC7IcI/AAAAAAAAFX0/dS4paYFgFpA/image_thumb%5B6%5D.png?imgmax=800 "image")](http://lh4.ggpht.com/pompom.new/SOoLnspT7NI/AAAAAAAAFXw/COv_QB_iinY/s1600-h/image%5B14%5D.png)

[![image](http://lh4.ggpht.com/pompom.new/SOoLsLQ11WI/AAAAAAAAFX8/Pw6EX2CWFKE/image_thumb%5B8%5D.png?imgmax=800 "image")](http://lh3.ggpht.com/pompom.new/SOoLqlGOvZI/AAAAAAAAFX4/-dQlcxJtklw/s1600-h/image%5B18%5D.png)

 

G:\\Setup\\SqlRun\_RS.msi

"C:\\Program Files\\Microsoft SQL Server\\90\\Setup
Bootstrap\\ARPWrapper.exe" /Remove

[![image](http://lh4.ggpht.com/pompom.new/SOoLushr33I/AAAAAAAAFYE/5ZEZk3H6gOI/image_thumb%5B16%5D.png?imgmax=800 "image")](http://lh3.ggpht.com/pompom.new/SOoLtcgHw4I/AAAAAAAAFYA/2v1-_fypWnI/s1600-h/image%5B27%5D.png)

[![image](http://lh5.ggpht.com/pompom.new/SOoLxnmIftI/AAAAAAAAFYM/SRa0sxj30kM/image_thumb%5B18%5D.png?imgmax=800 "image")](http://lh6.ggpht.com/pompom.new/SOoLvjyxRkI/AAAAAAAAFYI/tTe4ouR-GBA/s1600-h/image%5B31%5D.png)

[![image](http://lh4.ggpht.com/pompom.new/SOoLzvtT0tI/AAAAAAAAFYU/TlsPLhsBqEg/image_thumb%5B21%5D.png?imgmax=800 "image")](http://lh4.ggpht.com/pompom.new/SOoLyleXjiI/AAAAAAAAFYQ/p0FVbOFDBw0/s1600-h/image%5B36%5D.png)

移除完成

