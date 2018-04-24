---
layout: post
title: 'C# 序列化 Serializable'
author: 'James Peng'
tags: ['C#']
---

序列化（Serialization）是.NET平臺最酷的特性之一。

## 為什麼要序列化： ##

首先你應該明白序列化的目的就不難理解他了。

序列化的目的就是能在網路上傳輸物件，否則就無法實現面向物件的分散式計算。

比如你的用戶端要調用伺服器上的一個方法獲得一個產品物件，比如方法為：public   Product   findProduct(int   product_id);   

注意該方法返回一個Product物件，如果沒有序列化技術，用戶端就收不到返回的物件Product。而序列化的實現就是把物件變成一個可在網路上傳輸的位元組流。

利用序列化技術，可以實現物件的備份和還原。序列化可以將記憶體中的物件（或物件圖）序列化為資料流程，並保存到磁片上進行持久化；

還可以將資料流程反序列化為物件，實現物件的還原。序列化技術在分散式系統的資料傳輸中得到充分的利用，如：XML Web Service 利用XML序列化實現跨平臺，.NET Remoting 則用到了二進位序列化和SOAP序列化。

序列化是個很好用的開發方式，可以將任何繼承於 ISerializable 的 .NET 型別(Type)物件都可以被序列化成 Xml 或其他格式，以便於將複雜的物件資料儲存在資料庫或其他儲存媒體中。

## Serialize有2種作法 ##

1. 一種是存成binary 
2. 一種是存成xml


網路上的範例都太複雜了 基本的作法如下 幾行程式碼而已


首先 當然你的class要有[Serializable]的標籤

## 參考命名空間 ##

~~~csharp
using System.Runtime.Serialization;
using System.Runtime.Serialization.Formatters.Binary;
~~~

## 宣告一個物件 ##

~~~csharp
[Serializable]
public class myclass
{
	public string text;
	public int number;
	public float number2;

	public myclass()
	{
		text = "abc";
		number = 3;
		number2 = 12.34f;
	}
}
~~~


----------

然後兩種作法的讀寫範例程式碼如下 看你是要存成binary還是存成xml:

## Binary 存檔 ##

~~~csharp
myclass myobj = new myclass();

IFormatter binFmt = new BinaryFormatter();
Stream s = File.Open("output.bin", FileMode.Create);
binFmt.Serialize(s, myobj);
s.Close();
~~~

![](..\images\2010-12-20-CSharp_Serializable\VEZm3KR.png)

----------


## Binary 讀檔 ##

~~~csharp
            myclass myobj = new myclass();
            myobj.text = "aaa";
            myobj.number = 0;
            myobj.number2 = 0;

			IFormatter binFmt = new BinaryFormatter();
			Stream s = File.Open("output.bin", FileMode.Open);
			myobj = (myclass)binFmt.Deserialize(s);
			s.Close();
~~~

![](..\images\2010-12-20-CSharp_Serializable\ZBFXeEK.png)

----------


## XML 存檔 ##

~~~csharp
            myclass myobj = new myclass();

            System.Xml.Serialization.XmlSerializer ser = new System.Xml.Serialization.XmlSerializer(myobj.GetType());
            Stream s = File.Open("output.xml", FileMode.Create);
            ser.Serialize(s, myobj);
            s.Close();
~~~

![](..\images\2010-12-20-CSharp_Serializable\Dn2Jy6r.png)

----------


## XML 讀檔 ##

~~~csharp
            myclass myobj = new myclass();
            myobj.text = "aaa";
            myobj.number = 0;
            myobj.number2 = 0;

            System.Xml.Serialization.XmlSerializer ser = new System.Xml.Serialization.XmlSerializer(myobj.GetType());
            Stream s = File.Open("output.xml", FileMode.Open);
            myobj = (myclass)ser.Deserialize(s);
            s.Close();
~~~

![](..\images\2010-12-20-CSharp_Serializable\HwtSGzD.png)


----------

## Binary 與 XML 比較 ##

binary較省空間 但可維護性較差 移植性也比較差 存檔只能由程式開啟

若遺失了原始的class定義 資料很難parse回來 因為是二進位格式

而xml存檔格式較通用 即使本來的程式遺失了仍然可透過xml parser把存檔中的子元素救回來

使用者也可直接透過記事本或xml editer編輯存檔裡的元素 不需透過特定程式

但是xml檔案所佔容量較大 且並非所有.NET原生物件都能存成xml

某些型別在Serialize成xml時會有問題

例如Font , Color等高階型別只能由binary寫入 透過XML方式寫入會產生執行時期錯誤

(這方面應該是.NET函式庫沒做好 技術上來說那些高階型別存成xml並不是不可能 或許有其他原因是我不知道的)

----------

參考：

- http://blog.wahahajk.com/2009/06/c-serialize-binaryxml.html
- http://blog.xuite.net/grimmslaw/78/52048294-%E5%BA%8F%E5%88%97%E5%8C%96%EF%BC%88Serialization%EF%BC%89
- http://blog.miniasp.com/post/2008/03/19/How-to-serialize-and-deserialize-using-C-NET.aspx
- https://msdn.microsoft.com/zh-tw/library/system.runtime.serialization.iserializable.aspx
- https://msdn.microsoft.com/zh-tw/library/system.xml.serialization.xmlserializer.aspx
