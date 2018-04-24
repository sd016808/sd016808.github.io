---
layout: post
title: 'C# 取得 Enum 列舉常數的 Description 屬性'
author: 'James Peng'
tags: ['C#']
---

列舉型別(Enuymeration)讓開發人員透過可閱讀名稱，來表示程式中會使用到的常數值，使用上相當的方便。

但有時候光是在程式中看到這些簡化列舉名稱，總覺得還是少了些什麼。

在 C# 中，可以經由 Description 屬性來賦予每個列舉常數的詳細說明，但如果想要取得每個列舉值的描述說明資訊要如何做呢？提供網路上找到的解法提供給大家參考。


列舉型別的定義：

~~~csharp
namespace GISFCU.WRISP.Entity.Message
{
    /// 

    /// 認證及授權結果列舉
    /// 
    public enum EnumAuthecticationResult
    {
        None = -1, //無
        [Description("認證成功")]
        AuthecticationOk = 100,                 //認證成功
        [Description("認證權杖逾期")]
        AuthecticationTokenTimeout = 101,       //認證權杖逾期
        [Description("認證Token不合法")]
        InvalidAuthecticationToken = 102,       //認證Token不合法
        [Description("認證失敗")]
        AuthecticationFailed = 103,             //認證失敗
    }
}
~~~


----------


讀取列舉常數的輔助方法：

~~~csharp
namespace GISFCU.WRISP.Entity.Util
{
    public class EnumHelper
    {
        /// 

        /// 取得列舉的 Description
        /// 
        /// 
        /// 
        public static string GetEnumDescription(Enum value)
        {
            FieldInfo fi = value.GetType().GetField(value.ToString());

            DescriptionAttribute[] attributes =
                (DescriptionAttribute[])fi.GetCustomAttributes(
                typeof(DescriptionAttribute),
                false);

            if (attributes != null &&
                attributes.Length > 0)
                return attributes[0].Description;
            else
                return value.ToString();
        }
    }
}
~~~

----------

## 實際測試： ##

~~~csharp
        EnumAuthecticationResult authResult = EnumAuthecticationResult.AuthecticationOk;
        Console.WriteLine(EnumHelper.GetEnumDescription(authResult));
~~~

----------

## 輸出： ##

~~~text
 認證成功
~~~

----------
來源: 
http://jesily-tw.blogspot.tw/2013/04/c-description.html