---
layout: post
title: 'C# Kill Process'
author: 'James Peng'
tags: ['C#']
---

## 使用方法 ##

例如: 要 Kill Adobe PDF 的 Process

先找出 Process 名稱

![](..\images\2016-03-18-CSharp_KillProcess\iLKBQ1N.png)

~~~csharp
        public static void KillPdfProcess()
        {
            System.Diagnostics.Process[] processes = System.Diagnostics.Process.GetProcessesByName("AcroRd32");
            foreach (System.Diagnostics.Process process in processes)
            {
                process.Kill();
            }
        }
~~~



