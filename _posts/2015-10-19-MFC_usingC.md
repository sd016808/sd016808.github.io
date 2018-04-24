---
layout: post
title: 'MFC裡使用C Code'
author: 'James Peng'
tags: ['Visual C++']
---

The MSVC++ compiler has a feature which pre-compiles header files to speed up compilation time. The header file stdafx.h centralizes all the pre-compiled data into one place. Normally, MFC applications requires that #include "stdafx.h" be added to each source file that requires these headers.

![](..\images\2015-10-19-MFC_usingC\epXbXd9.png)

![](..\images\2015-10-19-MFC_usingC\vxPAl62.png)

![](..\images\2015-10-19-MFC_usingC\2Y1ReWq.png)


------------

## 參考: ##

* http://www.totalphase.com/support/articles/200349216-Writing-an-MFC-application-using-the-C-API
* https://totalphase.zendesk.com/hc/en-us/articles/200348526-Using-the-C-API-in-Visual-C-NET
