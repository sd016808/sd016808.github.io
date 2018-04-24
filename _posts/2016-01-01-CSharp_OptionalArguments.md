---
layout: post
title: 'C# 4.0 選擇性引數 Optional Arguments'
author: 'James Peng'
tags: ['C# 4.0']
---

 C# 4.0 開始支援了 Optional Parameters 功能

----------

## 選擇性參數（Optional Parameters） ##

選擇性參數也是 VB/VBA，以及 C/C++ 有的功能，只是 C# 一直都沒有加進來，新的 C# 4.0 開始加入了選擇性參數的功能，例如下列的函數：

宣告方法的引數,給個預設值就行了,例如:

~~~csharp
        private void button1_Click(object sender, EventArgs e)
        {
            MessageBox.Show(Add(10, 20));
            MessageBox.Show(Add(10, 20, 30));
        }
        static string Add(int x, int y, int z = 0)
        {
            return (x + y + z).ToString();
        }
~~~



![](..\images\2016-01-01-CSharp_OptionalArguments\ZFL6hy3.png)

![](..\images\2016-01-01-CSharp_OptionalArguments\SQdjusU.png)

----------


## 參考： ##

- http://vmiv.blogspot.tw/2009/06/c-4-optional-arguments.html
- https://msdn.microsoft.com/zh-tw/library/dd264739.aspx
- http://www.jiancool.com/article/55195414764/
