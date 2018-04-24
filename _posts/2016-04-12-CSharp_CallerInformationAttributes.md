---
layout: post
title: 'C# 5.0 使用 Caller Information Attributes'
author: 'James Peng'
tags: ['C# 5.0']
---

從 C# 5.0 開始 C# 支援 Caller Information

顧名思義 就是 "Caller Information" 

這對 Debug 或者是 Logging 都很有用...可以很方便取得 caller 的 Information

Caller Information有三個主要Attribute，分別為 : 

1. CallerFilePathAttribute: Full path of the source file 
2. CallerLineNumberAttribute: Line number in the source file at which the method is called. 
3. CallerMemberNameAttribute: Method or property name 

使用方法很簡單，只需把Attribute放置在Optional Parameters前端，就如一般的Class Attribute一樣。 

以下面程式碼為例:

~~~csharp
using System;
using System.Runtime.CompilerServices;
using System.Windows.Forms;

namespace WindowsFormsApplication2
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            MyFunction(" I Know That Feel Bro.");

        }

        private void MyFunction(string WhyCallMe,
                               [CallerMemberName] string MemberName = "",
                               [CallerFilePath] string FilePath = "",
                               [CallerLineNumber] int LineNumber = 0)
        {
            MessageBox.Show(
            string.Format("\r\n WhyCallMe : {0} \r\n " +
                                       "Time : {1} \r\n " +
                                       "Member name : {2} \r\n " +
                                       "FilePath : {3} \r\n " +
                                       "LineNumber : {4} ",
                                       WhyCallMe, DateTime.Now.ToString(), MemberName, FilePath, LineNumber.ToString()));
        }
    }
}

~~~

最後輸出

![](..\images\2016-04-12-CSharp_CallerInformationAttributes\0tRWb1D.png)

會自動帶入 Caller Information 的值

----------


## 參考： ##

- http://tatmingstudio.blogspot.tw/2012/11/c-50-caller-information-attributes.html
- https://msdn.microsoft.com/en-us/library/hh534540.aspx
