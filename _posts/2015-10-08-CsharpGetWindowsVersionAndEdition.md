---
layout: post
title: 'C# – Get Windows Version and Edition'
author: 'James Peng'
tags: ['C#']
---


~~~csharp
[DllImport("Kernel32.dll")]
internal static extern bool GetProductInfo(int osMajorVersion,int osMinorVersion,int spMajorVersion,int spMinorVersion,out int edition);
[DllImport("kernel32.dll")]
private static extern bool GetVersionEx(ref OSVERSIONINFOEX osVersionInfo);

~~~

SOURCE:

[https://github.com/jia-hong-peng/Windows-Version-Edition](https://github.com/jia-hong-peng/Windows-Version-Edition)



參考：

- http://hintdesk.com/c-get-windows-version-and-edition/
- https://msdn.microsoft.com/en-us/library/ms724358.aspx
- http://www.codeguru.com/cpp/misc/misc/system/article.php/c8973/Determine-Windows-Version-and-Edition.htm#page-1
