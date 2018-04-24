---
layout: post
title: 'ATI CCC打不開的解決法'
author: 'James Peng'
tags: ['Windows']
---

  

解決方法如下：

Uninstalling CCC, 並且

1.檢查目錄中 (Program Files folder )/ATI Technologies/ATI.ACE  
  是否是空的

2.打開目錄 (Windows folder)/Assembly  
  找出所有 公開金鑰碼 為"90ba9c70f846762e"
(可以點擊公開金鑰..自動排列)  
  的所有文件 並在其上點滑鼠右鍵 選擇反安裝

 

這裡需要注意的是, 如果無法移除

[![image](http://lh6.ggpht.com/-N60cQbOxmA8/TqvgBkNANII/AAAAAAAALaY/T4ZMy2Ohj-g/image_thumb%25255B1%25255D.png?imgmax=800 "image")](http://lh6.ggpht.com/-lX9Yy3Lf_rg/TqvgBAq--AI/AAAAAAAALaU/KML3TZ8dQ_Q/s1600-h/image%25255B3%25255D.png)

 

可以透過 7-zip

[![image](http://lh3.ggpht.com/-wSYf8lS6GH4/TqvgDGMvUeI/AAAAAAAALao/pToV1Nqx3Ao/image_thumb%25255B3%25255D.png?imgmax=800 "image")](http://lh3.ggpht.com/-hr0cNcbRwUY/TqvgCd6-aGI/AAAAAAAALag/zrE52j4EM2s/s1600-h/image%25255B7%25255D.png)

進去目錄裡刪除 帶有90ba9c70f846762e的相關資料夾

 

3.檢查目錄中 (Document and Settings)/(User)/AppData/Local/ATI/ACE  
  是否是空的

4.重新安裝 英文版的CCC與其Driver

以上動作皆不需重開機

這邊附上出處網頁 以便如果有其它問題可以尋問原作者

<http://www.hardwareheaven.com/mobility-radeon-drivers-support/186707-problems-installing-catalyst-control-center.html>

以上

