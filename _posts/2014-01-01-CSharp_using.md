---
layout: post
title: 'C# 使用 using 關鍵字有效回收資源'
author: 'James Peng'
tags: ['C#']
---

![](..\images\2014-01-01-CSharp_using\ciEqhx6.png)

~~~csharp
        private void btn_true_Click(object sender, EventArgs e)
        {
            using (new test())//在using語句中建立test物件
            {
            }//using語句區塊執行完成後會自動呼叫test物件的Dispose方法
        }
        class test : IDisposable//定義test類別實現IDisposable介面
        {
            public void Dispose()//實現介面方法
            {
                MessageBox.Show(//彈出消息對話框
                    "已經執行test物件的Dispose方法", "提示");
            }
        }
~~~
