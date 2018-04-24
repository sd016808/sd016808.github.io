---
layout: post
title: 'C# 觀念釐清- ref 區域變數 Scope'
author: 'James Peng'
tags: ['C# 觀念釐清']
---

C# 釐清 ref 區域變數 Scope

生存空間(Scope)：變數可被存取的程式碼區塊



## 第一題 ##

~~~csharp
using System;
using System.Windows.Forms;

namespace WindowsFormsApplication1
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            A a = new A { str = "555" };
            foo1(ref a);
            string anser1 = a.str;
            foo2(a);
            string anser2 = a.str;
            MessageBox.Show(anser1+"、"+anser2);
        }

        private void foo1(ref A a)
        {
            a.str = "666";
        }

        private void foo2(A a)
        {
            a.str = "777";
        }
    }

    public class A
    {
        public string str { get; set; }
    }
}
~~~



請問 按下 button1 時 a.str 分別是多少？


----------


## 第二題 ##

~~~csharp
using System;
using System.Windows.Forms;

namespace WindowsFormsApplication1
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            A a = new A { str = "555" };
            foo1(ref a);
            string anser1 = a.str;
            foo2(a);
            string anser2 = a.str;
            MessageBox.Show(anser1+"、"+anser2);
        }

        private void foo1(ref A a)
        {
            a = new A();
            a.str = "666";
        }

        private void foo2(A a)
        {
            a = new A();
            a.str = "777";
        }
    }

    public class A
    {
        public string str { get; set; }
    }
}

~~~



請問 按下 button1 時 a.str 分別是多少？
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
  
 
 
 
 
 
 
  
 
 
 
 
 
  
 
 
 
 
 
 
  
 
 
 
 
 
 
  
 
 
 
 
 
 
  
 
 
 
 
 
 
 
 
下一頁解答
 
 
 
 
 
 
 
 
----------



## 解答： ##

> 第一題： 666、777
> 第二題： 666、666

----------

## 觀念釐清： ##

待補