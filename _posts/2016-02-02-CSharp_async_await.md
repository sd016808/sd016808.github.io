---
layout: post
title: 'C# 使用非同步的 async 和 await'
author: 'James Peng'
tags: ['C# 5.0']
---

async和await是.net 4.5才開始出現用來簡化非同步開發，讓使用者更加輕鬆的去處理，而且看起來和一般的Code沒啥差異，

早期想要做非同步開發，大都透過Thread或是ThreadPool方式達成，往往都需要寫一堆Code，

## Async ##

[async](http://msdn.microsoft.com/zh-tw/library/hh156513.aspx) 修飾詞指出方法中， lambda 運算式，或匿名方法它會修改為非同步作業。


----------


## Await ##


[Await](http://msdn.microsoft.com/zh-tw/library/hh156528.aspx) 運算子套用至非同步方法的一項工作的暫停方法的執行，直到這項等候的工作完成。 這項工作表示正在執行的工作。

使用的非同步方法必須是 非同步 關鍵字修改。 這種方法，定義使用 async 修飾詞和通常包含一或多個


## Task.Run ##

在計算需要大量計算時，想把這樣的Function放入到背景處理，還須加上Task.Run方式處理

https://msdn.microsoft.com/zh-tw/library/hh195051(v=vs.110).aspx


----------


## 更改前 ##

原本程式撰寫方式，如下，當等待 Sum() 計算時候，整個 Winform 將會被 hold 無法任意移動。 

同時，也無法再執行其他後續的計算。

為了進行測試，這邊迴圈最大值設定為int.MaxValue

範例如下Code：

~~~csharp
private  void button1_Click(object sender, EventArgs e)
{
    Stopwatch st = new Stopwatch();
    st.Start();           
    textBox1.Text = Sum();
    st.Stop();
    textBox2.Text =( st.ElapsedMilliseconds/1000).ToString();
}
private string Sum()
{
    int sum1 = 0;         
    for (int i = 0; i <int.MaxValue; i++)
    {
        sum1 += i + 1;          
    }
    return sum1.ToString();          
}
~~~


----------


## 更改後 ##

這樣當我們執行 Sum 等待計算回傳時候，還可以同時操作 UI



~~~csharp

private async void button1_Click(object sender, EventArgs e)
{
    Stopwatch st = new Stopwatch();
    st.Start();
    textBox1.Text = await Task.Run(() => { return Sum(); });
    st.Stop();
    textBox2.Text =( st.ElapsedMilliseconds/1000).ToString();
}


private async Task<string> Sum()
{
    int sum1 = 0;
    for (int i = 0; i < int.MaxValue; i++)
    {
        sum1 += i + 1;
    }            
    return sum1.ToString();            
}
~~~

----------

參考：

- http://msdn.microsoft.com/zh-tw/library/hh156513.aspx
- http://blog.sanc.idv.tw/2013/07/c-asyncawait.html
- https://msdn.microsoft.com/zh-cn/magazine/hh456403.aspx
- https://dotblogs.com.tw/jaigi/2012/10/14/77474
- https://msdn.microsoft.com/zh-tw/library/hh195051(v=vs.110).aspx
