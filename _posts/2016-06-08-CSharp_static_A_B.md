---
layout: post
title: 'C# 觀念釐清-this vs base 和 static vs non-static'
author: 'James Peng'
tags: ['C# 觀念釐清']
---

C# 釐清 this vs base 和 static vs non-static


## this ##

子類別繼承父類別後，當存取同名的成員時，可以使用base和this來區分是存取父類別或子類別的成員

this：代表目前類別，可以用來識別類別成員或區域變數

~~~csharp
class 類別
{
    string 資料 = "World";
    public void 方法(string 資料)               //假設 資料="Hello"
    {
        System.Console.WriteLine( 資料 );       //顯示：Hello
        System.Console.WriteLine( this.資料 );  //顯示：World
    }
}
~~~

## base ##

代表父類別，可以用來識別父類別成員或子類別成員

~~~csharp
class 父類別
{
    virtual void 方法( ) { 
        System.Console.WriteLine("父類別方法"); 
    }
}

class 子類別 : 父類別
{
    public override void 方法( ){ 
        System.Console.WriteLine("子類別方法"); 
    }

    public void　方法A( )
    {
        base.方法( );   //顯示：父類別方法
        方法( );        //顯示：子類別方法
        this.方法( );   //顯示：子類別方法
    }
}
~~~

代表父類別的建構函式

~~~csharp
class 父類別
{
    public 父類別() {
        System.Console.Write(" 父類別");
    }
}

class 子類別 : 父類別
{
    public 子類別() : base()
    {
        System.Console.Write("子類別");     //顯示：父類別 子類別
    }
}
~~~

代表父類別的建構函式，並帶參數

~~~csharp
class 父類別
{
    public 父類別() { System.Console.Write(" 父類別A"); }
    public 父類別(string 參數) { System.Console.Write(" 父類別B"); }
}

class 子類別 : 父類別
{
    public 子類別(string 參數) : base(參數)
    {
        System.Console.Write("子類別");     //顯示：子類別 父類別B
    }
}
~~~


## 觀念釐清 ##

~~~csharp
using System;
using System.Windows.Forms;

namespace WindowsFormsApplication1
{
    public partial class Form1 : Form
    {
        public A a = new A { str = "0" };
        private A b = new A { str = "1" };
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            A a = new A { str = "2" };
            A b = new A { str = "3" };

            aManager.a = this.a;
            this.a.str = "4";
            aManager.a.str = "5";
            a = new A();

            aManager.b = b;
            b = a;
            a.str = "6";

            //Question1 : this.a.str = ?
            MessageBox.Show("Question1 :" + this.a.str);

            //Question2 : aManager.b.str = ?
            MessageBox.Show("Question2 :" + aManager.b.str);

            //Question3 : a.str = ?
            a.str = "6";
            foo1(ref a);
            MessageBox.Show("Question3 :" + a.str);

            //Question4 : a.str = ?
            a.str = "6";
            foo2(a);
            MessageBox.Show("Question4 :" + a.str);
        }

        private void foo1(ref A a)
        {
            a = new A();
            object number1 = 1, number2 = 1;

            if (number1.Equals(number2))
            {
                a.str = "7";
            }
            else if (number1.Equals((int)0x001))
            {
                a.str = "8";
                return;
            }
            if (number1 == number2)
            {
                a.str = null;
                if (string.IsNullOrEmpty(a.str))
                    return;
                a.str = "9";
            }
            else
            {
                a.str = "10";
            }
        }

        private void foo2(A a)
        {
            a = new A();
            a.str = "11";
        }
    }

    public class A
    {
        public string str { get; set; }
        public A()
        {
            str = "12";
        }
    }

    public class aManager
    {
        public static A a = new A { str = "13" };
        public static A b = new A { str = "14" };
    }
}
~~~
 
 
請問 按下 button1 時 MessageBox 會顯示什麼字串？
 
Question1=?
Question2=?
Question3=?
Question4=?

 
 
 
 
 
 
 
 
 
 
 
 
 
 
  
 
 
 
 
 
 
  
 
 
 
 
 
  
 
 
 
 
 
 
  
 
 
 
 
 
 
  
 
 
 
 
 
 
  
 
 
 
 
 
 
 
 
下一頁解答
 
 
 
 
 
 
 
 
----------



## 解答： ##



----------

## 觀念釐清： ##

待補



----------


## 參考 ##

- http://notepad.yehyeh.net/Content/CSharp/CH01/03ObjectOrient/5thisAndBase/index.php