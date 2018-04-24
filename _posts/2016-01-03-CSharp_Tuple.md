---
layout: post
title: 'C# 4.0 函數中一次返回多個回傳值 Tuple'
author: 'James Peng'
tags: ['C# 4.0']
---

 C# 4.0 開始支援了 Tuple 功能

在開發時，總會碰到一種需求：我希望回傳不只一個值。

一般的 function 只能回傳一個值，如果要多個值，通常有幾種方式：

- 自訂一個 class 或 struct ，來代表這幾個值是內聚在一起描述某種相同的職責所需的特徵。
- 透過物件身上的 filed/member 來存放經過這個方法之後，物件狀態的轉變，需要時再取用物件狀態即可。
- 透過 out 或 byref 的宣告，把其他要回傳的多個值，在呼叫時先傳進來。

----------

## 自訂一個 class 或 struct  ##

- AfterYear() 會讓 age 和 YearsOfWork 兩個變數都+1，
- 透過自訂一個 class person 把這幾個值包在一起回傳，就可以達到 一個 function 回傳一個物件，物件裡包含多個值

~~~csharp
using System;
using System.Windows.Forms;

namespace WindowsFormsApplication3
{
    public class person
    {
        public string Name { get; set; }
        public int Age { get; set; }        
        public int YearsOfWork  { get; set; }
    }
    public partial class Form1 : Form
    {
        person person = new person { Age = 20, Name = "james", YearsOfWork = 0 };

        public Form1()
        {
            InitializeComponent();            
        }

        private void button1_Click(object sender, EventArgs e)
        {            
            
            person = AfterYear(person);

            MessageBox.Show("HI~" + person.Name + ", Age=" + person.Age + " , Years of work=" + person.YearsOfWork);
        }

        private person AfterYear(person p)
        {
               p.Age +=1;
               p.YearsOfWork +=1;
               return p;
        }
    }

}

~~~

![](..\images\2016-01-03-CSharp_Tuple\2tESN3Y.png)

會碰到幾個問題：

- 回傳的多個值，有時候並不帶有強烈內聚的關係。被放在 class 或是 struct 並不妥當，只是單純為了放在一起而新增了一個容器。
- 這些 class 或 struct 有可能只被這個方法使用，為了這樣的需求而新增一個無法重用且無法明確表達同一職責的 class 或 struct, 容易撐爆跟污染你的產品。
- 
----------

## 透過物件身上的 filed/member 來存放經過這個方法之後  ##

~~~csharp
using System;
using System.Windows.Forms;

namespace WindowsFormsApplication3
{
    public class person
    {
        public string Name { get; set; }
        public int Age { get; set; }        
        public int YearsOfWork  { get; set; }
    }
    public partial class Form1 : Form
    {
        person person = new person { Age = 20, Name = "james", YearsOfWork = 0 };

        int BlogNextPage = 1;
        string url = string.Empty;

        public Form1()
        {
            InitializeComponent();            
        }

        private void button1_Click(object sender, EventArgs e)
        {            
            
            person = AfterYear(person);

            MessageBox.Show("HI~" + person.Name + ", Age=" + person.Age + " , Years of work=" + person.YearsOfWork + "\n NextPage = " + url);
        }

        private person AfterYear(person p)
        {
               p.Age +=1;
               p.YearsOfWork +=1;
               BlogNextPage += 1;
               url = "http://jia-hong-peng.github.io/page" + BlogNextPage;
               return p;
        }
    }

}

~~~

但仍有可能碰到：

- 當方法只是單純的 static 方法時，方法內只能取用 static 的 filed/member/property ，可能碰到 thread-safe 問題，同一時間點多個 thread 同時取用，會被互相影響。
- 期望方法回傳值的生命週期僅限於這個方法內，而非整個物件內都可以存取，以避免互相影響或被其他方法改變。


----------

## 透過 out 或 ref   ##

~~~csharp
using System;
using System.Windows.Forms;

namespace WindowsFormsApplication3
{
    public class person
    {
        public string Name { get; set; }
        public int Age { get; set; }        
        public int YearsOfWork  { get; set; }
    }
    public partial class Form1 : Form
    {
        person person = new person { Age = 20, Name = "james", YearsOfWork = 0 };
        int BlogNextPage = 1;
        
        string url = string.Empty;

        public Form1()
        {
            InitializeComponent();            
        }

        private void button1_Click(object sender, EventArgs e)
        {

            person = AfterYear(person, ref BlogNextPage);

            url = "http://jia-hong-peng.github.io/page" + BlogNextPage;
            MessageBox.Show("HI~" + person.Name + ", Age=" + person.Age + " , Years of work=" + person.YearsOfWork + "\n NextPage = " + url);
        }

        private static person AfterYear(person p, ref int nextpage)
        {                       
               p.Age +=1;
               p.YearsOfWork +=1;
               nextpage += 1;               
               return p;
        }
    }

}

~~~

問題：

- out 跟 byref 的用法很囉嗦，例如要先在方法外面宣告，或是在方法裡面一定要重新 assign 。
- 當需要多個值時，方法參數個數會因為回傳值而越來越多。一堆 out 或 byref 的參數，顯得相當礙眼。

----------

## 透過 Tuple  ##

~~~csharp
using System;
using System.Windows.Forms;

namespace WindowsFormsApplication3
{
    public class person
    {
        public string Name { get; set; }
        public int Age { get; set; }        
        public int YearsOfWork  { get; set; }
    }
    public partial class Form1 : Form
    {
        static int offset = 0;
        
       

        public Form1()
        {
            InitializeComponent();            
        }

        private void button1_Click(object sender, EventArgs e)
        {
            person person = new person { Age = 20, Name = "james", YearsOfWork = 0 };
            int BlogNextPage = 1;

            var result = AfterYear(person, BlogNextPage);
            
            MessageBox.Show("HI~" + result.Item1.Name + ", Age=" + result.Item1.Age + " , Years of work=" + result.Item1.YearsOfWork + "\n NextPage = " + "http://jia-hong-peng.github.io/page" + result.Item2);
        }

        private static Tuple<person,int> AfterYear(person p, int nextpage)
        {
            offset++;

            p.Age += offset;
            p.YearsOfWork += offset;
            nextpage += offset;
                           
            return Tuple.Create(p,nextpage);
        }
    }

}

~~~

----------

## 參考： ##

- https://msdn.microsoft.com/zh-tw/library/system.tuple(v=vs.110).aspx
- https://dotblogs.com.tw/hatelove/2013/12/12/tuple-introduction
- http://sweeteason.pixnet.net/blog/post/42659804-%5Bc%23%5D-%E4%BD%BF%E7%94%A8tuple-%E9%A1%9E%E5%88%A5%EF%BC%8C%E5%9C%A8%E5%87%BD%E6%95%B8%E4%B8%AD%E4%B8%80%E6%AC%A1%E8%BF%94%E5%9B%9E%E5%A4%9A%E5%80%8B%E5%9B%9E
