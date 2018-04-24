---
layout: post
title: 'SQL2005 錯誤15023 使用者 建立失敗，孤兒使用者'
author: 'James Peng'
tags: ['MSSQL']
---

先在該table查詢  
exec sp\_change\_users\_login 'Report'

[![image](http://lh5.ggpht.com/pompom.new/SKVHl84IMUI/AAAAAAAADc0/GeO8OSemP8Q/image_thumb%5B1%5D.png?imgmax=800 "image")](http://lh4.ggpht.com/pompom.new/SKvB-eV7cRI/AAAAAAAADcw/oqbS9WF8FT4/s1600-h/image%5B2%5D.png)

查到有孤兒帳號  
再執行 exec sp\_changedbowner '使用者名'

[![image](http://lh5.ggpht.com/pompom.new/SKVHnti92oI/AAAAAAAADc8/gV72mogFfDE/image6_thumb%5B1%5D.png?imgmax=800 "image")](http://lh5.ggpht.com/pompom.new/SKVHmhVxdgI/AAAAAAAADc4/QjLT4D7bQNE/s1600-h/image6%5B2%5D.png)

參考：<http://support.microsoft.com/kb/314546/zh-tw>

