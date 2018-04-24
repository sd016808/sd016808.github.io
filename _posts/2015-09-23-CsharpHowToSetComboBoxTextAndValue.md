---
layout: post
title: 'C# 使用 Key-Value 設定 ComboBox 的資料'
author: 'James Peng'
tags: ['WinForm']
---

有時候我們除了想要知道選取的項目外，還想要知道該項目的值。例如，如果從資料庫拉資料進ComboBox，我們想要同時把一筆資料的ID同時指定給該項目，以利於做其他判斷。這時候就必須讓ComboBox同時可以設定"項目名稱"及"值"。 

先寫一個函式會吐出ArrayList 裡面包含key-value

~~~csharp

        public ArrayList GetSomeList(string[] strKey, int[] iValue)
        {
            ArrayList data = new ArrayList();
            for (i = 0; i < count; ++i)
            {
                data.Add(new DictionaryEntry(strKey[i], iValue[i]));
            }
            return data;
        }
~~~


## Key-Value 設定 ComboBox 的資料 ##

利用ComboBox的DataSource屬性，把資料以Key-Value的方式指定給該屬性當作資料來源。

接著設定DisplayMember屬性為"Key"，ValueMember屬性為"Value"。 

~~~csharp
                ArrayList data = GetSomeList(strKey,iValue);
                
                this.comboBox_selectDevice.DisplayMember = "Key";
                this.comboBox_selectDevice.ValueMember = "Value";
                this.comboBox_selectDevice.DataSource = data;
~~~




參考：

- http://net-informations.com/q/faq/combovalue.html
- http://blog.tonycube.com/2011/09/comboboxcombobox-item-value.html
