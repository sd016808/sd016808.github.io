---
layout: post
title: 'Effective C#： (2) 建議使用 readonly 代替 const'
author: 'James Peng'
tags: ['Effective C#']
---


## Effective C# 條款二 item2 ##

第二條建議是

> 用 readonly 來替代 const


原因：

1. const，是編譯時(Compile-Time)常數，需將所有參考到的組件都給重新編譯，速度較快，但彈性不足。
2. readonly 是運行時(Runtime)常數，使用上彈性優於編譯時常數，若要修改常數的值，所有參考使用的組件都不需配合重新編譯。
3. const 只能宣告基本型別、列舉型別、與字串型別，readonly 宣告時沒有型別限制。

----------

## 範例 ##

**TestClass.cs**

~~~csharp
namespace EffectiveCSharp_Item2
{
    public class TestClass
    {
        public const int YearConst = 2016;
        public static readonly int YearReadonly = 2016;
    }
}
~~~

**Form1.cs**

~~~csharp
using System;
using System.Windows.Forms;

namespace EffectiveCSharp_Item2
{
    public partial class Form1 : Form
    {        
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            TestClass tc = new TestClass();

            if (DateTime.Now.Year == TestClass.YearConst)
            {
                Console.WriteLine("1");
            }

            if (DateTime.Now.Year == TestClass.YearReadonly)
            {
                Console.WriteLine("2");
            }
        }
    }
}

~~~

透過Reflector工具反組譯查看，

使用 const 編譯時常數的地方會被編譯器取代成 變數值。

![](..\images\2016-06-04-EffectiveCSharp_item2\LF3wx2b.png)


----------

## 常見情況 ##

當開發者修改const值時，若是沒有將所有參考到的組件都給重新編譯

會出現一個錯誤情況，

程式還能跑，但是其他參考的組件會使用修改前const的變數值...

因為其他參考到的地方是 編譯器是把有用到const的地方直接取代成 變數值，部分編譯就會有些地方沒修改成最新的 const值

就會出現問題...然後直到開發者將所有參考到的組件都給重新編譯

所以

**建議使用 readonly 代替 const**

----------

## 參考： ##

- http://www.clicktocontinue.com/books/EffectvCShrp.pdf
- https://dotblogs.com.tw/larrynung/2009/07/09/9263
