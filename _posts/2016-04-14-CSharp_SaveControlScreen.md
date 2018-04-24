---
layout: post
title: 'C# 截取螢幕上元件畫面'
author: 'James Peng'
tags: ['WinForm']
---

若只需要儲存某個Control畫面，

例如 想截取某Panel畫面 或是 元件 

## 作法如下 ##

~~~csharp
        private void button1_Click(object sender, EventArgs e)
        {
            SaveScreen(button1, @"C:\temp\Test.png");
        }

        private static void SaveScreen(Control ctl, string path)
        {
            using (System.Drawing.Bitmap bmp = new System.Drawing.Bitmap(ctl.ClientSize.Width, ctl.ClientSize.Height))
            {
                using (System.Drawing.Graphics grps = System.Drawing.Graphics.FromImage(bmp))
                {
                    grps.CopyFromScreen(ctl.PointToScreen(System.Drawing.Point.Empty), new System.Drawing.Point(0, 0), ctl.ClientSize);
                    grps.ReleaseHdc(grps.GetHdc());
                }
                bmp.Save(path);
            }
        }
~~~


----------


## 結果如下 ##

![](..\images\2016-04-14-CSharp_SaveControlScreen\kYgld5J.png)


----------


## 參考： ##

- https://dotblogs.com.tw/abbee/2014/11/27/147447
