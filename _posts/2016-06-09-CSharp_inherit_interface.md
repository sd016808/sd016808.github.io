---
layout: post
title: 'C# 觀念釐清-區分關於 abstract、virtual、override 和 new'
author: 'James Peng'
tags: ['C# 觀念釐清']
---


abstract、virtual、override和new是在類別的繼承關係中常用的四個修飾方法的關鍵字，在此略作總結。

1. 常用的中文名稱：

- abstract => 抽象方法。
- virtual => 虛擬方法。
- override => 覆蓋基礎類別方法。
- new => 隱藏基礎類別方法。
- override 和 new 有時都叫覆寫基礎類別方法。

2. 適用場合：

- abstract 和 virtual 用在基礎類別(父類別)中
- override 和 new 用在派(衍)生類別（子類別）中。

3. 具體概念：

- abstract 抽象方法，是空的方法，沒有方法實體，派(衍)生類必須以 override 實現此方法。
- virtual 虛擬方法，若希望或預料到基礎類別的這個方法在將來的派(衍)生類別中會被覆寫（override 或 new），則此方法必須被聲明為 virtual。
- override 覆寫繼承自基礎類別的virtual方法，可以理解為拆掉老房子，在原址上建新房子，老房子再也找不到了（基礎類別方法永遠調用不到了）。
- new 隱藏繼承自基礎類別的virtual方法，老房子還留着，在旁邊蓋個新房子，想住新房子的住新房子（作為衍生類別對象調用），想住老房子住老房子（作為基礎類別對象調用）。
- 當派(衍)生類別中出現與基礎類別同名的方法，而此方法前面未加 override 或 new 修飾符時，編譯器會報警告，但不報錯，真正執行時等同於加了new。

4. abstract 和 virtual 的區別：

- abstract 方法還沒實現，連累着基礎類別也不能被實例化，除了作為一種規則或符號外沒啥用；virtual 則比較好，派(衍)生類別想覆寫就覆寫，不想覆寫就吃老子的。
- 而且繼承再好也是少用為妙，繼承層次越少越好，派(衍)生類別新擴展的功能越少越好，virtual 深合此意。

5. override 和 new 的區別：

- 當派(衍)生類別對象作為基類類型使用時，override 的執行派(衍)生類別方法，new 的執行基礎類別方法。
- 如果作為派(衍)生類別類型調用，則都是執行 override 或 new 之後的。


## 觀念釐清 ##

~~~csharp
using System;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            int iPrice = 1000;

            //Question 1: result1 = ?
            var account1 = new SuperGoldAccount(iPrice);
            decimal result1 = account1.GetPrice();
            Console.WriteLine(result1.ToString());

            //Question 2: result = ?
            GeneralAccount account2 = new GoldAccount(iPrice);
            decimal result2 = account2.GetPrice();
            Console.WriteLine(result2.ToString());

            //Question 3: result=?
            GeneralAccount account3 = new SuperGoldAccount(iPrice);
            decimal result3 = account3.GetPrice();
            Console.WriteLine(result3.ToString());

            //Question 4: result=?
            GeneralAccount account4 = new SuperGoldAccount(iPrice);
            decimal result4 = account4.GetId();
            Console.WriteLine(result4.ToString());

            //Question 5: result=?
            GoldAccount account5 = new SuperGoldAccount(iPrice);
            decimal result5 = account5.GetId();
            Console.WriteLine(result5.ToString());

            Console.ReadLine();
        }
    }

    abstract class Account
    {
        private decimal _Price = 0;
        protected decimal Price
        {
            get { return _Price; }
            set { _Price = value; }
        }
        public abstract decimal GetPrice();
    }
    class GeneralAccount : Account
    {
        public GeneralAccount(decimal price)
        {
            Price = price;
        }

        public override decimal GetPrice()
        {
            return Price;
        }

        public virtual int GetId()
        {
            return 0;
        }
    }

    class GoldAccount : GeneralAccount
    {
        public GoldAccount(decimal price) : base(price)
        {
        }

        public override decimal GetPrice()
        {
            return base.GetPrice() * 0.9m;
        }

        public virtual new int GetId()
        {
            return 1;
        }
    }

    class SuperGoldAccount : GoldAccount
    {
        public SuperGoldAccount(decimal price) : base(price)
        {
        }

        public new decimal GetPrice()
        {
            return base.GetPrice() * 0.8m;
        }

        public override int GetId()
        {
            return 2;
        }
    }
}

~~~
 
 
請問 會印出什麼字串？
 
Question1=?

Question2=?

Question3=?

Question4=?

Question5=?
 
 
 

----------

## 參考： ##

- http://jimmy0222.pixnet.net/blog/post/37271702-%5Bc%23%5D-%E5%8D%80%E5%88%86-abstract%E3%80%81virtual%E3%80%81override-%E5%92%8C-new
 
 
 
 
 
 
 
 
 
 
 
  
 
 
 
 
 
 
  
 
 
 
 
 
  
 
 
 
 
 
 
  
 
 
 
 
 
 
  
 
 
 
 
 
 
  
 
 
 
 
 
 
 
 
下一頁解答
 
 
 
 
 
 
 
 
----------



## 解答： ##



----------

## 觀念釐清： ##

待補