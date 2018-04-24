---
layout: post
title: 'C# 善用 ApplicationSettings 儲存設定值'
author: 'James Peng'
tags: ['WinForm']
---



## 問題 ##

如何讓 combolist 存值下次開啟程式卻不會不消失？

問題來源：[MSDN 論壇](https://social.msdn.microsoft.com/Forums/zh-TW/7b74da1d-d43c-43a2-9f62-b384ce1a9338/combolist?forum=233)

----------

## 實作 ##

操作環境: C#2013, WinForm

有時候我們希望程式可以記下使用者的設定 

下次開啟時可以繼續沿用這些設定值 

方法有很多種 (用登錄檔、寫INI、自己定義XML...等) 

這裡介紹一種內建的方法 ApplicationSettings 來儲存這些設定值


----------

## 第一步 ##

首先，點選comboBox1裡的 ApplicationSettings 欄位 

點一下PropertyBinding的[...]來做屬性繫結設定


![](..\images\2016-04-15-CSharp_ApplicationSettings\HIgNt4h.png)



----------

## 第二步 ##

在Text欄位右邊點下拉按[(新增...)] 

預設值是[選項1] 

欄位Name可以自訂，我輸入[comboItems] 來和 comboBox1.Text 屬性做連繫 

按下確定後可以看到資料連繫後的變化

![](..\images\2016-04-15-CSharp_ApplicationSettings\tGLEfcC.png)


----------

## 第三步 ##


我們在[save]按鈕中輸入以下程式碼

~~~csharp
        private void button1_Click(object sender, EventArgs e)
        {
            Properties.Settings.Default.Save();
        }
~~~


----------

## 完工 ##

選擇後 按下save 重新開啟 都會記住存值

![](..\images\2016-04-15-CSharp_ApplicationSettings\rXIYVNf.png)



----------

## 後記 ##

他連繫的欄位其實就是放在 App.config 裡面

![](..\images\2016-04-15-CSharp_ApplicationSettings\hSbHj2j.png)


你可以直接用下面的程式碼取用[comboItems]的值

~~~csharp

       string  s = Properties.Settings.Default.comboItems;

~~~

## 參考： ##

- https://social.msdn.microsoft.com/Forums/zh-TW/7b74da1d-d43c-43a2-9f62-b384ce1a9338/combolist?forum=233
- https://dotblogs.com.tw/sam319/2010/01/01/12753
