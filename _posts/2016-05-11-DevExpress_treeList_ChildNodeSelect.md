---
layout: post
title: 'C# DevExpress TreeList 的 ChildNode Checkbox 加入按下 Shift 多選功能'
author: 'James Peng'
tags: ['DevExpress']
---

![](..\images\2016-05-11-CSharp_treeList_ChildNodeSelect\wCkvFBv.png)

承上篇 [C# DevExpress TreeList Checkbox 加入按下 Shift 多選功能]

可以 Shift 多選後，但當有 子項目時 卻沒考慮到，按下 Shift 多選功能


----------


## 展開/合起 子項目時 ##

展開時要選取，但不能讓項目打勾

![](..\images\2016-05-11-CSharp_treeList_ChildNodeSelect\MvycXEP.png)

- 加入變數 bDisableExpand

~~~csharp
        private void treeList_Parameter_AfterExpand(object sender, NodeEventArgs e)
        {
            bDisableExpand = true;
            e.Node.Selected = true;
        }

        private void treeList_Parameter_AfterCollapse(object sender, NodeEventArgs e)
        {
            bDisableExpand = true;
            e.Node.Selected = true;
        }

        private void treeList_Parameter_Click(object sender, EventArgs e)
        {
            if (!bDisableExpand)
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

                string indexInfo = node.GetValue(CURR_INDEX).ToString();
                uint index = 0;
                uint subIndex = 0;
	            if (node.Tag != null && node.Tag.ToString() != "root")
	            {
                    index = Convert.ToUInt32(indexInfo, 16);
                    subIndex = 0;
                }
                else
                {
                    string curr_subitem_idx = node.GetValue(CURR_SUBITEM_IDX).ToString();
                    if (!string.IsNullOrEmpty(curr_subitem_idx))
                    {
                        index = Convert.ToUInt32(curr_subitem_idx, 16);
                        subIndex = Convert.ToUInt32(indexInfo, 16);
                    }
                }
            }
            else
            {
                bDisableExpand = false;
            }
            
        }


        private void treeList_Parameter_Click(object sender, EventArgs e)
        {
            if (!bDisableExpand)
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
                //CheckedChildNodes(node, node.Checked);
                shiftFuntion();

                string indexInfo = node.GetValue(CURR_INDEX).ToString();
                uint index = 0;
                uint subIndex = 0;
	            if (node.Tag != null && node.Tag.ToString() != "root")
	            {
                    index = Convert.ToUInt32(indexInfo, 16);
                    subIndex = 0;
                }
                else
                {
                    string curr_subitem_idx = node.GetValue(CURR_SUBITEM_IDX).ToString();
                    if (!string.IsNullOrEmpty(curr_subitem_idx))
                    {
                        index = Convert.ToUInt32(curr_subitem_idx, 16);
                        subIndex = Convert.ToUInt32(indexInfo, 16);
                    }
                }
            }
            else
            {
                bDisableExpand = false;
            }
            
        }
~~~


----------


## ChildNode Checkbox 加入按下 Shift 多選功能 ##

![](..\images\2016-05-11-CSharp_treeList_ChildNodeSelect\HHuQb9x.png)

- 判斷 Node.Level 處理 子項目
- 加入函式 CheckedChildNodes()

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
                    if (m_lastNode.Level == 0 && m_nowNode.Level == 0)
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
                                    CheckedChildNodes(treeList_Parameter.Nodes[j], true);
                                }
                            }
                            else if (iFirstNodeIndex < iLastNodeIndex)
                            {
                                int j = iFirstNodeIndex;
                                for (; j <= iLastNodeIndex; j++)
                                {
                                    treeList_Parameter.Nodes[j].Checked = true;
                                    CheckedChildNodes(treeList_Parameter.Nodes[j], true);
                                }
                            }
                        }
                    }
                    else if (m_lastNode.Level == 1 && m_nowNode.Level == 1)
                    {
                        if (m_lastNode.Id >= 0 && m_nowNode.Id >= 0)
                        {                            
                            if (m_lastNode.Id < m_nowNode.Id)
                            {
                                int j = m_lastNode.Id;
                                for (; j <= m_nowNode.Id; j++)
                                {
                                    TreeListNode myNode = treeList_Parameter.FindNodeByID(j);
                                    if (myNode != null) myNode.Checked = true;
                                }
                            }
                            else if (m_nowNode.Id < m_lastNode.Id)
                            {
                                int j = m_nowNode.Id;
                                for (; j <= m_lastNode.Id; j++)
                                {
                                    TreeListNode myNode = treeList_Parameter.FindNodeByID(j);
                                    if (myNode != null) myNode.Checked = true;                                    
                                }
                            }
                        }               
                    }
                }

            }
        }

        private void treeList_Parameter_FocusedNodeChanged(object sender, FocusedNodeChangedEventArgs e)
        {           
            m_lastNode = m_nowNode;
            m_nowNode = e.Node;
        }

        private void CheckedChildNodes(TreeListNode node , bool checkedstat)
        {
            foreach (TreeListNode childNode in node.Nodes)
            {
                childNode.Checked = checkedstat;
                if (childNode.HasChildren)
                    CheckedChildNodes(childNode, checkedstat);
            }
        }

~~~
