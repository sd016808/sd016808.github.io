---
layout: post
title: 'C# 取得螢幕解析度'
author: 'James Peng'
tags: ['C#']
---


## C# 取得螢幕解析度 ##

~~~csharp
        private static string GetMonitorSize()
        {
            String Width = SystemInformation.PrimaryMonitorSize.Width.ToString();
            String Height = SystemInformation.PrimaryMonitorSize.Height.ToString();
            return Width + "*" + Height;
        }
~~~

