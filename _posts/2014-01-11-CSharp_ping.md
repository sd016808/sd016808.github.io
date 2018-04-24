---
layout: post
title: 'C# 透過 Ping IP或網址 檢查是否連線'
author: 'James Peng'
tags: ['C#']
---

## 引用 ##

~~~csharp
using System.Net.NetworkInformation;
~~~

## C# 透過 Ping IP或網址 檢查是否連線 ##

~~~csharp
        private static bool IsInLan()
        {
            string strLanUrl = "192.168.1.55";
            Ping ping = new Ping();
            PingReply reply;

            try
            {
                reply = ping.Send(strLanUrl);
                if (reply.Status == IPStatus.Success)  return true;
                else return false;
            }
            catch
            {
                return false;
            }
        }
~~~


