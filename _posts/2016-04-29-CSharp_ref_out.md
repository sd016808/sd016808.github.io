---
layout: post
title: 'C# Method參數的種類 ref 與 out'
author: 'James Peng'
tags: ['C#']
---

當我們試圖在C#中用method去獲得資訊時，您得到的只是一個回傳值。因此乍看之下，似乎每呼叫一個method只能得到一個值。

當然，若每次要獲得一項資料就得呼叫一個method，這實在太麻煩了。

舉例來說，您有一個Color的類別，用標準RGB（red-green-blue）的方式以三個值來描述顏色。如果您只利用回傳值來寫，程式碼應該如下：


~~~csharp
//假設color是Color類別的instance
int red =color.GetRed();
int green =color.GetGreen();
int blue =color.GetBlue();
~~~

可是我們比較希望寫成這樣：

~~~csharp
int red;
int green;
int blue;
color.GetRGB(red,green,blue);
~~~

不過這樣寫有個問題，當我們呼叫color.GetRGB這method時，red、green、blue的值只會被複製到這method的區域堆疊，不管值最後怎麼變都不會影響到呼叫端的變數。

在C++中的解決方法是，我們使用指標或是reference的方式來傳遞變數，因此任何的變動都是針對呼叫端的變數在做。

在C#中的解決方法也差不多，它提供兩種相近的辦法。第一個是使用關鍵字ref，這關鍵字會通知C#編譯器，傳過去的參數會指向呼叫端變數的位置。

因此，只要被呼叫的method做了任何變動都會修改到呼叫端的變數。


## 底下程式碼說明在Color的範例中如何使用ref關鍵字： ##

~~~csharp
using System;
class Color
{
	public Color( )
	{
		this.red =255;
		this.green =0;
		this.blue =125;
	}
	protected int red;
	protected int green;
	protected int blue;
	public void GetColors(ref int red,ref int green,ref int blue)
	{
		red =this.red;
		green =this.green;
		blue =this.blue;
	}
}
class RefTest1App
{
	public static void Main( )
	{
		Color color =new Color( );
		int red;
		int green;
		int blue;
		color.GetColors(ref red,ref green,ref blue);
		Console.WriteLine("red ={0},green ={1},blue ={2}",
red,green,blue);
	}
}
~~~


使用ref關鍵字有個缺點，那就是當您使用ref關鍵字時，必須在傳給method前先做初始化的動作，因此上面的程式是無法通過編譯的。

## 我們必須把程式碼改成這樣： ##


~~~csharp
using System;
class Color
{
	public Color()
	{
		this.red =255;
		this.green =0;
		this.blue =125;
	}
	protected int red;
	protected int green;
	protected int blue;
	public void GetColors(ref int red,ref int green,ref int blue)
	{
		red =this.red;
		green =this.green;
		blue =this.blue;
	}
}
class RefTest2App
{
	public static void Main( )
	{
		Color color =new Color( );
		int red =0;
		int green =0;
		int blue =0;
		color.GetColors(ref red,ref green,ref blue);
		Console.WriteLine("red ={0},green ={1},blue ={2}",
red,green,blue);
	}
}
~~~

----------


在這程式中，要特地去初始化一個要被覆寫的變數似乎有點奇怪。

因此C#提供了另一個選擇，用out關鍵字：代表這傳入值有任何的改變都要讓呼叫端知道。


## 用out關鍵字 ##

~~~csharp
using System;
class Color
{
	public Color( )
	{
		this.red =255;
		this.green =0;
		this.blue =125;
	}
	protected int red;
	protected int green;
	protected int blue;
	public void GetColors(out int red,out int green,out int blue)
	{
		red =this.red;
		green =this.green;
		blue =this.blue;
	}
}
class OutTest1App
{
	public static void Main( )
	{
		Color color =new Color( );
		int red;
		int green;
		int blue;
		color.GetColors(out red,out green,out blue);
		Console.WriteLine("red ={0},green ={1},blue ={2}",
					red,green,blue);
	}
}
~~~

這兩個關鍵字ref與out唯一的差異就是用out關鍵字不需要在參數傳入前先初始化。

那要ref關鍵字幹嘛呢？

當您需要保證呼叫端必須初始化參數才可使用時就會用到關鍵字ref。

在之前的範例中，我們會用out是因為被呼叫的method並不需要管它得到什麼樣的變數值。

如果被呼叫的method需要傳入值來運作呢？




## 看下面程式碼： ##

~~~csharp
using System;
class Window
{
	public Window(int x,int y)
	{
		this.x =x;
		this.y =y;
	}
	protected int x;
	protected int y;
	public void Move(int x,int y)
	{
		this.x =x;
		this.y =y;
	}
	public void ChangePos(ref int x,ref int y)
	{
		this.x +=x;;
		this.y +=y;
		x =this.x;
		y =this.y;
	}
}
class OutTest2App
{
	public static void Main( )
	{
		Window wnd =new Window(5,5);
		int x =5;
		int y =5;
		wnd.ChangePos(ref x,ref y);
		Console.WriteLine("{0},{1}",x,y);
		x =-1;
		y =-1;
		wnd.ChangePos(ref x,ref y);
		Console.WriteLine("{0},{1}",x,y);
	}
}
~~~


您可以看到，被呼叫的method，Window.ChangePos，會根據傳入值來運作。在這種情形下，ref關鍵字可以強迫呼叫端必須先做初始化以確保程式正確執行。

----------

參考：

- 書名：完全剖析C#, 出版日期：2001/10/30
