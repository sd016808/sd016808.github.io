---
layout: post
title: 'C# DevExpress TreeList 的 Checkbox 加入按下 Shift 反向多選功能'
author: 'James Peng'
tags: ['DevExpress']
---

## 預期效果 ##

![](http://i.imgur.com/Sa0MkVN.gif)

可以 Shift 多選後，子項目可以多選功能後，但沒考慮到 反向多選

當我們使用 Shift 多選後，想改變選取範圍，反向多選功能

承 上篇及 上上篇 

- [C# DevExpress TreeList Checkbox 加入按下 Shift 多選功能]
- [C# DevExpress TreeList 的 ChildNode Checkbox 加入按下 Shift 多選功能]

----------

## 參考操作邏輯 ##

參考 Windows檔案總管 選取檔案時的使用者經驗

![](http://i.imgur.com/kQlko8Y.gif)


----------

## 實作方法 ##

分幾個步驟：

## 宣告 ##

~~~csharp
        protected TreeListNode m_StartNode;
        protected TreeListNode m_EndNode;
        protected TreeListNode m_CancelNode;        
        bool bWhenExpandDisableClick = false;
~~~


----------

## treeList 事件 FocusedNodeChanged ##

這裡用來 存三個 Node：

1. 滑鼠第 1 次點選後 記錄為啟始點 「m_StartNode」 和 「m_EndNode」 都記錄為此點。
2. 滑鼠按下 「Shift 鍵」，滑鼠點選後，把 「m_EndNode」 丟到 「m_CancelNode」 ，然後  「m_EndNode」 存入 當前 node
3. 目的是 沒按 「Shift 鍵」 為起點，有按 「Shift 鍵」 為終點，然後會記錄一個叫做 「m_CancelNode」 的為 上次點選的終點。
4. 每次多選 就是：如果上次有選過 先取消(反向) 起點到終點 之間的結果 ，再跑一次  從起點到終點 勾選(反向)。

~~~csharp
        private void treeList_Parameter_FocusedNodeChanged(object sender, FocusedNodeChangedEventArgs e)
        {
            Debug.WriteLine("treeList_Parameter_FocusedNodeChanged");
            
            if (m_StartNode == null)
            {
                m_EndNode = e.Node;
                m_StartNode = e.Node;
                m_CancelNode = e.Node;
            }

            if (Control.ModifierKeys != Keys.Shift)
            {
                m_EndNode = e.Node;
                m_StartNode = e.Node;
            }
            else if(Control.ModifierKeys == Keys.Shift)
            {
                m_CancelNode = m_EndNode;
            }            
            m_EndNode = e.Node;

            Debug.WriteLine("m_CancelNode=" + m_CancelNode.Id);
            Debug.WriteLine("m_StartNode=" + m_StartNode.Id);
            Debug.WriteLine("m_EndNode=" + m_EndNode.Id);
        }
~~~

----------

## treeList 事件 Click ##

- 當展開縮起時，不要觸發（勾選時才觸發）
- 如果沒按 Shift ，才去勾選 node (有按的話不觸發，以免影響 Shift 方法)
- 如果 m_CancelNode 有得取消 就取消
- 做一次正常的 起點 到 終點 勾選

~~~csharp
        private void treeList_Parameter_Click(object sender, EventArgs e)
        {
            Debug.WriteLine("treeList_Parameter_Click");

            if (!bWhenExpandDisableClick)
            {
                TreeListNode node = treeList_Parameter.FocusedNode;

                if (Control.ModifierKeys != Keys.Shift)
                {
                    if (node.Checked)
                    {
                        node.Checked = false;
                    }
                    else
                    {
                        node.Checked = true;
                    }
                }

                if (m_CancelNode.Id != m_EndNode.Id)
                {
                    shiftFuntion(m_StartNode.Id, m_CancelNode.Id);
                }                
                shiftFuntion(m_StartNode.Id, m_EndNode.Id);

            }
            else
            {
                bWhenExpandDisableClick = false;
            }

        }
~~~

----------

## 函數 shiftFuntion ##

- 帶入兩個點
- 他會分為 由上往下選，由下往上選
- 起點不動作
- 其他點，Checked 會反向，應觀眾要求 折起來的 裡面不選， 展開的 裡面選


~~~csharp

        /// <summary>
        /// Press Shift key to checked multiple nded
        /// </summary>
        private void shiftFuntion(int iStart, int iEnd)
        {
            Debug.WriteLine("shiftFuntion");
            //press shift key
            if (Control.ModifierKeys == Keys.Shift)
            {
                //var all = treeList_Parameter.GetAllCheckedNodes();
                //if (all.Count >= 2)
                //{
                if (iStart > iEnd) // down ->top 
                {
                    Debug.WriteLine("=====================================");
                    Debug.WriteLine("down ->top ");
                    for (int i = iStart; i >= iEnd; i--)
                    {
                        if (i == iStart) //start node
                        {
                            Debug.WriteLine("down ->top >>> start node");
                            Debug.WriteLine("node.id =" + i);
                            Debug.WriteLine("nodePrv.id =" + (i - 1));
                        }                        
                        else
                        {
                            Debug.WriteLine("node.id =" + i);
                            var node = treeList_Parameter.FindNodeByID(i);
                            if (node.Level == 1 && node.ParentNode.Expanded)
                            {
                                node.Checked = !node.Checked;
                            }
                            else if (node.Level == 0)
                            {
                                node.Checked = !node.Checked;
                            }
                            Debug.WriteLine("node.Checked =" + node.Checked);
                        }
                    }
                }
                else if (iStart < iEnd) // top ->down
                {
                    Debug.WriteLine("=====================================");
                    Debug.WriteLine("top ->down ");

                    for (int i = iStart; i <= iEnd; i++)
                    {
                        if (i == iStart) //start node
                        {
                            Debug.WriteLine("top ->down >>> start node ");
                            Debug.WriteLine("node.id =" + i);
                            Debug.WriteLine("nodePrv.id =" + (i + 1));
                        }                        
                        else
                        {
                            Debug.WriteLine("node.id =" + i);
                            var node = treeList_Parameter.FindNodeByID(i);
                            if (node.Level == 1 && node.ParentNode.Expanded)
                            {
                                node.Checked = !node.Checked;
                            }
                            else if (node.Level == 0)
                            {
                                node.Checked = !node.Checked;
                            }
                            Debug.WriteLine("node.Checked =" + node.Checked);
                        }
                    }
                }

            }
        }
~~~

----------

## treeList 事件 展開 收起 ##

用來修正 bug，因為點選 展開收起時，會觸方 勾選 動作。

~~~csharp
        private void treeList_Parameter_AfterExpand(object sender, NodeEventArgs e)
        {
            bWhenExpandDisableClick = true;            
        }
        private void treeList_Parameter_AfterCollapse(object sender, NodeEventArgs e)
        {
            bWhenExpandDisableClick = true;            
        }
~~~



----------

## 參考： ##

- http://ezgif.com/video-to-gif
