---
layout: post
title: '_CRT_SECURE_NO_WARNINGS'
author: 'James Peng'
tags: ['Visual C++']
---

開發時使用一些標準函數時會出現錯誤...

編譯錯誤訊息：_CRT_SECURE_NO_WARNINGS

~~~text
    1>GUIDlg.cpp(347): error C4996: 'asctime': This function or variable may be unsafe. Consider using asctime_s instead. To disable deprecation, use _CRT_SECURE_NO_WARNINGS. See online help for details.
    1>  C:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\include\time.h(174) : 請參閱 'asctime' 的宣告
~~~


----------

## 解決辦法： ##

1.對著右邊的 方案總管點右鍵 > 屬性 

![](..\images\2015-10-17-MFC_CRT_SECURE_NO_WARNINGS.\34J8xX3.png)


2.組態屬性 > C/C++ > 前置處理器 > 前置處理器定義，然後在後面加上 

~~~text
    _CRT_SECURE_NO_WARNINGS
~~~

![](..\images\2015-10-17-MFC_CRT_SECURE_NO_WARNINGS.\bWD1OL2.png)


3.就可以了...


----------

詳細可以參考：

- http://msdn.microsoft.com/zh-cn/library/8ef0s5kh.aspx
- http://msdn.microsoft.com/zh-cn/library/wd3wzwts%28v=vs.110%29.aspx
