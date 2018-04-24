---
layout: post
title: 'Effective C#： (3) 建議使用 is 或 as 來取代強制轉型'
author: 'James Peng'
tags: ['Effective C#']
---


## Effective C# 條款三 item3 ##

第3條建議是

> 建議使用 is 或 as 來取代強制轉型


原因：

1. 因為它比強制轉型安全，也具有較好的效能。


----------


# 使用 強制轉型 #


**TestClass.cs**

~~~csharp
namespace EffectiveCSharp_Item3
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

namespace EffectiveCSharp_Item3
{
    public partial class Form1 : Form
    {        
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            object obj = new TestClass();
            TestClass tc = (TestClass)obj;        
        }
    }
}

~~~

如果無法轉型 會產生例外處理

![](..\images\2016-06-05-EffectiveCSharp_item3\FAiTkI1.png)

所以，還要用 Try Catch 例外處理包起來

除此之外，也需外加null的判斷 (因為null可轉為任意型態)


**Form1.cs**

~~~csharp
using System;
using System.Windows.Forms;

namespace EffectiveCSharp_Item3
{
    public partial class Form1 : Form
    {        
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            try
            {
                object obj = new TestClass();
                TestClass tc = (TestClass)obj;  
                if (tc != null)
                {
                    Console.WriteLine("轉型成功");
                }
                else
                {
                    //null
                }
            }
            catch
            {
                Console.WriteLine("轉型失敗");
            }      
        }
    }
}

~~~



# 使用 as 轉型 #

使用 as 運算子來做轉型，不需添加例外處理

## as 轉型成功 ##

**Form1.cs**

~~~csharp
using System;
using System.Windows.Forms;

namespace EffectiveCSharp_Item3
{
    public partial class Form1 : Form
    {        
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            object obj = new TestClass();
            TestClass tc = obj as TestClass;

            if (tc != null)
            {
                Console.WriteLine("轉型成功");
            }
            else
            {
                Console.WriteLine("轉型失敗");
            }
        }
    }
}

~~~

![](..\images\2016-06-05-EffectiveCSharp_item3\v86yXhH.png)



----------

## as 轉型失敗 ##

**Form1.cs**

~~~csharp
using System;
using System.Windows.Forms;

namespace EffectiveCSharp_Item3
{
    public partial class Form1 : Form
    {        
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            object obj = new TestClass();
            Form1 tc = obj as Form1;

            if (tc != null)
            {
                Console.WriteLine("轉型成功");
            }
            else
            {
                Console.WriteLine("轉型失敗");
            }
        }
    }
}

~~~

![](..\images\2016-06-05-EffectiveCSharp_item3\Fp7aJ6i.png)

----------

## 使用 is 運算子的時機 ##

as 運算子只能用於參考類型，不能應用於值類型(除非是Nullable的值類型)

因為值類型不可為 null 導致

~~~csharp
            object obj = new TestClass();
            int tc = obj as int; // 無法編譯.
~~~

![](..\images\2016-06-05-EffectiveCSharp_item3\TSXllvl.png)


像這樣的值類型轉換，我們就可以使用 is 運算子先做判斷避免異常

~~~csharp
            object obj = new TestClass();
            if (obj is int)
            {
                int tc = (int)obj;
            }
~~~

一般來說只有當不能使用as運算子作轉型動作時，才會考慮使用is運算子

is 運算子用來檢查物件是否與指定的型別相容，本身不能用來轉型。

----------

## 參考： ##

- http://www.clicktocontinue.com/books/EffectvCShrp.pdf
- https://msdn.microsoft.com/zh-tw/library/scekt9xw.aspx
- https://dotblogs.com.tw/larrynung/2009/09/30/10834
