---
layout: post
title: 'C# TreeView KeyPress 如何調用 TreeView NodeMouseClick'
author: 'James Peng'
tags: ['WinForm']
---

![](..\images\2016-06-12-CSharp_TreeView_KeyPress_NodeMouseClick\1tTVvJG.png)

> 錯誤	655	引數 2: 無法從 'System.Windows.Forms.KeyEventArgs' 轉換為 'System.Windows.Forms.TreeNodeMouseClickEventArgs'	D:\Project\ProjTreeForm.cs	130	53	EXXBuilder
> 錯誤	654	最符合的多載方法 'EXXBuilder.ProjTreeForm.projTreeView_NodeMouseClick(object, System.Windows.Forms.TreeNodeMouseClickEventArgs)' 有一些無效的引數	D:\Project\ProjTreeForm.cs	130	17	EXXBuilder


~~~csharp
        private void projTreeView_KeyDown(object sender, KeyEventArgs e)
        {
            if (e.KeyCode == Keys.Enter)
                projTreeView_DoubleClick(sender, e);

            if (e.KeyCode == Keys.Delete)
            {
                if (projTreeView.SelectedNode == null)
                    return;

                projTreeView_NodeMouseClick(sender, new System.Windows.Forms.TreeNodeMouseClickEventArgs(projTreeView.SelectedNode, System.Windows.Forms.MouseButtons.Left, 1, 0, 0));
                deleteToolStripMenuItem_Click(sender, e);
            }
        }
~~~


----------

參考:
- http://bbs.csdn.net/topics/300216220
