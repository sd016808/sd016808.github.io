---
layout: post
title: 'C# 取得資料夾建立時間'
author: 'James Peng'
tags: ['C#']
---

![](..\images\2014-01-17-CSharp_GetFoldCTime\TwmLAvi.png)

## C# 取得資料夾建立時間 ##

從 DirectoryInfo 物件裡取得

~~~csharp
        private void button1_Click(object sender, EventArgs e)
        {
            FolderBrowserDialog FBDialog = new FolderBrowserDialog();//建立FolderBrowserDialog物件
            if (FBDialog.ShowDialog() == DialogResult.OK)//判斷是否選擇資料夾
            {
                string strPath = FBDialog.SelectedPath;//記錄選擇的資料夾
                textBox1.Text = strPath;//顯示選擇的資料夾
                DirectoryInfo DInfo = new DirectoryInfo(strPath);//建立DirectoryInfo物件
                label2.Text = "建立時間：" + DInfo.CreationTime.ToString();//顯示資料夾建立時間
            }
        }
~~~


