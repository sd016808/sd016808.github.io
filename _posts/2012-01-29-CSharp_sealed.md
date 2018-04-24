---
layout: post
title: 'C# sealed 禁止類別繼承 禁止方法覆寫'
author: 'James Peng'
tags: ['C#']
---

## 套用至類別時 ##

sealed 修飾詞會防止其他類別繼承該類別。

~~~csharp
class A {}    
sealed class B : A {}
~~~

## 套用至方法或屬性時 ##

sealed 修飾詞必須一律和 override 搭配使用。

~~~csharp
class X
{
    protected virtual void F() { Console.WriteLine("X.F"); }
    protected virtual void F2() { Console.WriteLine("X.F2"); }
}
class Y : X
{
    sealed protected override void F() { Console.WriteLine("Y.F"); }
    protected override void F2() { Console.WriteLine("Y.F2"); }
}
class Z : Y
{
    // Attempting to override F causes compiler error CS0239.
    // protected override void F() { Console.WriteLine("C.F"); }

    // Overriding F2 is allowed.
    protected override void F2() { Console.WriteLine("Z.F2"); }
}
~~~

----------

參考：

- https://dotblogs.com.tw/nobel12/2011/07/22/31963
- https://msdn.microsoft.com/zh-tw/library/88c54tsw.aspx
