---
layout: post
title: 'C# BigInteger 處理超大數值'
author: 'James Peng'
tags: ['C# 4.0']
---



## 用途 ##

平常在寫程式處理數字一般使用 int 就夠了, 某些特殊情況可能會用到 long, 

但在某些情況下，你想把 Hex String 轉成 int 會超過最大值

如果你遇到 Uint64 還不夠處理的窘境，可以試試看 BigInteger Class，就可以處理超大數值。

在.net framework 4.0 中，System.Numerics.dll 中提供了BigInteger 類。

使用這個類可以很方便的解決這個問題。

所以下面來介紹 BigInteger 的用法:


----------

## 引用命名空間 ##

![](..\images\2016-04-08-CSharp_BigNumber\t6jTfdI.png)

~~~csharp
using System.Numerics;
~~~


----------


## 範例1: 檢查位元 ##

- 把 Hex String 轉 BigInteger
- 在檢查第 3 個bit 看是1還是0

~~~csharp
int bitNumber = 3;
BigInteger iData = BigInteger.Parse(
                strHex,
                NumberStyles.AllowHexSpecifier);
BigInteger iBit = (BigInteger)(iData >> bitNumber) & 1;

string strResult = (iBit == 1 ? "True" : "False");
~~~

----------


## 範例2: 判斷 n 是質數 ##

1. 如果是偶數，肯定不是質數
2. 如果能夠被小於或等於 Sqrt(n) 的數除盡，則不是質數。

~~~csharp
private  static  bool IsPrime()
{
    string largeNumber = @"300000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001";

    BigInteger bigInteger = BigInteger.Parse(largeNumber);
    if (bigInteger.IsEven)
    {
        return  false ;
    }
    for (BigInteger bi = 3; BigInteger.Pow(bi, 2) <= bigInteger; bi += 2)
    {
        if (bigInteger % bi == 0)
        {
            return  false ;
        }
    }
    return  true ;
}
~~~


----------


## 範例3: 10進制 轉 2進制 ##

~~~csharp
/// <summary>
        /// Converts a <see cref="BigInteger"/> to a binary string.
        /// </summary>
        /// <param name="bigint">A <see cref="BigInteger"/>.</param>
        /// <returns>
        /// A <see cref="System.String"/> containing a binary
        /// representation of the supplied <see cref="BigInteger"/>.
        /// </returns>
        public static string ToBinaryString(BigInteger bigint)
        {

            var bytes = bigint.ToByteArray();
            var idx = bytes.Length - 1;

            // Create a StringBuilder having appropriate capacity.
            var base2 = new StringBuilder(bytes.Length * 8);

            // Convert first byte to binary.
            var binary = Convert.ToString(bytes[idx], 2);

            // Ensure leading zero exists if value is positive.
            if (binary[0] != '0' && bigint.Sign == 1)
            {
                base2.Append('0');
            }

            // Append binary string to StringBuilder.
            base2.Append(binary);

            // Convert remaining bytes adding leading zeros.
            for (idx--; idx >= 0; idx--)
            {
                base2.Append(Convert.ToString(bytes[idx], 2).PadLeft(8, '0'));
            }

            return base2.ToString();
        }
~~~


----------


## 範例4: 10進制 轉 8進制 ##

~~~csharp

 		/// <summary>
        /// Converts a <see cref="BigInteger"/> to a octal string.
        /// </summary>
        /// <param name="bigint">A <see cref="BigInteger"/>.</param>
        /// <returns>
        /// A <see cref="System.String"/> containing an octal
        /// representation of the supplied <see cref="BigInteger"/>.
        /// </returns>
        public static string ToOctalString(BigInteger bigint)
        {
            var bytes = bigint.ToByteArray();
            var idx = bytes.Length - 1;

            // Create a StringBuilder having appropriate capacity.
            var base8 = new StringBuilder(((bytes.Length / 3) + 1) * 8);

            // Calculate how many bytes are extra when byte array is split
            // into three-byte (24-bit) chunks.
            var extra = bytes.Length % 3;

            // If no bytes are extra, use three bytes for first chunk.
            if (extra == 0)
            {
                extra = 3;
            }

            // Convert first chunk (24-bits) to integer value.
            int int24 = 0;
            for (; extra != 0; extra--)
            {
                int24 <<= 8;
                int24 += bytes[idx--];
            }

            // Convert 24-bit integer to octal without adding leading zeros.
            var octal = Convert.ToString(int24, 8);

            // Ensure leading zero exists if value is positive.
            if (octal[0] != '0' && bigint.Sign == 1)
            {
                base8.Append('0');
            }

            // Append first converted chunk to StringBuilder.
            base8.Append(octal);

            // Convert remaining 24-bit chunks, adding leading zeros.
            for (; idx >= 0; idx -= 3)
            {
                int24 = (bytes[idx] << 16) + (bytes[idx - 1] << 8) + bytes[idx - 2];
                base8.Append(Convert.ToString(int24, 8).PadLeft(8, '0'));
            }

            return base8.ToString();
        }

~~~

----------

參考：

- https://msdn.microsoft.com/zh-tw/library/system.numerics.biginteger(v=vs.100).aspx
- https://msdn.microsoft.com/zh-tw/library/system.globalization.numberstyles(v=vs.110).aspx
- http://www.cnblogs.com/LoveJenny/archive/2011/10/19/2217153.html
- http://www.codeproject.com/Articles/60108/BigInteger-Library

