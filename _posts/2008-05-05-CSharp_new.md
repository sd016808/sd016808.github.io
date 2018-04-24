---
layout: post
title: 'C# 只new()一次'
author: 'James Peng'
tags: ['Design Pattern']
---


~~~csharp
private static ConfigHelper _ch = null; 
public ConfigHelper ch { 
    get { 
        if (_ch == null) 
        { 
           _ch = new ConfigHelper(); 
        } 
        return _ch; 
    } 
} 
~~~
