---
layout: post
title: '無法存取 IIS Metabase。'
author: 'James Peng'
tags: ['IIS']
---

[![image](http://lh4.ggpht.com/_AnTT9cbXdqY/SWRTfkLNWMI/AAAAAAAAGVA/c1_6EJV_tzY/image_thumb%5B10%5D.png?imgmax=800)](http://lh5.ggpht.com/_AnTT9cbXdqY/SWRTe-292ZI/AAAAAAAAGU8/EQuAsKKojlg/image%5B16%5D.png?imgmax=800)

**描述:** 在執行目前 Web
要求的過程中發生未處理的例外情形。請檢閱堆疊追蹤以取得錯誤的詳細資訊，以及在程式碼中產生的位置。  
**例外詳細資訊:** System.Web.Hosting.HostingEnvironmentException:
無法存取 IIS Metabase。  
**用來執行 ASP.NET 的處理序帳戶必須擁有 IIS Metabase (例如
IIS://servername/W3SVC) 的讀取權限。如需修改 Metabase
使用權限的詳細資訊，請參閱
<http://support.microsoft.com/?kbid=267904>。**  
**原始程式錯誤:**

`在執行目前 Web 要求期間，產生未處理的例外狀況。如需有關例外狀況來源與位置的資訊，可以使用下列的例外狀況堆疊追蹤取得。`

`==================================================================`

aspnet\_regiis -i  重新安裝 .net framework就可以了

[![image](http://lh5.ggpht.com/_AnTT9cbXdqY/SWRTh8d1csI/AAAAAAAAGVI/Ew33iVqU9Mk/image_thumb%5B9%5D.png?imgmax=800)](http://lh4.ggpht.com/_AnTT9cbXdqY/SWRThQuO8AI/AAAAAAAAGVE/DQJQMz-gSMw/image%5B15%5D.png?imgmax=800)

