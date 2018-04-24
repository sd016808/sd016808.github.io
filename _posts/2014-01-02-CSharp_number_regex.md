---
layout: post
title: 'C# 使用 正規表達式 Regex 驗證'
author: 'James Peng'
tags: ['C#']
---

## 驗證輸入是否為數字 ##

~~~csharp
        /// <summary>
        /// 驗證輸入是否為數字
        /// </summary>
        /// <param name="str_number">用戶輸入的字串</param>
        /// <returns>方法返回布爾值</returns>
        public bool IsNumber(string str_number)
        {
            return System.Text.RegularExpressions.Regex.//使用正則表達式判斷是否匹配
                IsMatch(str_number, @"^[0-9]*$");
        }
~~~

----------

## 驗證輸入是否為非零正整數 ##

~~~csharp
        /// <summary>
        /// 驗證輸入是否為非零正整數
        /// </summary>
        /// <param name="str_intNumber">用戶輸入的數值</param>
        /// <returns>方法返回布林值</returns>
        public bool IsIntNumber(string str_intNumber)
        {
            return System.Text.RegularExpressions.Regex.//使用正規化運算式判斷是否符合
                IsMatch(str_intNumber, @"^\+?[1-9][0-9]*$");
        }
~~~

----------


## 驗證輸入是否為非零負整數 ##

~~~csharp
        /// <summary>
        /// 驗證輸入是否為非零負整數
        /// </summary>
        /// <param name="str_intNumber">用戶輸入的數值</param>
        /// <returns>方法返回布林值</returns>
        public bool IsIntNumber(string str_intNumber)
        {
            return System.Text.RegularExpressions.Regex.//使用正規劃運算式判斷是否符合
                IsMatch(str_intNumber, @"^\-[1-9][0-9]*$");
        }
~~~

----------


## 長度為6-18位 ##

~~~csharp
        /// <summary>
        /// 驗證密碼長度是否正確
        /// </summary>
        /// <param name="str_Length">密碼字串</param>
        /// <returns>方法返回布林值</returns>
        public bool IsPasswLength(string str_Length)
        {
            return System.Text.RegularExpressions.Regex.//使用正規化運算式判斷是否符合
                IsMatch(str_Length, @"^\d{6,18}$");
        }
~~~

----------


## 小數點兩位 ##

~~~csharp
        /// <summary>
        /// 驗證小數是否正確
        /// </summary>
        /// <param name="str_decimal">小數字串</param>
        /// <returns>返回布爾值</returns>
        public bool IsDecimal(string str_decimal)
        {
            return System.Text.RegularExpressions.Regex.//使用正則表達式判斷是否匹配
                IsMatch(str_decimal, @"^[0-9]+(.[0-9]{2})?$");
        }
~~~


----------

## 驗證月份是否正確 ##

~~~csharp
        /// <summary>
        /// 驗證月份是否正確
        /// </summary>
        /// <param name="str_Month">月份訊息字串</param>
        /// <returns>返回布爾值</returns>
        public bool IsMonth(string str_Month)
        {
            return System.Text.RegularExpressions.Regex.//使用正則表達式判斷是否匹配
                IsMatch(str_Month, @"^(0?[[1-9]|1[0-2])$");
        }
~~~


----------


## 驗證每月的31天 ##

~~~csharp
        /// <summary>
        /// 驗證每月的31天
        /// </summary>
        /// <param name="str_day">每月的天數</param>
        /// <returns>返回布爾值</returns>
        public bool IsDay(string str_day)
        {
            return System.Text.RegularExpressions.Regex.//使用正則表達式判斷是否匹配
                IsMatch(str_day, @"^((0?[1-9])|((1|2)[0-9])|30|31)$");
        }
~~~


----------


## 驗證輸入字符是否為大寫字母 ##

~~~csharp

        /// <summary>
        /// 驗證輸入字符是否為大寫字母
        /// </summary>
        /// <param name="str_UpChar">用戶輸入的字串</param>
        /// <returns>方法返回布林值</returns>
        public bool IsUpChar(string str_UpChar)
        {
            return System.Text.RegularExpressions.Regex.//使用正規化運算式判斷是否符合
                IsMatch(str_UpChar, @"^[A-Z]+$");
        }
~~~


----------


## 驗證輸入字符是否為小寫字母 ##

~~~csharp
        /// <summary>
        /// 驗證輸入字符是否為小寫字母
        /// </summary>
        /// <param name="str_UpChar">用戶輸入的字串</param>
        /// <returns>方法返回布林值</returns>
        public bool IsUpChar(string str_UpChar)
        {
            return System.Text.RegularExpressions.Regex.//使用正規化運算式判斷是否匹配
                IsMatch(str_UpChar, @"^[a-z]+$");
        }
~~~


----------

## 正則運算式匹配 ##

~~~csharp
public struct RegularExp 
    { 
        public const string Chinese = @"^[\u4E00-\u9FA5\uF900-\uFA2D]+$"; 
        public const string Color = "^#[a-fA-F0-9]{6}"; 
        public const string Date = @"^((((1[6-9]|[2-9]\d)\d{2})-(0?[13578]|1[02])-(0?[1-9]|[12]\d|3[01]))|(((1[6-9]|[2-9]\d)\d{2})-(0?[13456789]|1[012])-(0?[1-9]|[12]\d|30))|(((1[6-9]|[2-9]\d)\d{2})-0?2-(0?[1-9]|1\d|2[0-8]))|(((1[6-9]|[2-9]\d)(0[48]|[2468][048]|[13579][26])|((16|[2468][048]|[3579][26])00))-0?2-29-))$"; 
        public const string DateTime = @"^((((1[6-9]|[2-9]\d)\d{2})-(0?[13578]|1[02])-(0?[1-9]|[12]\d|3[01]))|(((1[6-9]|[2-9]\d)\d{2})-(0?[13456789]|1[012])-(0?[1-9]|[12]\d|30))|(((1[6-9]|[2-9]\d)\d{2})-0?2-(0?[1-9]|1\d|2[0-8]))|(((1[6-9]|[2-9]\d)(0[48]|[2468][048]|[13579][26])|((16|[2468][048]|[3579][26])00))-0?2-29-)) (20|21|22|23|[0-1]?\d):[0-5]?\d:[0-5]?\d$"; 
        public const string Email = @"^[\w-]+(\.[\w-]+)*@[\w-]+(\.[\w-]+)+$"; 
        public const string Float = @"^(-?\d+)(\.\d+)?$"; 
        public const string ImageFormat = @"\.(?i:jpg|bmp|gif|ico|pcx|jpeg|tif|png|raw|tga)$"; 
        public const string Integer = @"^-?\d+$"; 
        public const string IP = @"^(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])$"; 
        public const string Letter = "^[A-Za-z]+$"; 
        public const string LowerLetter = "^[a-z]+$"; 
        public const string MinusFloat = @"^(-(([0-9]+\.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*\.[0-9]+)|([0-9]*[1-9][0-9]*)))$"; 
        public const string MinusInteger = "^-[0-9]*[1-9][0-9]*$"; 
        public const string Mobile = "^0{0,1}13[0-9]{9}$"; 
        public const string NumbericOrLetterOrChinese = @"^[A-Za-z0-9\u4E00-\u9FA5\uF900-\uFA2D]+$"; 
        public const string Numeric = "^[0-9]+$"; 
        public const string NumericOrLetter = "^[A-Za-z0-9]+$"; 
        public const string NumericOrLetterOrUnderline = @"^\w+$"; 
        public const string PlusFloat = @"^(([0-9]+\.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*\.[0-9]+)|([0-9]*[1-9][0-9]*))$"; 
        public const string PlusInteger = "^[0-9]*[1-9][0-9]*$"; 
        public const string Telephone = @"(\d+-)?(\d{4}-?\d{7}|\d{3}-?\d{8}|^\d{7,8})(-\d+)?"; 
        public const string UnMinusFloat = @"^\d+(\.\d+)?$"; 
        public const string UnMinusInteger = @"\d+$"; 
        public const string UnPlusFloat = @"^((-\d+(\.\d+)?)|(0+(\.0+)?))$"; 
        public const string UnPlusInteger = @"^((-\d+)|(0+))$"; 
        public const string UpperLetter = "^[A-Z]+$"; 
        public const string Url = @"^http(s)?://([\w-]+\.)+[\w-]+(/[\w- ./?%&amp;=]*)?$"; 
    }
~~~

----------


## 判斷字串是否與指定正則運算式匹配 ##

~~~csharp
        public static bool IsMatch(string input, string regularExp) 
        { 
            return Regex.IsMatch(input, regularExp); 
        }
~~~

----------


## 驗證非負整數（正整數 + 0）  ##

~~~csharp
        public static bool IsUnMinusInt(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.UnMinusInteger); 
        }
~~~

----------


## 驗證正整數  ##

~~~csharp
        public static bool IsPlusInt(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.PlusInteger); 
        }
~~~

----------


## 驗證非正整數（負整數 + 0）   ##

~~~csharp
        public static bool IsUnPlusInt(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.UnPlusInteger); 
        }
~~~


----------


## 驗證負整數   ##

~~~csharp
        public static bool IsMinusInt(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.MinusInteger); 
        }
~~~

----------


## 驗證整數 ##

~~~csharp
        public static bool IsInt(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.Integer); 
        }
~~~


----------


##  驗證非負浮點數（正浮點數 + 0） ##

~~~csharp
        public static bool IsUnMinusFloat(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.UnMinusFloat); 
        }
~~~

----------


##  驗證正浮點數  ##

~~~csharp
        public static bool IsPlusFloat(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.PlusFloat); 
        }
~~~

----------


##  驗證非正浮點數（負浮點數 + 0）  ##

~~~csharp
        public static bool IsUnPlusFloat(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.UnPlusFloat); 
        }
~~~

----------

## 驗證負浮點數 ## 

~~~csharp
        public static bool IsMinusFloat(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.MinusFloat); 
        }
~~~

----------

## 驗證浮點數 ## 

~~~csharp
        public static bool IsFloat(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.Float); 
        }
~~~

----------

## 驗證由26個英文字母組成的字串  ##

~~~csharp
        public static bool IsLetter(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.Letter); 
        }
~~~

----------

 ## 驗證由中文組成的字串  ##

~~~csharp
        public static bool IsChinese(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.Chinese); 
        }
~~~

----------

## 驗證由26個英文字母的大寫組成的字串  ##

~~~csharp
        public static bool IsUpperLetter(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.UpperLetter); 
        }
~~~

----------

## 驗證由26個英文字母的小寫組成的字串  ##

~~~csharp
        public static bool IsLowerLetter(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.LowerLetter); 
        }
~~~

----------

## 驗證由數位和26個英文字母組成的字串  ##
~~~csharp
        public static bool IsNumericOrLetter(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.NumericOrLetter); 
        }
~~~

----------

## 驗證由數位組成的字串  ##

~~~csharp
        public static bool IsNumeric(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.Numeric); 
        } 
~~~

----------

## 驗證由數位和26個英文字母或中文組成的字串  ##

~~~csharp
        public static bool IsNumericOrLetterOrChinese(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.NumbericOrLetterOrChinese); 
        }
~~~

----------

## 驗證由數位、26個英文字母或者下劃線組成的字串  ##

~~~csharp
        public static bool IsNumericOrLetterOrUnderline(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.NumericOrLetterOrUnderline); 
        }
~~~

----------

## 驗證email地址  ##

~~~csharp
        public static bool IsEmail(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.Email); 
        }
~~~

----------

## 驗證URL  ##

~~~csharp
        public static bool IsUrl(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.Url); 
        }
~~~

----------

## 驗證電話號碼 ## 

~~~csharp
        public static bool IsTelephone(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.Telephone); 
        }
~~~

----------

## 驗證手機號碼  ##

~~~csharp
        public static bool IsMobile(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.Mobile); 
        }
~~~

----------

## 通過檔副檔名驗證圖像格式  ##

~~~csharp
        public static bool IsImageFormat(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.ImageFormat); 
        }
~~~

----------


## 驗證IP  ##

~~~csharp
        public static bool IsIP(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.IP); 
        }
~~~

----------


## 驗證日期（YYYY-MM-DD）  ##

~~~csharp
        public static bool IsDate(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.Date); 
        }
~~~

----------


## 驗證日期和時間（YYYY-MM-DD HH:MM:SS）  ##

~~~csharp
        public static bool IsDateTime(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.DateTime); 
        }
~~~

----------


## 驗證顏色（#ff0000）  ##

~~~csharp
        public static bool IsColor(string input) 
        { 
            return Regex.IsMatch(input, RegularExp.Color); 
        } 
~~~


----------

參考：

- http://tsczx.pixnet.net/blog/post/24462274-c%23-%E4%BD%BF%E7%94%A8%E6%AD%A3%E5%89%87%E9%81%8B%E7%AE%97%E5%BC%8F%E5%88%A4%E6%96%B7%E5%90%84%E7%A8%AE%E8%BC%B8%E5%85%A5%E5%80%BC
