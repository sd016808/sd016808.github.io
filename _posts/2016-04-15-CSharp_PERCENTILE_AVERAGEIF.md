---
layout: post
title: 'C# 計算前某百分比內的平均'
author: 'James Peng'
tags: ['C#']
---

如果你在 Excel 中取得一個成績的資料表，而想要計算某百分比內的平均分數，該如何處理？

![](..\images\2016-04-15-CSharp_PERCENTILE_AVERAGEIF\tVdnXPt.png)


![](..\images\2016-04-15-CSharp_PERCENTILE_AVERAGEIF\mKLklJM.png)

前10% VBA公式:

~~~text
=AVERAGEIF(B2:B11,">="&PERCENTILE(B2:B11,1-ROW(1:1)/10))
~~~


前20% VBA公式:

~~~text
=AVERAGEIF(B2:B11,">="&PERCENTILE(B2:B11,1-ROW(2:2)/10))
~~~

以此類推


----------


如果想在 C# 裡也 使用跟 Excel的 PERCENTILE 函數 及 AVERAGEIF 函數 


## C# 實做 PercenTile ##

~~~csharp
        public static Decimal CalcPercenTile(List<Decimal> data, Decimal percent)
        {
            if (percent < 0) throw new ArgumentException("percent must greater than 0");
            if (percent > 1) throw new ArgumentException("percent most less than 1");

            List<Decimal> _d = new List<decimal>(data);
            _d.Sort();
            Decimal n = percent * (_d.Count - 1) + 1;

            if (n == data.Count) return data[0];
            if (n == 1) return data[data.Count - 1];

            int v = (int)n;
            Decimal d = n - v;

            return _d[v - 1] + (_d[v] - _d[v - 1]) * d;
        }
~~~


----------

## C# 實做 Averageif ##

~~~csharp
        public static Decimal Averageif(List<Decimal> data, Decimal Threshold)
        {
            var cl1 = new List<Decimal>();
            foreach (Decimal cl in data)
            {

                if (cl >= Threshold)
                {
                    cl1.Add(cl);
                }                
            }

            Decimal result = 0;
            foreach (Decimal c in cl1)
            {
                result += c;
            }
            result = result / cl1.Count;

            return result;
        }
~~~


----------

## Usage: ##

~~~csharp
        private void button1_Click(object sender, EventArgs e)
        {
            
            List<Decimal> data = new List<decimal>() { 90,80,70,60,50,40,30,20,10,10 };

            MessageBox.Show(Averageif(data, (Decimal)CalcPercenTile(data, (Decimal)1)).ToString());
            MessageBox.Show(Averageif(data, (Decimal)CalcPercenTile(data, (Decimal)0.90)).ToString());
            MessageBox.Show(Averageif(data, (Decimal)CalcPercenTile(data, (Decimal)0.80)).ToString());
            MessageBox.Show(Averageif(data, (Decimal)CalcPercenTile(data, (Decimal)0.70)).ToString());
            MessageBox.Show(Averageif(data, (Decimal)CalcPercenTile(data, (Decimal)0.60)).ToString());
            MessageBox.Show(Averageif(data, (Decimal)CalcPercenTile(data, (Decimal)0.50)).ToString());
            MessageBox.Show(Averageif(data, (Decimal)CalcPercenTile(data, (Decimal)0.40)).ToString());
            MessageBox.Show(Averageif(data, (Decimal)CalcPercenTile(data, (Decimal)0.30)).ToString());
            MessageBox.Show(Averageif(data, (Decimal)CalcPercenTile(data, (Decimal)0.20)).ToString());
            MessageBox.Show(Averageif(data, (Decimal)CalcPercenTile(data, (Decimal)0.10)).ToString());
            MessageBox.Show(Averageif(data, (Decimal)CalcPercenTile(data, (Decimal)0)).ToString());

        }
~~~

![](..\images\2016-04-15-CSharp_PERCENTILE_AVERAGEIF\ID3Z5JK.png)

----------


## 參考： ##

- http://isvincent.pixnet.net/blog/post/32931655-excel-%E8%A8%88%E7%AE%97%E5%89%8D%E6%9F%90%E7%99%BE%E5%88%86%E6%AF%94%E5%85%A7%E7%9A%84%E5%B9%B3%E5%9D%87
- http://studio.wellwind.idv.tw/archives/208
- http://en.wikipedia.org/wiki/Percentile
- http://office.microsoft.com/zh-hk/excel-help/HP005209211.aspx
