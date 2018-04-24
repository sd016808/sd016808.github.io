---
layout: post
title: 'C# 設計模式 - Singleton Pattern 單例模式'
author: 'James Peng'
tags: ['Design Pattern']
---

![](..\images\2016-04-04-CSharp_Singleton\JmNihNT.png)

Singleton Pattern在設計上是希望確保一個類別在系統運行的時候，只有一個實體(instance)產生，同時，這個類別必須要提供一個公開的方法讓其他人取得自己的實體。



## 用途 ##

在某些情況下，你的程式只會希望有一個物件的實體存在，比如說，連接資料庫的物件、程式管理員的物件、處理印表機列印的類別等等。

如果這些物件可以有多個實體，那每次需要這些功能的時候，我們都可以任意的new一個實體出來進行操作，這樣會造成相當恐怖的結果。

- 保證一個類別 僅有一個 實體(instance)
- 提供一個 公開的靜態方法。


----------


## 概念程式碼 ##

~~~csharp
// Singleton pattern -- Structural example
using System;
namespace DoFactory.GangOfFour.Singleton.Structural
{ // MainApp test application 
    class MainApp
    {
        static void Main()
        {

            // Constructor is protected -- cannot use new 
            Singleton s1 = Singleton.Instance();
            Singleton s2 = Singleton.Instance();
            if (s1 == s2)
            {
                Console.WriteLine("Objects are the same instance");
            }
            // Wait for user 
            Console.Read();
        }

    }

    // "Singleton"

    class Singleton
    {
        private static Singleton instance; // Note: Constructor is 'protected' 
        protected Singleton()
        {
        }

        public static Singleton Instance()
        {
            // Use 'Lazy initialization'
            if (instance == null)
            {
                instance = new Singleton();
            }
            return instance;
        }
    }
}
~~~


----------


## 實際範例程式碼 ##


~~~csharp
// Singleton pattern -- Real World example

using System;
using System.Collections;
using System.Threading;

namespace DoFactory.GangOfFour.Singleton.RealWorld
{
    // MainApp test application 
    class MainApp
    {
        static void Main()
        {
            LoadBalancer b1 = LoadBalancer.GetLoadBalancer();
            LoadBalancer b2 = LoadBalancer.GetLoadBalancer();
            LoadBalancer b3 = LoadBalancer.GetLoadBalancer();
            LoadBalancer b4 = LoadBalancer.GetLoadBalancer();

            // Same instance?
            if (b1 == b2 &&
                b2 == b3 &&
                b3 == b4)
            {
                Console.WriteLine("Same instance\n");
            }
            // All are the same instance -- use b1 arbitrarily
            // Load balance 15 server requests
            for (int i = 0; i < 15; i++)
            {
                Console.WriteLine(b1.Server);
            }
            // Wait for user
            Console.Read();
        }
    }

    // "Singleton"
    class LoadBalancer
    {
        private static LoadBalancer instance;
        private ArrayList servers = new ArrayList();
        private Random random = new Random();

        // Lock synchronization object
        private static object syncLock = new object();

        // Constructor (protected)
        protected LoadBalancer()
        {
            // List of available servers 
            servers.Add("ServerI");
            servers.Add("ServerII");
            servers.Add("ServerIII");
            servers.Add("ServerIV");
            servers.Add("ServerV");
        }

        public static LoadBalancer GetLoadBalancer()
        {
            // Support multithreaded applications through 
            // 'Double checked locking' pattern which (once 
            // the instance exists) avoids locking each 
            // time the method is invoked 
            if (instance == null)
            {
                lock (syncLock)
                {
                    if (instance == null)
                    {
                        instance = new LoadBalancer();
                    }
                }
            }
            return instance;
        }

        // Simple, but effective random load balancer 
        public string Server
        {
            get
            {
                int r = random.Next(servers.Count);
                return servers[r].ToString();
            }
        }
    }
}
~~~

----------

參考：

- http://kevingo75.blogspot.tw/2010/12/singleton-pattern-part1.html

