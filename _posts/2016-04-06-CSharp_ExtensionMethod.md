---
layout: post
title: 'C# 3.0 使用 Extension Method 擴充方法'
author: 'James Peng'
tags: ['C# 3.0']
---


在.Net中，如果想要在已有的類別上去新增自己自訂的Function，應該要怎麼做呢？

這裡我希望字串反轉，但 string 裡 沒有 Reverse反轉這方法

![](..\images\2016-04-06-CSharp_ExtensionMethod\gFgroCr.png)



## 新增一個 Extension Method 擴充方法 ##

![](..\images\2016-04-06-CSharp_ExtensionMethod\y5lJBHY.png)

- 自訂的 類別 必須是 public static的
- 類別中的 function 必須是 public static 類型
- function的第一個傳入參數必須要加上 this 的關鍵字，而隨後跟著的是要擴充的類別名稱需要傳入參數的話，則在function中的第二個參數加入想要傳入的參數型態以及名稱

![](..\images\2016-04-06-CSharp_ExtensionMethod\N3vm7HV.png)

![](..\images\2016-04-06-CSharp_ExtensionMethod\keJTzt9.png)


## StringExtensions.cs ##

~~~csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Globalization;
using System.Linq;
using System.Numerics;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace TestReportGenerator.Extension
{
    public static class StringExtensions
    {
        public static string Reverse(this string s)
        {
            if (String.IsNullOrEmpty(s))
                return "";
            char[] charArray = new char[s.Length];
            int len = s.Length - 1;
            for (int i = 0; i <= len; i++)
            {
                charArray[i] = s[len - i];
            }
            return new string(charArray);
        }

        public static string ReverseByte(this string s)
        {
            if (String.IsNullOrEmpty(s))
                return "";

            string result = "";
            string[] strArray = s.Split(' ');
            if (strArray.Length > 1)
            { 
                for(int i=strArray.Length-1; i>=0;i--)
                {
                    if (!string.IsNullOrEmpty(strArray[i]))
                    {
                        result = result + strArray[i] + " ";
                    }                    
                }                 
            }

            return result.Trim();
        }

        public static int ToInt32(this string s)
        {
            try
            {
                return Int32.Parse(s);
            }
            catch
            {
                return 0;
            }            
        }

   

        public static int ToHexInt(this string s)
        {
            return Convert.ToInt32(s, 16);
        }

     

        public static string ToBinaryString(this string s)
        {

            ulong unsignedValue = UInt64.MaxValue;
            BigInteger iHexData = new BigInteger(unsignedValue);
            iHexData = BigInteger.Parse(
                    string.Format("0{0}", s),
                    NumberStyles.AllowHexSpecifier);


            if (iHexData > 0)
            {
                
                string result = iHexData.ToBinaryString();
                result = result.Substring(1, result.Length - 1);


                for (int iZeroCount = s.Length * 4 - result.Length; iZeroCount > 0; iZeroCount--)
                {
                    result = "0" + result;
                }

                return result;
            }
            else
            {
                string result = "";
                for (int iZeroCount = s.Length * 4; iZeroCount > 0; iZeroCount--)
                {
                    result += "0";
                }
                return result;
            }
        }


        public static string AddSpaceFromRight(this string result, int everLength)
        {

            string NewResult = "";
            for (int j = 0; j < result.Length; j++)
            {
                if (j % everLength == 0)
                {
                    NewResult = result.Substring((result.Length - 1) - j, 1) + " " + NewResult;
                }
                else
                {
                    NewResult = result.Substring((result.Length - 1) - j, 1) + NewResult;
                }
            }
            
            if((result.Length % everLength) > 0)
            {
                for (int getZeroCount = everLength - (result.Length % everLength); getZeroCount > 0; getZeroCount--)
                {
                    NewResult = "0" + NewResult.Trim();
                }
            }
             return NewResult.Trim();
        }

        public static string AddSpaceFromLeft(this string result , int everLength)
        {
            string NewResult = "";
            for (int j = 0; j < result.Length; j++)
            {
                if (j % everLength == 0)
                {
                    NewResult = NewResult + " " + result.Substring(j, 1);
                }
                else
                {
                    NewResult = NewResult + result.Substring(j, 1);
                }
            }
            return NewResult.Trim();
        }

        public static string NewLine(this string s)
        {
            return s += "\r\n";
        }

        /// <summary>
        /// 列舉擴充方法, 取得描述文字
        /// </summary>
        /// <param name="value">列舉</param>
        /// <returns>String</returns>
        public static string ToDescriptionString(this System.Enum value)
        {
            DescriptionAttribute[] attributes = (DescriptionAttribute[])value.GetType().GetField(value.ToString()).GetCustomAttributes(typeof(DescriptionAttribute), false);

            if (attributes != null && attributes.Length > 0)
                return attributes[0].Description;
            else
                return value.ToString();
        }



        public static char Chr(this int Num)
        {
            char C = Convert.ToChar(Num);
            return C;
        }

        public static int ASC(this string S)
        {
            int N = Convert.ToInt32(S[0]);
            return N;
        }

        public static int ASC(this char C)
        {
            int N = Convert.ToInt32(C);
            return N;
        }

    }


    public static class ControlExtensions
    {
        public static void Do<TControl>(this TControl control, Action<TControl> action)
          where TControl : Control
        {
            if (control.InvokeRequired)
                control.Invoke(action, control);
            else
                action(control);
        }
    }
}

~~~

----------

## BigIntegerExtensions.cs ##

~~~csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Globalization;
using System.Linq;
using System.Numerics;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace TestReportGenerator.Extension
{
    public static class StringExtensions
    {
        public static string Reverse(this string s)
        {
            if (String.IsNullOrEmpty(s))
                return "";
            char[] charArray = new char[s.Length];
            int len = s.Length - 1;
            for (int i = 0; i <= len; i++)
            {
                charArray[i] = s[len - i];
            }
            return new string(charArray);
        }

        public static string ReverseByte(this string s)
        {
            if (String.IsNullOrEmpty(s))
                return "";

            string result = "";
            string[] strArray = s.Split(' ');
            if (strArray.Length > 1)
            { 
                for(int i=strArray.Length-1; i>=0;i--)
                {
                    if (!string.IsNullOrEmpty(strArray[i]))
                    {
                        result = result + strArray[i] + " ";
                    }                    
                }                 
            }

            return result.Trim();
        }

        public static int ToInt32(this string s)
        {
            try
            {
                return Int32.Parse(s);
            }
            catch
            {
                return 0;
            }            
        }

   

        public static int ToHexInt(this string s)
        {
            return Convert.ToInt32(s, 16);
        }

     

        public static string ToBinaryString(this string s)
        {

            ulong unsignedValue = UInt64.MaxValue;
            BigInteger iHexData = new BigInteger(unsignedValue);
            iHexData = BigInteger.Parse(
                    string.Format("0{0}", s),
                    NumberStyles.AllowHexSpecifier);


            if (iHexData > 0)
            {
                
                string result = iHexData.ToBinaryString();
                result = result.Substring(1, result.Length - 1);


                for (int iZeroCount = s.Length * 4 - result.Length; iZeroCount > 0; iZeroCount--)
                {
                    result = "0" + result;
                }

                return result;
            }
            else
            {
                string result = "";
                for (int iZeroCount = s.Length * 4; iZeroCount > 0; iZeroCount--)
                {
                    result += "0";
                }
                return result;
            }
        }


        public static string AddSpaceFromRight(this string result, int everLength)
        {

            string NewResult = "";
            for (int j = 0; j < result.Length; j++)
            {
                if (j % everLength == 0)
                {
                    NewResult = result.Substring((result.Length - 1) - j, 1) + " " + NewResult;
                }
                else
                {
                    NewResult = result.Substring((result.Length - 1) - j, 1) + NewResult;
                }
            }
            
            if((result.Length % everLength) > 0)
            {
                for (int getZeroCount = everLength - (result.Length % everLength); getZeroCount > 0; getZeroCount--)
                {
                    NewResult = "0" + NewResult.Trim();
                }
            }
             return NewResult.Trim();
        }

        public static string AddSpaceFromLeft(this string result , int everLength)
        {
            string NewResult = "";
            for (int j = 0; j < result.Length; j++)
            {
                if (j % everLength == 0)
                {
                    NewResult = NewResult + " " + result.Substring(j, 1);
                }
                else
                {
                    NewResult = NewResult + result.Substring(j, 1);
                }
            }
            return NewResult.Trim();
        }

        public static string NewLine(this string s)
        {
            return s += "\r\n";
        }

        /// <summary>
        /// 列舉擴充方法, 取得描述文字
        /// </summary>
        /// <param name="value">列舉</param>
        /// <returns>String</returns>
        public static string ToDescriptionString(this System.Enum value)
        {
            DescriptionAttribute[] attributes = (DescriptionAttribute[])value.GetType().GetField(value.ToString()).GetCustomAttributes(typeof(DescriptionAttribute), false);

            if (attributes != null && attributes.Length > 0)
                return attributes[0].Description;
            else
                return value.ToString();
        }



        public static char Chr(this int Num)
        {
            char C = Convert.ToChar(Num);
            return C;
        }

        public static int ASC(this string S)
        {
            int N = Convert.ToInt32(S[0]);
            return N;
        }

        public static int ASC(this char C)
        {
            int N = Convert.ToInt32(C);
            return N;
        }

    }


    public static class ControlExtensions
    {
        public static void Do<TControl>(this TControl control, Action<TControl> action)
          where TControl : Control
        {
            if (control.InvokeRequired)
                control.Invoke(action, control);
            else
                action(control);
        }
    }
}

~~~


----------


## 參考： ##

- https://dotblogs.com.tw/bauann/2010/08/30/17498
- https://dotblogs.com.tw/larrynung/archive/2009/07/26/9682.aspx
- https://msdn.microsoft.com/zh-tw/library/bb383977.aspx
