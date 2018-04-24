---
layout: post
title: 'C# Property'
author: 'James Peng'
tags: ['C#']
---

設計一個優良的class時，不但要做到將實作部分隱藏起來、還要禁止對class欄位的直接存取。透過accessor method（存取方法）來讀寫各欄位的值，可以確保任何對欄位的處理都在我們掌控之中，也就是說不會發生不合理的動作，並且確保所有該做的後續動作都正常執行。

舉個例子來說，假設您有個表示地址的類別，包含了郵遞區號與都市的欄位。當客戶端修改了Address.ZipCode時，會自動到資料庫檢查郵遞區號是否有效，並在Address.City中設定對應的都市。

如果客戶端能直接存取public的Address.ZipCode，那您就無法做一些額外的處理，因為客戶端不透過method存取它。因此與其讓人直接存取Address.ZipCode欄位，不如把Address.ZipCode與Address.City設定為protected，然後提供一些存取方法來設定Address.ZipCode欄位。如此您就可以在程式碼中加上想做的額外處理。

郵遞區號的範例程式如下。請注意我們已經把ZipCode欄位設為pro-tected，任何客戶端都不能直接存取它，並且定義了兩個public的存取方法，GetZipCode與SetZipCode。


~~~csharp
class Address
{
	protected string ZipCode;
	protected string City;
	public string GetZipCode( )
	{
		return this.ZipCode;
	}
	public void SetZipCode(string ZipCode)
	{
		//	檢查ZipCode是否存在
		this.ZipCode =ZipCode;
		//	根據郵遞區號更新City欄位
	}
}
~~~

客戶端必須這樣存取Address.ZipCode：

~~~csharp
Address addr =new Address( );
addr.SetZipCode("55555 ");
string zip =addr.GetZipCode( );
~~~


----------

## Property的定義與使用 ##

許多物件導向語言，如C++與Java，都使用accessor method來處理資料，而這的確也很好用。

然而C#提供了更好的機制，property，來取代accessor method，讓客戶端程式碼變得更優雅。

藉由property，客戶端可以像在存取一個普通的public field，不需要知道是否有accessor method的存在。

一個C#的property包括了一個field的宣告和用來修改field值accessor method。這些accessor method稱為getter與setter方法。

Getter用來獲得field值，而setter用來修改field值。再將先前的範例用C#的property重寫一次，如下：


~~~csharp
class Address
{
	protected string city;
	protected string zipCode;
	public string ZipCode
	{
		get
		{
			return zipCode;
		}
		set
		{
			//	根據資料庫來驗證資料
			zipCode =value;
			//	根據驗證過的zipCode來更新city
		}
	}
}
~~~

我們建立了一個Address.zipCode的field與Address.ZipCode的property。一開始可能有人會以為我們定義兩次Address.ZipCode field。

事實上是在定義property，而非field。

Property可以讓我們將accessor定義在類別成員裡面，提供一種更直覺的語法，利用object.field的形式來存取資料。如果在程式中忘記建立Address.zipCode，並且把setter中的zipCode=value改成ZipCode=value，這setter將會陷入無窮迴圈。
此外，表面上這setter並沒有接受任何參數，實際上要傳給它的參數會自動放在叫做value的變數中，讓setter可以存取它。



----------


## 唯讀的Property  ##

之前的範例中，Address.ZipCode同時具備了getter與setter，因此都是可讀可寫的。

當然，有時候您可能希望客戶端不能修改field內容，想把它設定為唯讀屬性。這時候您只要省略掉setter method即可。

為了示範如何使用唯讀的property，在此假定不准客戶端修改Address.city，客戶端只能透過Address.ZipCode來修改這field的值：

~~~csharp
class Address
{
	protected string city;
	public string City
	{
		get
		{
			return city;
		}
	}
	protected string zipCode;
	public string ZipCode
	{
		get
		{
			return zipCode;
		}
		set
		{
			//	根據資料庫驗證資料
			zipCode =value;
			//	根據驗證過的zipCode更新city
		}
	}
}
~~~



## Property的繼承 ##

跟method一樣，可以用virtual、override或是abstract來修飾它。

因此，衍生類別可以繼承、改寫property，就像其他來自基礎類別的成員一樣。

不過重要的是，您只能針對property加上這些修飾子。換句話說，如果您同時有getter與setter method，您改寫了其中一個就得同時改寫另一個。


----------

## Property的進階使用 ##


目前為止，我們知道property的用途有：

- 為客戶端提供更進一步的抽象化。
- 透過object.field的語法，對類別成員提供了一種統一的存取方法。
- 讓類別能保證某個field被修改時，一定能做一些額外處理。

第三種功能讓property有另一個優點：lazy initialization。這是一種最佳化技術，可以讓類別的成員在被使用到時才被初始化。

當您的class含有一個很少被用到的成員，而這成員的初始化很耗時又佔資源，這時候lazy initialization就很有用了。最常遇到的狀況就是我們需要從資料庫或是繁忙的網路上擷取資料。因為這些成員很少被用到而且初始化成本又高，所以可以把初始化動作延後到它被getter呼叫時。假設您有個庫存程式，銷售人員常在他們的電腦上執行它，並且用來存放顧客訂單、隨時檢查庫存狀況。利用property，您可以先實體化相關的類別，但不一定要同時讀取庫存資料（請參考以下範例）。一直到銷售人員想要實際存取庫存記錄時，getter才會去存取遠端資料庫。


~~~csharp
class Sku
{
	protected double onHand;
	public string OnHand
	{
		get
		{
			//	由資料庫讀取資料並設定onHand的值
			return onHand;
		}
	}
}
~~~

----------

目前為止，您知道property為field提供了accessor method，並且提供客戶端一個更統一、更直覺的介面。

因此property又被稱作smart field。現在，讓我們更進一步來看看C#中如何定義array。學習如何在array中使用property，也就是indexer。


----------

參考：

- 書名：完全剖析C#, 出版日期：2001/10/30
