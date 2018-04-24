---
layout: post
title: 'C# 撰寫 Unsafe Code'
author: 'James Peng'
tags: ['C#']
---

許多人從C++轉移到C#時最關心的問題就是：當他們有需要時，是否有完整的控制能力能複製記憶體的內容，這也就是unsafe code要解決的問題。雖然unsafe code這名字聽起來很可怕，不過它並不是真的不安全、或是不可靠的程式碼。它指的是.NET執行時期不會自動幫它配置與釋放記憶體的程式碼。當您想透過指標與舊有系統溝通（例如，C API）、或您的程式需要直在記憶體上做複製的動作時，支援unsafe code的能力就變得非常重要了。

您撰寫unsafe code時會用到兩個關鍵字：unsafe與fixed。關鍵字unsafe會指明某個區塊的程式碼會在unmanaged環境執行。這關鍵字可以用在所有的method，包括建構子與屬性、甚至method中的某一個區塊。關鍵字fixed是用來釘住某個managed物件。這個動作主要是來通知garbage collection（GC）不可以搬移它。程式執行時，物件配置再回收後，在記憶體中就會產生一個“空白區塊”。為了避免記憶體不連續，.NET執行時期會將物件搬來搬去，讓記憶體的使用更有效率。當然，如果這時候您用了一個指標指向記憶體某個位置，而.NET在沒通知您的情況下把物件搬走，那您的指標就沒用了。由於GC在記憶體中搬移物件是為了增進程式的效率，所以您使用這個關鍵字時最好要謹慎一點。


----------


## 在C#中使用指標 ##

讓我們先來看看在C#中使用指標與unsafe code時要注意的規則，之後再看一些範例。

指標只能用在value型別、陣列、字串。此外要注意的是，陣列的第一個元素也必須是value型別，因為C#傳回的是陣列的第一個元素的指標，而不是陣列本身的指標。

因此，從編譯器的角度來看，它還是傳回一個value型別的指標，而非reference型別的。


----------



C/C++ 指標運算子

<table border="1"><tbody><tr><th><font size="2">運算子<span id="Layer51"></span></font></th><th><font size="2">敘述<span id="Layer52"></span></font></th></tr><tr><td><font size="2">&amp;<span id="Layer53"></span></font></td><td><font size="2">address-of 運算子用來傳回變數的記憶體位置<span id="Layer54"></span></font></td></tr><tr><td><font size="2">*<span id="Layer55"></span></font></td><td><font size="2">dereference 運算子用來取得指標所指記憶體位置內的值<span id="Layer56"></span></font></td></tr><tr><td><font size="2">-&gt;<span id="Layer57"></span></font></td><td><font size="2">dereference與member access 運算子用來存取成員以及指標的dereference<span id="Layer58"></span></font></td></tr></tbody></table>


----------



下面的程式碼對C/C++的程式設計師來說應該很熟悉。我呼叫一個method，帶了兩個指向int的指標做參數，並在回到呼叫端之前修改指標所指的值。這程式沒什麼，不過卻說明了如何在C#中使用指標。


~~~csharp
//	使用 /unsafe　參數來編譯此程式
using System;
class Unsafe1App
{
	public static unsafe void GetValues(int*x,int*y)
	{
		*x =6;
		*y =42;
	}
	public static unsafe void Main()
	{
		int a =1;
		int b =2;
		Console.WriteLine("Before GetValues():a ={0},b ={1}",
			a,b);
		GetValues(&a,&b);
		Console.WriteLine("After GetValues():a ={0},b ={1}",
			a,b);
	}
}
~~~

此程式編譯時必須使用/unsafe參數。執行的結果應該如下：



~~~csharp
Before GetValues():a =1,b =2
After GetValues():a =6,b =42
~~~



----------

參考：

- 書名：完全剖析C#, 出版日期：2001/10/30
