---
layout: post
title: 'C# WinForm Process.Start外部exe 如何完全阻斷父界面接收事件'
author: 'James Peng'
tags: ['WinForm']
---

打開外部exe之後，啟動exe的父界面要完全不能進行任何操作。

當然按常人所想再加一句waitforexit就能決了啦，

在exe啟動的時候跑去父界面隨便點了一個按鈕，然後奇怪的事情就發生了：

在exe關閉之後，你剛剛點擊的那個按鈕就會裡面響應。

其實最後發現不止是按鈕，是整個界面都會在exe啟動的過程中響應鼠標事件，但是需求要和showdialog出子界面一樣的效果。



出錯的code

~~~csharp
using System;
using System.Diagnostics;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace WindowsFormsApplication6
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            this.button2.Enabled = false;
            string target = "calc.exe";
            ProcessStartInfo pInfo = new ProcessStartInfo(target);
            using (Process p = new Process())
            {
                p.StartInfo = pInfo;
                p.Start();
                p.WaitForInputIdle();
                p.WaitForExit();
            }
            this.button2.Enabled = true;
        }

        private void button2_Click(object sender, EventArgs e)
        {
            MessageBox.Show("Error!");
        }

        private void button3_Click(object sender, EventArgs e)
        {
            this.button2.Enabled = false;
            Task.Run(()=> {
                string target = "calc.exe";
                ProcessStartInfo pInfo = new ProcessStartInfo(target);
                using (Process p = new Process())
                {
                    p.StartInfo = pInfo;
                    p.Start();
                    p.WaitForInputIdle();
                    p.WaitForExit();
                }
                this.FindForm().Enabled = true;
            });
        }
    }
}

~~~

----------

正確的code

~~~csharp
        private void button3_Click(object sender, EventArgs e)
        {
            this.button2.Enabled = false;
            Task.Run(()=> {
                string target = "calc.exe";
                ProcessStartInfo pInfo = new ProcessStartInfo(target);
                using (Process p = new Process())
                {
                    p.StartInfo = pInfo;
                    p.Start();
                    p.WaitForInputIdle();
                    p.WaitForExit();
                }
                this.FindForm().Enabled = true;
            });
        }
~~~

首先啟動exe的waitforexit只是將 UI thread 暫時阻塞它，

在exe保持啟動的過程中，你對父界面進行的任何操作其實都會像排隊一樣，等主線程被阻塞的事情結束之後，它就會開始反應了。

現在的做法是把啟動exe的事情放到新的thread裡面去做，不去阻塞UI thread，所以 被 disable 的 button 早已處理，

這樣就能完全阻止那種現象的發生。

## reference： ##

- http://www.cnblogs.com/dachuyin/p/6505837.html
- 
