---
layout: post
title: 'C# Hex String 轉 Byte Array'
author: 'James Peng'
tags: ['C#']
---

## Byte Array 轉 Hex String ##

~~~csharp
        public static string ByteArrayToString(byte[] ba)
        {
            string hex = BitConverter.ToString(ba);
            return "0x"+hex.Replace("-", "");
        }
~~~


----------

## Hex String 轉 Byte Array ##

~~~csharp
        public static byte[] StringToByteArray(String hex)
        {
            hex = hex.ToLower().Replace("0x","");
            int NumberChars = hex.Length;
            byte[] bytes = new byte[NumberChars / 2];
            for (int i = 0; i < NumberChars; i += 2)
                bytes[i / 2] = Convert.ToByte(hex.Substring(i, 2), 16);
            return bytes;
        }
~~~


----------


## 驗證 ##

~~~csharp
        private void button1_Click(object sender, EventArgs e)
        {
            byte[] byteArray = StringToByteArray("0x3F8CCCCD");

            string temp = "";
            foreach (var s in byteArray)
            {
                temp += s + " ";
            }

            MessageBox.Show(temp.Trim());

            MessageBox.Show(ByteArrayToString(byteArray));
        }
~~~

輸入 0x3F8CCCCD

![](..\images\2016-04-25-CSharp_ConvertByteArraytoHexadecimalString\iRutcxD.png)

- 063 = 0x3F = 0011 1111
- 140 = 0x8C = 1000 1100
- 204 = 0xCC = 1100 1100
- 205 = 0xCD = 1100 1101

再把 byteArray 轉回hex字串

![](..\images\2016-04-25-CSharp_ConvertByteArraytoHexadecimalString\TZvGi4A.png)

----------

參考：

- http://stackoverflow.com/questions/311165/how-do-you-convert-byte-array-to-hexadecimal-string-and-vice-versa
