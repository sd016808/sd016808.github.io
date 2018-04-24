---
layout: post
title: 'C# 取得漢字拼音首字母 透過微軟的Visual Studio International Pack 類別'
author: 'James Peng'
tags: ['C# 3rd-Party Library']
---

提取漢字拼音的方法可以使用​​微軟的一個類庫Visual Studio International Pack ，今天試了一試，確實好用！

下面分享下使用方法：

1. 首先下載Visual Studio International Pack 1.0，[官方下載地址](https://www.microsoft.com/zh-TW/download/details.aspx?id=15251)。
2. 下載完畢後解壓，解壓後可以發現7個MSI安裝文件，其中CHSPinYinConv.msi是漢字拼音組件，CHTCHSConv.msi是進行繁簡體互轉組件，安裝這兩個MSI就可以了(x86操作系統上的默認安裝目錄是C:\Program Files\ Microsoft Visual Studio International Pack \) 。
3. 安裝完畢後，需要在VS裡添加引用，分別引用：C:\Program Files\Microsoft Visual Studio International Pack\Simplified Chinese Pin-Yin Conversion Library（拼音）下和C:\Program Files\Microsoft Visual Studio International Pack\ Traditional Chinese to Simplified Chinese Conversion Library and Add-In Tool（繁簡互轉）下的dll即可使用。

完成上面的工作後，使用方法就非常簡單了，下面看代碼：

~~~csharp
using  Microsoft.International.Converters.PinYinConverter; // 導入拼音相關

namespace  WebApplication2 
{ 
    public  class  Class1 
    {    
        ///  <summary>  
        ///  漢字轉化為拼音
        ///  </summary>  
        ///  <param name="str "> 漢字</param>  
        ///  <returns> 全拼</returns>  
        public  static  string  GetPinyin( string  str) 
        { 
            string  r  =  string .Empty; 
            foreach  ( char  obj  in  str) 
            { 
                try 
                { 
                    ChineseChar chineseChar  =  new  ChineseChar (obj); 
                    string  t  =  chineseChar.Pinyins[ 0 ].ToString(); 
                    r  +=  t.Substring( 0 , t.Length  -  1 ); 
                } 
                catch 
                { 
                    r  +=  obj.ToString(); 
                } 
            } 
            return  r ; 
        } 

        ///  <summary>  
        ///  漢字轉化為拼音首字母
        ///  </summary>  
        ///  <param name="str"> 漢字</param>  
        ///  <returns> 首字母</ returns>  
        public  static  string  GetFirstPinyin( string  str) 
        { 
            string  r  =  string .Empty; 
            foreach  ( char  obj  in  str) 
            { 
                try 
                { 
                    ChineseChar chineseChar  =  new  ChineseChar(obj); 
                    string  t  =  chineseChar.Pinyins[ 0 ].ToString() ; 
                    r  +=  t.Substring( 0 ,  1 ); 
                } 
                catch 
                { 
                    r  +=  obj.ToString(); 
                } 
            } 
            return  r; 
        } 
    } 
}
~~~

調用方法：（注意先引用）

~~~csharp
GetPinyin("風影");//獲取全拼
GetFirstPinyin("風影");//獲取首字母
~~~
 

是不是非常簡單呢？有了這個類庫就省事多了！


----------


順便再補充一下繁簡體互轉的方法，某些時候可能會用到：

先導入

~~~csharp
using Microsoft.International.Converters.TraditionalChineseToSimplifiedConverter;
~~~


~~~csharp
///  <summary>  
        ///  簡體轉換為繁體
        ///  </summary>  
        ///  <param name="str"> 簡體字</param>  
        ///  <returns> 繁體字</returns>  
        public  static  string  GetTraditional( string  str) 
        { 
            string  r  =  string .Empty; 
            r  =  ChineseConverter.Convert(str, ChineseConversionDirection.SimplifiedToTraditional); 
            return  r; 
        } 
        ///  <summary>  
        ///  繁體轉換為簡體
        ///  </summary >  
        ///  <param name="str"> 繁體字</param>  
        ///  <returns> 簡體字</returns>  
        public  static  string  Get Simplified ( string  str) 
        { 
            string  r  =  string .Empty; 
            r  =  ChineseConverter. Convert(str, ChineseConversionDirection.TraditionalToSimplified); 
            return  r; 
        }
~~~



該類庫的功能概述

Microsoft Visual Studio International Pack 1.0版包括以下功能：

    East Asia Numeric Formatting Library - 支持將小寫的數字字符串格式化成簡體中文，繁體中文，日文和韓文的大寫數字字符串。
    Japanese Kana Conversion Library - 支持將日文假名（Kana）轉化為另一種日文字符。
    Japanese Text Alignment Library - 支持日文特有的一種對齊格式。
    Japanese Yomi Auto-Completion Library - 類庫支持感知日文輸入法的輸入自動完成和一個文本框控件的示例。
    Korean Auto Complete TextBox Control - 支持韓文輸入法的智能感知和輸入自動完成的文本框控件。
    Simplified Chinese Pin-Yin Conversion Library - 支持獲取簡體中文字符的常用屬性比如拼音，多音字，同音字，筆劃數。
    Traditional Chinese to Simplified Chinese Conversion Library and Add-In Tool - 支持簡繁體中文之間的轉換。該組件還包含一個Visual Studio集成開發環境中的插件（Add-in）支持簡繁體中文資源文件之間的轉換。
 

Visual Studio International Feature Pack 2.0 是對1.0 版本的擴展,包含一組控件和類庫：


- Yomigana Framework 包含了類庫和控件。
- 類庫：Yomigana 類庫容許對串（string）類型加註Yomigana，同時也支持對一般類型的註解功能，任何實現了IEnumerable接口的對像都可以被串類型和泛型的實例註解。為了簡化複雜的註解字符串比較特設計了支持各種日文比較選項的比較類型。
	- 通用的一些類，用泛型實現對一個可枚舉的類型注音。
	- 特殊目的的一些類，用以上泛型實現對一個字符串用某種類型中註音。
	- 特殊目的的一些StringAnnotation 類，用以上泛型實現對一個字符串用字符串注音，包括解析和格式化功能。
	- 一個比較器類，使用以上類實現比較字符串。
	- 一個實現了IEnumerable <string> 的數據結構，把一個字符串分成枚舉的字符串段，並用IEnumerator <string> 輸出。
- 控件：
	- 增強的Ajax/WPF/WinForm 文本框（TextBox）控件用來根據用戶的輸入捕獲讀音。
	- 一個增強的使用Ruby標籤的ASP.NET Label控件。
- Chinese Text Alignment Class Library and TextBox Controls 包含支持簡體中文文本對齊的WinForm 和WPF 的TextBox控件, 以及供幫助開發人員很容易地按中文文本對齊顯示字符串的一個類庫。
- Chinese Auto Complete Class Library and TextBox Controls 包含支持感知簡體中文和繁體中文輸入法並自動完成的WinForm 和WPF 的TextBox控件， 以及供開發人員很容易地向標準控件添加感知輸入法並自動完成功能的一個類庫。
- Korean Auto Complete Class Library and ComboBox Controls 包含支持感知韓語輸入法並自動完成的WinForm 和WPF 的ComboBox控件， 以及供開發人員很容易地向標準控件添加感知輸入法並自動完成功能的一個類庫。
- Numeric Formatting Class Library 包含支持五種語言的數字格式化成文字的類, 2.0 版支持格式化阿拉伯數字為阿拉伯文字。

可見，這個類庫在開發國際化程序時是非常實用的。 


----------

參考：

- http://www.shaoqun.com/a/15075.aspx
- http://www.cnblogs.com/yazdao/archive/2011/06/04/2072488.html
- https://www.microsoft.com/zh-TW/download/details.aspx?id=15251
