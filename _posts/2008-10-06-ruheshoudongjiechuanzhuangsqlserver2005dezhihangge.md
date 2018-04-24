---
layout: post
title: '如何手動解除安裝 SQL Server 2005 的執行個體'
author: 'James Peng'
tags: ['MSSQL']
---

參考：<http://support.microsoft.com/kb/909967>  
  
1.先停止服務  
[![image](http://lh6.ggpht.com/pompom.new/SOoFYZnClbI/AAAAAAAAFWg/rpbzTfHfHkM/image_thumb%5B15%5D.png?imgmax=800 "image")](http://lh3.ggpht.com/pompom.new/SOoFXLacDEI/AAAAAAAAFWc/xLYoGqAX6ZU/s1600-h/image%5B24%5D.png)

2.開始 –\> 執行

"C:\\Program Files\\Microsoft SQL Server\\90\\Setup
Bootstrap\\ARPWrapper.exe" /Remove

[![image](http://lh6.ggpht.com/pompom.new/SOoFaBOH4WI/AAAAAAAAFWo/O76aLuaBm-o/image_thumb%5B3%5D.png?imgmax=800 "image")](http://lh6.ggpht.com/pompom.new/SOoFZfPmcaI/AAAAAAAAFWk/Ucjr8O83yjY/s1600-h/image%5B7%5D.png)

 

3.選好 按下一步

[![image](http://lh6.ggpht.com/pompom.new/SOoFdPS3z8I/AAAAAAAAFWw/jitHr89zwxo/image_thumb%5B1%5D.png?imgmax=800 "image")](http://lh6.ggpht.com/pompom.new/SOoFbeMuGTI/AAAAAAAAFWs/yzmGwMy_g6g/s1600-h/image%5B3%5D.png)

[![image](http://lh6.ggpht.com/pompom.new/SOoFfFcn-jI/AAAAAAAAFW4/jpEtK70KcuY/image_thumb%5B5%5D.png?imgmax=800 "image")](http://lh5.ggpht.com/pompom.new/SOoFeKj6vAI/AAAAAAAAFW0/WoZpdwOxMp4/s1600-h/image%5B11%5D.png)

[![image](http://lh5.ggpht.com/pompom.new/SOoFhbkBY9I/AAAAAAAAFXA/g1Zq6VFbETs/image_thumb%5B7%5D.png?imgmax=800 "image")](http://lh5.ggpht.com/pompom.new/SOoFgLbCB2I/AAAAAAAAFW8/dHaW2B0-k-A/s1600-h/image%5B15%5D.png)

[![image](http://lh4.ggpht.com/pompom.new/SOoFla-AQCI/AAAAAAAAFXM/iH2iPkyGfp4/image_thumb%5B9%5D.png?imgmax=800 "image")](http://lh4.ggpht.com/pompom.new/SOoFjHVxs2I/AAAAAAAAFXE/aPLHVgNaBgY/s1600-h/image%5B19%5D.png)

 

移除了一個！

**注意** [新增或移除程式] 也可以使用 **/Remove** 選項，執行這個
ARPWrapper.exe 程式。然而，ARPWrapper.exe 程式的參考卻可能已經遭到刪除。

先重新安裝「Microsoft SQL Server 安裝程式支援檔案」元件
(SqlSupport.msi)，然後再解除安裝這個執行個體中的每個元件。

光碟路徑G:\\Setup\\SqlSupport.msi

 

再次移除另一個：

[![image](http://lh5.ggpht.com/pompom.new/SOoIV6mzhDI/AAAAAAAAFXU/8ZEbK7HeREQ/image_thumb%5B1%5D.png?imgmax=800 "image")](http://lh3.ggpht.com/pompom.new/SOoIUl76GYI/AAAAAAAAFXQ/eycT0DWT6O8/s1600-h/image%5B4%5D.png)

