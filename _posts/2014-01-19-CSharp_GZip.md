---
layout: post
title: 'C# 使用GZip壓縮檔案'
author: 'James Peng'
tags: ['C# 3rd-Party Library']
---

## C# 使用GZip壓縮檔案 ##

GZipStream 物件使用的是 GZ 演算法，而非標準 Zip 演算法
.GZ 和 .ZIP 是不同的

## 引用 ##

~~~csharp
using System.IO.Compression;
~~~

## 壓縮 ##

~~~csharp
        private void button2_Click(object sender, EventArgs e)
        {

            if (String.IsNullOrEmpty(textBox1.Text))
            {
                MessageBox.Show("請選擇源檔案!", "訊息提示");
                return;
            }

            if (String.IsNullOrEmpty(textBox2.Text))
            {
                MessageBox.Show("請輸入壓縮檔案名!", "訊息提示");
                return;
            }

            string str1 = textBox1.Text;
            string str2 = textBox2.Text.Trim() + ".gzip";
            byte[] myByte = null;
            FileStream myStream = null;
            FileStream myDesStream = null;
            GZipStream myComStream = null;
            try
            {
                myStream = new FileStream(str1, FileMode.Open, FileAccess.Read, FileShare.Read);
                myByte = new byte[myStream.Length];
                myStream.Read(myByte, 0, myByte.Length);
                myDesStream = new FileStream(str2, FileMode.OpenOrCreate, FileAccess.Write);
                myComStream = new GZipStream(myDesStream, CompressionMode.Compress, true);
                myComStream.Write(myByte, 0, myByte.Length);
                MessageBox.Show("壓縮檔案完成！");
            }
            catch { }
            finally
            {
                myStream.Close();
                myComStream.Close();
                myDesStream.Close();
            }
        }
~~~


----------

## 解壓縮 ##

~~~csharp
        private void button2_Click(object sender, EventArgs e)
        {

            if (String.IsNullOrEmpty(textBox1.Text))
            {
                MessageBox.Show("請選擇GZIP檔案!", "訊息提示");
                return;
            }

            if (String.IsNullOrEmpty(textBox2.Text))
            {
                MessageBox.Show("請輸入解壓檔案名!", "訊息提示");
                return;
            }

            string str1 = textBox1.Text;
            string str2 = textBox2.Text.Trim();
            byte[] myByte = null;
            FileStream myStream = null;
            FileStream myDesStream = null;
            GZipStream myDeComStream = null;
            try
            {
                myStream = new FileStream(str1, FileMode.Open);
                myDeComStream = new GZipStream(myStream, CompressionMode.Decompress, true);
                myByte = new byte[4];
                int myPosition = (int)myStream.Length - 4;
                myStream.Position = myPosition;
                myStream.Read(myByte, 0, 4);
                myStream.Position = 0;
                int myLength = BitConverter.ToInt32(myByte, 0);
                byte[] myData = new byte[myLength + 100];
                int myOffset = 0;
                int myTotal = 0;
                while (true)
                {
                    int myBytesRead = myDeComStream.Read(myData, myOffset, 100);
                    if (myBytesRead == 0)
                        break;
                    myOffset += myBytesRead;
                    myTotal += myBytesRead;
                }
                myDesStream = new FileStream(str2, FileMode.Create);
                myDesStream.Write(myData, 0, myTotal);
                myDesStream.Flush();
                MessageBox.Show("解壓檔案完成！");
            }
            catch { }
            finally
            {
                myStream.Close();
                myDeComStream.Close();
                myDesStream.Close();
            }
        }
~~~

----------

參考：

- https://msdn.microsoft.com/zh-tw/library/system.io.compression.gzipstream(v=vs.110).aspx

