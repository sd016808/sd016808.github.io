---
layout: post
title: 'C# Constant vs. Read-Only Field'
author: 'James Peng'
tags: ['C# 觀念釐清']
---

當程式執行時，一定會有許多資料是您不想去改變的，例如，您的程式會用到的資料檔案、數學相關類別會用到的圓周率、或是任何您知道絕對不會改變的值。

為了應付這樣的狀況，C#提供了兩種很類似的類別：constant與read-only field。


----------


## Constant ##

從constant（常數）這字就可以知道，這個由關鍵字const所修飾的值在程式執行期間都不會改變。

定義const時，有兩點規則要遵守：

- 第一，一個常數是在編譯時期就已經由程式設計師或編譯器決定它的值了。
- 第二，一個常數成員的值必須明確的指定。

要定義一個常數，就在要定義的成員前面加上關鍵字const，如下：

~~~csharp
using System;
class MagicNumbers
{
	public const double pi =3.1415;
	public const int answerToAllLifesQuestions =42;
}
class ConstApp
{
	public static void Main( )
	{
		Console.WriteLine("pi ={0},everything else ={1}",
			MagicNumbers.pi,MagicNumbers.answerToAllLifesQuestions);
	}
}
~~~

這程式有個重點要注意，我們並不需要將MagicNumber這類別實體化，因為const的成員預設都是static的。

我們來看看這程式的MSIL碼就會一目了然：



~~~csharp
answerToAllLifesQuestions :public static literal int32 =int32(0x0000002A)
pi :public static literal float64 =float64(3.1415000000000002)
~~~


----------

## Read-Only Field ##

把某個成員定義成const是很好的做法，因為它很明顯的告訴您，程式設計師要把它定義為絕不會改變的值，唯一的限制，就是您必須在編譯時期先知道它的值。

當一個值要等到執行時期才知道，但是在執行期間又必須是不可改變的，那該怎麼做？這在其他語言中是無解的，但C#的設計者已經創造了一個read-only field來解決這個問題。

當您把一個field用關鍵字readonly來定義時，您只有在constructor裡才能設定它的值。過了這個時點，不管是在class或是class的用戶端都不能再改它的值了。

假設您有個圖形程式，而您想要知道螢幕的解析度，就不能用const，因為要到執行時期才會知道使用者的螢幕解析度。因此程式要寫成：


~~~csharp
using System;
class GraphicsPackage
{
	public readonly int ScreenWidth;
	public readonly int ScreenHeight;
	public GraphicsPackage( )
	{
		this.ScreenWidth =1024;
		this.ScreenHeight =768;
	}
}
class ReadOnlyApp
{
	public static void Main( )
	{
		GraphicsPackage graphics =new GraphicsPackage( );
		Console.WriteLine("Width ={0},Height ={1}",
						graphics.ScreenWidth,
						graphics.ScreenHeight);
	}
}
~~~

乍看之下，這程式碼似乎符合我們需求，不過，似乎有個小小的問題：

我們定義的read-only field是instance field，也就是我們要把類別實體化後才能使用這些field。

其實這不但不是問題，反而才是我們所需要的做法，因為您可以在類別實體化時才決定read-only field的值。

但如果您又要它是static、又希望執行時期才決定它的值時，該怎麼做呢？

您必須在該field前加上static、readonly修飾子，它就成為一個 static constructor 。Static constructor是用來初始化一些static、readonly的field的。

現在我們將之前的範例做個修改，把螢幕解析度的field改為static、readonly，並新增一個static constructor。請注意我們在constructor前加的關鍵字static：


~~~csharp
using System;
class GraphicsPackage
{
	public static readonly int ScreenWidth;
	public static readonly int ScreenHeight;
	static GraphicsPackage( )
	{
		//	計算螢幕解析度的程式碼
		ScreenWidth =1024;
		ScreenHeight =768;
	}
}
class ReadOnlyApp
{
	public static void Main( )
	{
		Console.WriteLine("Width ={0},Height ={1}",
							GraphicsPackage.ScreenWidth,
							GraphicsPackage.ScreenHeight);
	}
}

~~~


----------

參考：

- 書名：完全剖析C#, 出版日期：2001/10/30
