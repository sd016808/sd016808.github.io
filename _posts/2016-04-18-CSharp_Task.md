---
layout: post
title: 'C# Task 簡單用法'
author: 'James Peng'
tags: ['C# 4.0']
---

# 問題：Button1按第2次時,無法清除textBox1的文字 #

問題來源 [msdn論壇](https://social.msdn.microsoft.com/Forums/zh-TW/7e2f9d7e-f6c1-433a-903c-032a2cead7b4/button12textbox1?forum=233)

各位大大們好,小弟跟大家請教一個問題

~~~csharp
private void button1_Click(object sender, EventArgs e)
{
textBox1.Text = " ";

Thread.Sleep(2000); //Delay 1秒

textBox1.Text = "測試文字~按第2次Button\r\n要先清除間隔2秒後才顯示唷";
} 
~~~

以上程式的目地,是按下button1時,會先清除textBox1的文字,經過2秒後才顯示出測試的文字

但發生了一個異常現像,當button1按下第2次到第n次時,都無法清除textBox1的文字

我有在其他論壇問過，有大大告知可用Application.DoEvents();

如果不使用Application.DoEvents();是否也可以辦到此功能呢？

請各位大大指導一下,謝謝~

程式是由VS2013撰寫,可以下方網址取得,謝謝~

https://drive.google.com/folderview?id=0B9ORcd2U4a0RUDRhNWtXQlFuOVk&usp=sharing



----------

# 解答 #

首先 Thread.Sleep 不應該出現在 UI 所在的執行緒, 如：button1_Click 內

因為在UI 執行緒中使用 Thread.Sleep 等於在封鎖操作畫面，這並不是一個好的使用者經驗設計方式.

所以, 建議是使用多執行緒處理能比較符合此應用的情況

而在 C#4.0 後出現了 System.Threading.Tasks命名空間的類型

Task 跟 ThreadPool 的功能類似，而且寫起來更為 簡單，直觀，簡潔，本篇就介紹Task的用法。

----------

您可以這樣做:

## 引用命名空間 ##

~~~csharp
using System.Threading;
using System.Threading.Tasks;
~~~

----------

## 建立 Task ##

我們在 button1_Click 裡，使用 Task 讓 要處理的事情另外開執行緒，這樣可以和 UI執行緒 分開。

以下範例：

~~~csharp
        private static CancellationTokenSource source = new CancellationTokenSource();        
        private static CancellationToken token = source.Token;
        private Task task = null;

        private void button1_Click(object sender, EventArgs e)
        {
            source = new CancellationTokenSource();
            token = source.Token;
            task = Task.Factory.StartNew(() =>
            {

                //裡面寫要處理的事

            }, token);
        }
~~~

----------


## Control.Invoke 方法 ##

這時候，直覺上是這樣寫：

~~~csharp
        private static CancellationTokenSource source = new CancellationTokenSource();        
        private static CancellationToken token = source.Token;
        private Task task = null;

        private void button1_Click(object sender, EventArgs e)
        {
            source = new CancellationTokenSource();
            token = source.Token;
            task = Task.Factory.StartNew(() =>
            {

                //裡面寫要處理的事
                textBox1.Text = " ";
                Thread.Sleep(2000); //Delay 1秒
                textBox1.Text = "測試文字~按第2次Button\r\n要先清除間隔2秒後才顯示唷";

            }, token);
        }
~~~

你會遇到 跨執行緒作業無效：

![](..\images\2016-04-18-CSharp_Task\qlXIOFJ.png)

你自已開的Task裡 和 UI執行緒 是分開的，所以你需要用 Control.Invoke 方法寫在 task裡 去操作 UI


~~~csharp
        private static CancellationTokenSource source = new CancellationTokenSource();        
        private static CancellationToken token = source.Token;
        private Task task = null;

        private void button1_Click(object sender, EventArgs e)
        {
            source = new CancellationTokenSource();
            token = source.Token;
            task = Task.Factory.StartNew(() =>
            {

                //裡面寫要處理的事
                textBox1.Invoke(new Action(() => textBox1.Clear()));
                System.Threading.Thread.Sleep(10000); //Delay 1秒
                textBox1.Invoke(new Action(() => textBox1.Text = "測試文字~按第2次Button , 要先清除間隔2秒後才顯示唷"));    
            }, token);
        }
~~~

就完成了！


----------


##  取消 Task ##

順帶一提 CancellationTokenSource 類別 可以用來對 Task 操作中途終止運行

寫法如下：

~~~csharp
source.Cancel();
~~~

----------

參考：

- https://msdn.microsoft.com/zh-tw/library/system.windows.forms.control.invoke(v=vs.110).aspx
- https://msdn.microsoft.com/zh-tw/library/dd321955(v=vs.110).aspx
- https://msdn.microsoft.com/zh-tw/library/system.threading.cancellationtokensource(v=vs.110).aspx
- https://msdn.microsoft.com/zh-tw/library/system.threading.tasks.task(v=vs.110).aspx
