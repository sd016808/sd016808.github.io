---
layout: post
title: 'C# 4.0 具名引數 Named Argument'
author: 'James Peng'
tags: ['C# 4.0']
---

 C# 4.0 開始支援了 Named Argument 功能

----------



## 具名引數（Named Argument）  ##

具名引數可以無視順序的給值！

有時候也可以在引數過多的時候當作一種註解吧～

具名引數顧名思義，就是直接使用參數的名稱來設定參數值，這個功能在 VB/VBA 中已經是司空見慣，並且被 C# 開發人員視為相當稱羨的功能之一，C# 4.0 編譯器以及 Visual Studio 2010 編輯介面開始支援具名引數，例如下列的函數：

~~~csharp

        private void button1_Click(object sender, EventArgs e)
        {
            MessageBox.Show(Add( y:20, x: 10));
            MessageBox.Show(Add(z: 30, x: 10, y: 20));
        }
        static string Add(int x, int y, int z = 0)
        {
            return ((x + y) * z).ToString();
        }

~~~

![](..\images\2016-01-02-CSharp_NamedArgument\ItotdEy.png)

![](..\images\2016-01-02-CSharp_NamedArgument\bQ5rfH0.png)

----------

## 參考： ##

- https://fredxxx123.wordpress.com/2014/01/16/c-%E5%85%B7%E5%90%8D%E5%BC%95%E6%95%B8named-arguments/
