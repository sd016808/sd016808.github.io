---
layout: post
title: 'C# 讓視窗縮小到工具列 NotifyIcon 與 contextMenuStrip 的應用'
author: 'James Peng'
tags: ['WinForm']
---

先拉 NotifyIcon 與 contextMenuStrip 這兩個元件到 Form

![](..\images\2015-09-24-CsharpNotifyIconAndContextMenuStrip\LJHvGF7.png)


點選Form右上角縮到最小之後，就會將Form隱藏，並將notifyicon顯示於右下角

~~~csharp
        private void MainForm_Resize(object sender, EventArgs e)
        {
            if (this.WindowState == FormWindowState.Minimized)
            {
                this.Hide();
                this.notifyIcon1.Visible = true;
                this.notifyIcon1.BalloonTipText = "Have a nice day!";
                this.notifyIcon1.BalloonTipTitle = AppInfo.PROGRAM + " " + AppInfo.VERSION;
                this.notifyIcon1.ShowBalloonTip(500); 
            }
        }
~~~


點選windows桌面右下角的icon兩下，則Form會顯示

~~~csharp
        private void notifyIcon1_DoubleClick(object sender, EventArgs e)
        {
            this.Visible = true;
            this.WindowState = FormWindowState.Normal;
            this.notifyIcon1.Visible = false; 
        }
~~~

這裡以下開始就是在設定在icon上按右鍵，分別相對應的功能

設定contextMenuStrip元件

![](..\images\2015-09-24-CsharpNotifyIconAndContextMenuStrip\5UmzYgn.png)


NotifyIcon裡，指定 contextMenuStrip

![](..\images\2015-09-24-CsharpNotifyIconAndContextMenuStrip\inFsmJn.png)

~~~csharp
        private void exitToolStripMenuItem_Click(object sender, EventArgs e)
        {
            Close();
        }

        private void hideToolStripMenuItem_Click(object sender, EventArgs e)
        {
           
                this.Hide();
                this.notifyIcon1.Visible = true;
                this.notifyIcon1.BalloonTipText = "Have a nice day!";
                this.notifyIcon1.BalloonTipTitle = AppInfo.PROGRAM + " " + AppInfo.VERSION;
                this.notifyIcon1.ShowBalloonTip(500);
            
        }

        private void showToolStripMenuItem_Click(object sender, EventArgs e)
        {
            //Show();
            this.Visible = true;
            this.WindowState = FormWindowState.Normal;
            this.notifyIcon1.Visible = false; 
        }
~~~


DEMO：

![](..\images\2015-09-24-CsharpNotifyIconAndContextMenuStrip\X6pJhhm.png)

![](..\images\2015-09-24-CsharpNotifyIconAndContextMenuStrip\XZDjbMp.png)


參考：

- http://ms-net.blogspot.tw/2008/04/notifyicon-contextmenustrip.html
- http://blog.xuite.net/merci0212/wretch/141175655-[%E8%BD%89%E8%BC%89]C# %E8%A3%BD%E4%BD%9Cwindows%E7%B8%AE%E5%B0%8F%E5%9C%96%E7%A4%BA%E5%B7%A5%E5%85%B7%EF%BC%8C%E4%BD%BF%E7%94%A8notifyicon(Trayicon)%E5%92%8Ccontextmenustrip
