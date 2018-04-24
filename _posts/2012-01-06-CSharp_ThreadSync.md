---
layout: post
title: 'C# 執行緒同步處理和互動'
author: 'James Peng'
tags: ['C#']
---

# 執行緒同步處理和互動 #

同步處理產生者和消費者執行緒

範例會建立兩個輔助或背景工作執行緒。其中一個執行緒會產生項目並將其存放在不是安全執行緒的泛型佇列中。

另一個執行緒會使用這個佇列中的項目。此外，主執行緒也會定期顯示佇列的內容，因此會有三個執行緒存取佇列。lock 關建字是用來同步處理對佇列的存取，以確保佇列的狀態不會損毀。

使用 lock 關鍵字除了單純避免同時進行存取以外，兩種事件物件也提供進階的同步處理功能。

其中一種物件是用來發出結束背景工作執行緒的信號，產生者執行緒則使用另一種物件在新物件加入佇列時，發出信號通知消費者執行緒。

這兩種事件物件會封裝在名為 SyncEvents 的類別中。這可以輕鬆地將事件傳遞至代表消費者和產生者執行緒的物件。

----------


## ThreadSync.cs ##



~~~csharp
// Copyright (C) Microsoft Corporation. All Rights Reserved.
// 此程式碼發行依據的條款為 
// Microsoft 公用授權 (MS-PL，http://opensource.org/licenses/ms-pl.html)。
//
// Copyright (C) Microsoft Corporation.  All rights reserved.

using System;
using System.Threading;
using System.Collections;
using System.Collections.Generic;

// 這個類別封裝了執行緒同步處理事件， 
// 這樣就能輕易地將這些事件傳遞給 Consumer 和 
// Producer 類別。 
public class SyncEvents
{
    public SyncEvents()
    {
        // 將 AutoResetEvent 當做 "new item" 事件使用，
        // 因為我們希望每當消費者執行緒回應此事件時，
        // 會自動重設這個事件。
        _newItemEvent = new AutoResetEvent(false);

        // 將 ManualResetEvent 當做 "exit" 事件使用，
        // 因為我們希望多個執行緒在這個事件發出信號時
        // 產生回應。如果使用 AutoResetEvent，
        // 事件物件會在單一執行緒回應之後， 
        // 還原成非信號狀態，而且其他執行緒
        // 將無法結束。
        _exitThreadEvent = new ManualResetEvent(false);

        // 同時將這兩個事件放置在 WaitHandle 陣列，如此一來 
        // consumer 執行緒就可以使用 WaitAny 方法封鎖這兩個
        // 事件。
        _eventArray = new WaitHandle[2];
        _eventArray[0] = _newItemEvent;
        _eventArray[1] = _exitThreadEvent;
    }

    // Public 屬性允許安全存取事件。
    public EventWaitHandle ExitThreadEvent
    {
        get { return _exitThreadEvent; }
    }
    public EventWaitHandle NewItemEvent
    {
        get { return _newItemEvent; }
    }
    public WaitHandle[] EventArray
    {
        get { return _eventArray; }
    }

    private EventWaitHandle _newItemEvent;
    private EventWaitHandle _exitThreadEvent;
    private WaitHandle[] _eventArray;
}

// Producer 類別會在佇列中具有 20 個項目之前，
// 非同步地 (使用背景工作執行緒) 在將項目加入至佇列中。
public class Producer 
{
    public Producer(Queue<int> q, SyncEvents e)
    {
        _queue = q;
        _syncEvents = e;
    }
    public void ThreadRun()
    {
        int count = 0;
        Random r = new Random();
        while (!_syncEvents.ExitThreadEvent.WaitOne(0, false))
        {
            lock (((ICollection)_queue).SyncRoot)
            {
                while (_queue.Count < 20)
                {
                    _queue.Enqueue(r.Next(0, 100));
                    _syncEvents.NewItemEvent.Set();
                    count++;
                }
            }
        }
        Console.WriteLine("Producer thread: produced {0} items", count);
    }
    private Queue<int> _queue;
    private SyncEvents _syncEvents;
}

// Consumer 類別會使用自己的工作背景執行緒在佇列中消費項目。
// Producer 類別使用 NewltemEvent 通知新項目的
// Consumer 類別。
public class Consumer
{
    public Consumer(Queue<int> q, SyncEvents e)
    {
        _queue = q;
        _syncEvents = e;
    }
    public void ThreadRun()
    {
        int count = 0;
        while (WaitHandle.WaitAny(_syncEvents.EventArray) != 1)
        {
            lock (((ICollection)_queue).SyncRoot)
            {
                int item = _queue.Dequeue();
            }
            count++;
        }
        Console.WriteLine("Consumer Thread: consumed {0} items", count);
    }
    private Queue<int> _queue;
    private SyncEvents _syncEvents;
}

public class ThreadSyncSample
{
    private static void ShowQueueContents(Queue<int> q)
    {
        // 列舉集合本來就不是安全執行緒，
        // 因此務必在列舉期間鎖定集合，
        // 以防止 consumer 執行緒和 producer 執行緒
        // 修改內容。(只有主要執行緒才能
        // 呼叫這個方法。)
        lock (((ICollection)q).SyncRoot)
        {
            foreach (int i in q)
            {
                Console.Write("{0} ", i);
            }
        }
        Console.WriteLine();
    }

    static void Main()
    {
        // 設定執行緒同步化需要之包含
        // 事件資訊的結構
        SyncEvents syncEvents = new SyncEvents();

        // 使用泛型 Queue 集合儲存要生產和消費的項目。 
        // 在此情況中使用 'int'。
        Queue<int> queue = new Queue<int>();

        // 建立物件，一個生產項目，另一個 
        // 則進行消費。將佇列和執行緒同步化事件傳遞
        // 到這兩個物件。
        Console.WriteLine("Configuring worker threads...");
        Producer producer = new Producer(queue, syncEvents);
        Consumer consumer = new Consumer(queue, syncEvents);

        // 建立生產者和消費者物件
        // 的執行緒物件。這個步驟
        // 不會建立或啟動真實的執行緒。
        Thread producerThread = new Thread(producer.ThreadRun);
        Thread consumerThread = new Thread(consumer.ThreadRun);

        // 建立和啟動兩個執行緒。     
        Console.WriteLine("Launching producer and consumer threads...");        
        producerThread.Start();
        consumerThread.Start();

        // 讓生產者和消費者執行緒執行 10 秒。
        // 使用主要執行緒 (執行這個方法的執行緒)
        // 每 2.5 秒就顯示佇列內容。
        for (int i = 0; i < 4; i++)
        {
            Thread.Sleep(2500);
            ShowQueueContents(queue);
        }

        // 發出結束信號給 consumer 和 producer 這兩個執行緒。
        // 這兩個執行緒皆會回應，因為 ExitThreadEvent 是 
        // 手動重設事件 -- 所以在明確重設之前會保持 'set'。
        Console.WriteLine("Signaling threads to terminate...");
        syncEvents.ExitThreadEvent.Set();

        // 使用 Join 封鎖主要執行緒，先等到生產者執行緒結束，
        // 然後再等到消費者執行緒結束。
        Console.WriteLine("main thread waiting for threads to finish...");
        producerThread.Join();
        consumerThread.Join();
    }
}

~~~


----------


## 執行結果 ##

~~~text
Configuring worker threads...
Launching producer and consumer threads...
25 90 75 52 58 91 2 8 37 30 12 23 91 69 15 87 96 50 40
6 97 35 16 32 68 78 83 53 24 20 23 25 57 50 18 44 61 61
97 9 99 7 70 63 68 96 55 10 55 35 27 71 82 43 80 85 2
40 54 48 79 91 42 25 14 84 18 75 50 82 7 35 66 40 20 82
Signaling threads to terminate...
main thread waiting for threads to finish...
Consumer Thread: consumed 296 items
Producer thread: produced 315 items
請按任意鍵繼續 . . .
~~~


----------

參考：

- https://code.msdn.microsoft.com/Threading-Sample-bc441f8e/file/46647/2/%E5%9F%B7%E8%A1%8C%E7%B7%92%E8%99%95%E7%90%86%E7%AF%84%E4%BE%8B.zip
- https://code.msdn.microsoft.com/Threading-Sample-bc441f8e
- https://msdn.microsoft.com/zh-tw/library/yy12yx1f(v=vs.80).aspx
