---
layout: post
title: 'C# 使用執行緒集區 ThreadPool'
author: 'James Peng'
tags: ['C#']
---

# 使用執行緒集區 #

下列範例使用 .NET Framework 執行緒集區來計算在 20 和 40 之間，十個數字的 Fibonacci 結果。


----------



## 參考 ##

~~~csharp
 using System.Threading;
~~~


----------


## Fibonacci 類別 ##

每個 Fibonacci 結果都由 Fibonacci 類別表示，此類別提供會執行計算且名為 ThreadPoolCallback 的方法。


~~~csharp
// Fibonacci 類別提供使用輔助執行緒的介面
// 來執行冗長的 Fibonacci(N) 計算。
// 提供 N 以及在作業完成時物件發出信號的事件，
// 給 Fibonacci 建構函式。
// 可以使用 FibOfN 屬性擷取結果。
public class Fibonacci
{
    public Fibonacci(int n, ManualResetEvent doneEvent)
    {
        _n = n;
        _doneEvent = doneEvent;
    }

    // 搭配執行緒集區使用的包裝函式方法。
    public void ThreadPoolCallback(Object threadContext)
    {
        int threadIndex = (int)threadContext;
        Console.WriteLine("thread {0} started...", threadIndex);
        _fibOfN = Calculate(_n);
        Console.WriteLine("thread {0} result calculated...", threadIndex);
        _doneEvent.Set();
    }

    // 會計算第 N 個 Fibonacci 數字的遞迴方法。
    public int Calculate(int n)
    {
        if (n <= 1)
        {
            return n;
        }
        else
        {
            return Calculate(n - 1) + Calculate(n - 2);
        }
    }

    public int N { get { return _n; } }
    private int _n;

    public int FibOfN { get { return _fibOfN; } }
    private int _fibOfN;

    ManualResetEvent _doneEvent;
}
~~~


----------


## ThreadPool Example ##

這時會建立表示每個 Fibonacci 值的物件，而且 ThreadPoolCallback 方法會傳遞到 QueueUserWorkItem，這個多載會指派集區中的可用執行緒來執行方法。

因為提供了半隨機值給每個 Fibonacci 物件計算，而且其中每個執行緒都會競爭處理器時間，所以您無法預先知道要花多少時間，才能計算完所有十個結果。

這就是在建構期間，ManualResetEvent 類別的執行個體 (Instance) 會傳遞給每個 Fibonacci 物件的原因。

每個物件完成計算時，都會傳送所提供事件物件的信號，這樣可讓主要執行緒使用 WaitAll 阻斷執行，直到十個 Fibonacci 物件都計算出結果為止。

然後 Main 方法會顯示每個 Fibonacci 結果。

~~~csharp
public class ThreadPoolExample
{
    static void Main()
    {
        const int FibonacciCalculations = 10;

        // 每個 Fibonacci 物件使用一個事件
        ManualResetEvent[] doneEvents = new ManualResetEvent[FibonacciCalculations];
        Fibonacci[] fibArray = new Fibonacci[FibonacciCalculations];
        Random r = new Random();

        // 使用 ThreadPool 設定並啟動執行緒:
        Console.WriteLine("launching {0} tasks...", FibonacciCalculations);
        for (int i = 0; i < FibonacciCalculations; i++)
        {
            doneEvents[i] = new ManualResetEvent(false);
            Fibonacci f = new Fibonacci(r.Next(20,40), doneEvents[i]);
            fibArray[i] = f;
            ThreadPool.QueueUserWorkItem(f.ThreadPoolCallback, i);
        }

        // 等待集區中所有的執行緒進行計算...
        WaitHandle.WaitAll(doneEvents);
        Console.WriteLine("Calculations complete.");

        // 顯示結果...
        for (int i= 0; i<FibonacciCalculations; i++)
        {
            Fibonacci f = fibArray[i];
            Console.WriteLine("Fibonacci({0}) = {1}", f.N, f.FibOfN);
        }
    }
}
~~~


----------


## 執行結果 ##

~~~text
launching 10 tasks...
thread 0 started...
thread 0 result calculated...
thread 1 started...
thread 1 result calculated...
thread 2 started...
thread 2 result calculated...
thread 3 started...
thread 3 result calculated...
thread 4 started...
thread 5 started...
thread 5 result calculated...
thread 6 started...
thread 7 started...
thread 7 result calculated...
thread 8 started...
thread 8 result calculated...
thread 9 started...
thread 9 result calculated...
thread 6 result calculated...
thread 4 result calculated...
Calculations complete.
Fibonacci(33) = 3524578
Fibonacci(28) = 317811
Fibonacci(34) = 5702887
Fibonacci(26) = 121393
Fibonacci(39) = 63245986
Fibonacci(34) = 5702887
Fibonacci(38) = 39088169
Fibonacci(29) = 514229
Fibonacci(24) = 46368
Fibonacci(32) = 2178309
請按任意鍵繼續 . . .
~~~


----------

參考：

- https://code.msdn.microsoft.com/Threading-Sample-bc441f8e/file/46647/2/%E5%9F%B7%E8%A1%8C%E7%B7%92%E8%99%95%E7%90%86%E7%AF%84%E4%BE%8B.zip
- https://code.msdn.microsoft.com/Threading-Sample-bc441f8e
- https://msdn.microsoft.com/zh-tw/library/system.threading.threadpool(v=vs.110).aspx
