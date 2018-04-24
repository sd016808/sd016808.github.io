---
layout: post
title: 'C++ 取得當前執行檔 路徑'
author: 'James Peng'
tags: ['Visual C++']
---

~~~cpp
CString MyFunc::GetAppDir() 
{ 
	CString m_strAppPath; 
	if (::GetModuleFileName(NULL, m_strAppPath.GetBuffer(MAX_PATH), MAX_PATH) == 0) 
	{ 
	  m_strAppPath.ReleaseBuffer(); 
	  m_strAppPath = _T(""); 
	} 
	else 
	{ 
	  m_strAppPath.ReleaseBuffer(); 
	  m_strAppPath.Format(m_strAppPath.Left(m_strAppPath.ReverseFind('\\'))); 
	} 
	return m_strAppPath; 
}
~~~
