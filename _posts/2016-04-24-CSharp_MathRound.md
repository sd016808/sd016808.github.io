---
layout: post
title: 'C# 無條件進位 無條件捨去 四捨五入'
author: 'James Peng'
tags: ['C#']
---

## 無條件進位 ##

~~~csharp
        /// <summary>
        /// 無條件進位
        /// </summary>
        /// <param name="s"></param>
        /// <returns></returns>
        private int RoundedUp(double s)
        {            
            int result = 0;
            result = Convert.ToInt16(Math.Ceiling(s));
            return result;
        }
~~~


----------

## 無條件捨去 ##

~~~csharp
        /// <summary>
        /// 無條件捨去
        /// </summary>
        /// <param name="s"></param>
        /// <returns></returns>
        private int RoundedDown(double s) 
        {
            int result = 0;
            result = Convert.ToInt16(Math.Floor(s));
            return result;
        }
~~~


----------

## 四捨五入 ##

~~~csharp
        /// <summary>
        /// 四捨五入
        /// </summary>
        /// <param name="s">計算值</param>
        /// <param name="i">到小數點第幾位</param>
        /// <returns></returns>
        private double Rounding(double s, int i)
        {

            double result = 0;
            result = Math.Round(s, i, MidpointRounding.AwayFromZero); //若是要呈現一般認知的四捨五入需加入第三個參數-MidpointRounding.AwayFromZero
            return result;
        }
~~~

----------


## 用法 ##

~~~csharp
        private void button2_Click(object sender, EventArgs e)
        {
           double s = 3.14159;
           MessageBox.Show( RoundedUp( s).ToString());
           MessageBox.Show(RoundedDown(s).ToString());
           MessageBox.Show(Rounding(s,1).ToString());
        }
~~~

![](..\images\2016-04-24-CSharp_MathRound\8RxEpKs.png)

![](..\images\2016-04-24-CSharp_MathRound\pWpcGyR.png)

![](..\images\2016-04-24-CSharp_MathRound\0GNEnoR.png)

----------

參考：

- https://dotblogs.com.tw/jaigi/2011/05/11/24822
- http://msdn.microsoft.com/zh-tw/library/ef48waz8(v=VS.100).aspx
