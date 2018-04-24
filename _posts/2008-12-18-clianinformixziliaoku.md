---
layout: post
title: 'C#連Informix資料庫'
author: 'James Peng'
tags: ['C# 3rd-Party Library']
---

先到 IBM 官網下載 Informix Client Software Development Kit (Client SDK)

CSDK官方網址：<http://www-01.ibm.com/software/data/informix/tools/csdk/>

下載頁面：<http://www14.software.ibm.com/webapp/download/search.jsp?rs=ifxdl>

選好要下載的版本後

[![image](http://lh3.ggpht.com/_AnTT9cbXdqY/SUmeENHBIlI/AAAAAAAAGFA/3ABGB7AylDc/image_thumb%5B18%5D.png?imgmax=800 "image")](http://lh5.ggpht.com/_AnTT9cbXdqY/SUmeDGlLM-I/AAAAAAAAGE8/sH6rBJh-z_o/s1600-h/image%5B7%5D.png)

#####  

> 1.  
> [Informix Downloads (IBM Informix Client SDK V3.50.TC3 for Windows
> 2003, XP, Vista,
> 32bit)](http://www14.software.ibm.com/webapp/download/preconfig.jsp?id=2008-05-15+10%3A17%3A15.804015R&S_TACT=104CBW71&S_CMP=)
>
> A single packaging of several application programming interfaces
> (APIs) for rapid, cost-effective development of applications for IBM
> Informix servers.
>
> 3.50.TC3  
> Released product  
> 07 Nov 2008  
> 74MB  
> Windows Server 2003, Windows Vista Family, Windows XP

下載好後，安裝完成。只為了COPY那個 IBM.Data.Informix.dll

#### [![image](http://lh3.ggpht.com/_AnTT9cbXdqY/SUmeFRT8MyI/AAAAAAAAGFI/OgOLszw9FpE/image_thumb%5B19%5D.png?imgmax=800 "image")](http://lh5.ggpht.com/_AnTT9cbXdqY/SUmeEip5ouI/AAAAAAAAGFE/v-hNMmSsSTQ/s1600-h/image%5B10%5D.png)

 


----------


### 如何建立連線：

  
[C\#]  

~~~csharp
using IBM.Data.Informix;
~~~

連線字串

~~~csharp
     string ConnectionString = "Host=" + HOST + "; " +     "Service=" + SERVICENUM + "; " +     "Server=" + SERVER + "; " +     "Database=" + DATABASE + "; " +     "User Id=" + USER + "; " +     "Password=" + PASSWORD + "; ";
~~~

常用classes

  

-   [IfxDataAdapter](http://publib.boulder.ibm.com/infocenter/idshelp/v111/topic/com.ibm.net_cc.doc/dpref-ifx51.htm#dqx1db2dataadapterclass)  
-   [IfxCommand](http://publib.boulder.ibm.com/infocenter/idshelp/v111/topic/com.ibm.net_cc.doc/dpref-ifx47.htm#dqx1db2commandclass)  
-   [IfxConnection](http://publib.boulder.ibm.com/infocenter/idshelp/v111/topic/com.ibm.net_cc.doc/dpref-ifx48.htm#dqx1db2connectionclass)  
-   [IfxDataReader](http://publib.boulder.ibm.com/infocenter/idshelp/v111/topic/com.ibm.net_cc.doc/dpref-ifx52.htm#dqx1db2datareaderclass)

  



----------

參考：

- [Connect to Informix with ADO.NET](http://www.ibm.com/developerworks/data/library/techarticle/dm-0510durity/)
- [IBM Data Server Provider for .NET for Informix Dynamic Server](http://publib.boulder.ibm.com/infocenter/idshelp/v111/index.jsp?topic=/com.ibm.net_cc.doc/dpref-ifx41.htm)
