---
layout: post
title: 'C# DevExpress TreeList Checkbox 加入按下 Shift 多選功能'
author: 'James Peng'
tags: ['DevExpress']
---

##  C# DevExpress TreeList Checkbox 加入按下 Shift 多選功能  ##

![](..\images\2016-05-05-CSharp_treeList_Shift\uIstocv.png)


當點選 Row 時 ，會選取 Checkbox

~~~csharp
        private void treeList_Parameter_Click(object sender, EventArgs e)
        {
            
            TreeListNode node = treeList_Parameter.FocusedNode;
            if (node.Checked)
            {
                node.Checked = false;                
            }
            else
            {
                node.Checked = true;                                
            }
            shiftFuntion();
        }
~~~


----------


## 禁用 TreeListNode CheckBox ##

![](..\images\2016-05-05-CSharp_treeList_Shift\PbGnt81.png)

點選 CheckBox 時，實際上要選取整行， 所以 e.Node.Selected = true

然後 選取整行 時，已經會觸發 treeList_Parameter_Click(object sender, EventArgs e) 的 Checked ，

所以 把 Checkbox 禁用掉 e.CanCheck = false

~~~csharp
        private void treeList_Parameter_BeforeCheckNode(object sender, CheckNodeEventArgs e)
        {
            e.Node.Selected = true;
            e.CanCheck = false;
            
        }
~~~


----------

## 當選取 Focused 到 node 時 ##

把當前 node 記錄起來，以及前一次選取的 node 記錄起來

~~~csharp
        private void treeList_Parameter_FocusedNodeChanged(object sender, FocusedNodeChangedEventArgs e)
        {
            m_lastNode = m_nowNode;
            m_nowNode = e.Node;
        }
~~~


----------

## shift Funtion ##

找出兩個 node 之間的node index，然後 全部勾選。


~~~csharp

        /// <summary>
        /// Press Shift key to checked multiple nded
        /// </summary>
        private void shiftFuntion()
        {

            if (Control.ModifierKeys == Keys.Shift)
            {
                var all = treeList_Parameter.GetAllCheckedNodes();
                if (all.Count >= 2)
                {

                    int iLastNodeIndex = -1;
                    int iFirstNodeIndex = -1;
                    int i = 0;
                    foreach (TreeListNode mynode in treeList_Parameter.Nodes)
                    {
                        if (mynode == m_lastNode) iLastNodeIndex = i;
                        else if (mynode == m_nowNode) iFirstNodeIndex = i;

                        i++;
                    }

                    if (iLastNodeIndex >= 0 && iFirstNodeIndex >= 0)
                    {
                        if (iLastNodeIndex < iFirstNodeIndex)
                        {
                            int j = iLastNodeIndex;
                            for (; j <= iFirstNodeIndex; j++)
                            {
                                treeList_Parameter.Nodes[j].Checked = true;
                            }
                        }
                        else if (iFirstNodeIndex < iLastNodeIndex)
                        {
                            int j = iFirstNodeIndex;
                            for (; j <= iLastNodeIndex; j++)
                            {
                               treeList_Parameter.Nodes[j].Checked = true;
                            }
                        }

                    }
                }

            }   
        }
~~~




----------

參考：

- http://www.cnblogs.com/Yan-Zhiwei/p/3833613.html
