---
layout: post
title: 'C# 避免重複開啟應用程式'
author: 'James Peng'
tags: ['C#']
---

在開發Windows Form的時候，常常會需要限制使用者，一次只能開啟一個程式，實做方式是利用 .NET 內建的 Mutex 類別進行實做，幾乎任何情況下都能輕易實做程式不重複執行的目的，包括單機環境與多人使用的伺服器環境。

本篇採用具名的 Mutex 用法：在整台主機系統內實做 Mutex 機制。


## 先宣告 ##

- GUID 請自行更換，長度不得大於 260 個字元
- 在同一台主機相同使用者的範圍內進行 Mutex 互斥 ( 採用 Local\ 前置詞 )
- 在同一台主機所有使用者的範圍內進行 Mutex 互斥( 採用 Global\ 前置詞 )
- 如果你僅需限制互斥的範圍只需在「同使用者」的範圍內，只需將 Mutex 名稱的 Global\ 改成 Local\ 即可。

~~~csharp
        //Create This Program GUID
        //https://www.guidgenerator.com/online-guid-generator.aspx
        public const string GUID = @"{36972afb-b4df-4aae-90c2-fdba1094bfce}";

        public enum ProcessCheckRange
        {
            Global,
            Local
        }
~~~


----------

## 定義 IsProcessCanRun() 函式 ##

~~~csharp
        public static bool IsProcessCanRun(ProcessCheckRange type)
        {            
            string mutexRange = string.Empty;
            bool isCanRun = false;

            mutexRange = type.ToDescriptionString() + "\\";

            mutex = new System.Threading.Mutex(true, mutexRange + GUID, out isCanRun);

            return isCanRun;
        }
~~~


----------

## 用法 ##

- 在 Program.cs 應用程式的主要進入點 Main() 裡，加入以下code：

~~~csharp
            //使用時需傳入鎖定範圍是Global或是Local，即可依據回傳值判斷程式是否已在運行中 
            bool isCanRun = AppInfo.IsProcessCanRun(ProcessCheckRange.Global);
            if (isCanRun)
            {

            }
~~~

----------

參考：

- https://www.guidgenerator.com/online-guid-generator.aspx
- https://dotblogs.com.tw/kirkchen/2009/12/16/12487
- http://msdn.microsoft.com/zh-tw/library/bwe34f1k.aspx
- http://blog.miniasp.com/post/2009/10/How-to-avoid-Console-Application-or-WinForm-being-started-multiple-times.aspx
- http://bluemuta38.pixnet.net/blog/post/65305291-%E9%81%BF%E5%85%8D%E9%87%8D%E8%A4%87%E9%96%8B%E5%95%9Fap
- 
