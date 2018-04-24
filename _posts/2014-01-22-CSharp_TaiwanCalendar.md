---
layout: post
title: 'C# 取得目前日期週數'
author: 'James Peng'
tags: ['C#']
---

如果要取得現在的時間是今年的第幾週，以下提供一個簡單的方法。

- 

~~~csharp

        private string GetWeek(string datetime)
        {
            string week = new System.Globalization.TaiwanCalendar().GetWeekOfYear(Convert.ToDateTime(datetime), System.Globalization.CalendarWeekRule.FirstDay, System.DayOfWeek.Sunday).ToString();
            return week;
        }

~~~

參考：

- http://einboch.pixnet.net/blog/post/251144804
- [https://msdn.microsoft.com/zh-tw/library/system.globalization.taiwancalendar(v=vs.110).aspx](https://msdn.microsoft.com/zh-tw/library/system.globalization.taiwancalendar(v=vs.110).aspx)
