---
layout: post
title: 'C# 讀取 XML 到 TreeView'
author: 'James Peng'
tags: ['WinForm']
---

## 讀取XML ##
~~~csharp
XElement xdoc = XElement.Load(xmlPath);
~~~
----------


## 取得 XML 標籤名稱 ##
~~~csharp
  foreach (XElement eleClass in xdoc.Elements())
  {	
	string strName = eleClass.Name.ToString();
  }
~~~

----------


## 取得 XML 屬性值 ##
~~~csharp
  foreach (XElement eleClass in xdoc.Elements())
  {
	string className = eleClass.Attribute("ClassName").Value;	
  }
~~~


----------

## 把 XML讀取到的值 加入 TreeView   ##

![](..\images\2016-01-04-CSharp_TreeView\WTwtAyr.png)

~~~csharp
using System;
using System.Windows.Forms;

namespace WindowsFormsApplication1
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            AddNodeToTreeView("root", "Dir1", "Node1", "", "");
            AddNodeToTreeView("root", "Dir1", "Node2", "", "");
            AddNodeToTreeView("root", "Dir1", "Node3", "", "");
            AddNodeToTreeView("root", "Dir1", "Node4", "", "");

            AddNodeToTreeView("root", "Dir2", "Node1", "", "");
            AddNodeToTreeView("root", "Dir2", "Node2", "", "");
            AddNodeToTreeView("root", "Dir2", "Node3", "", "");
            AddNodeToTreeView("root", "Dir2", "Node4", "", "");
        }

        static int ICON_UNKNOW = 2;
        static int ICON_FOLDER = 0;
        static int ICON_VENDER = 3;
        static int ICON_TYPE = 1;

        private void AddNodeToTreeView(string nodeRoot, string nodeType, string nodeName, string iconStr, string filePath)
        {
            TreeNode prodNode = null;
            TreeNode funcNode = null;

            if (treeView1.Nodes.ContainsKey(nodeRoot))
            {
                int indexProdType = treeView1.Nodes.IndexOfKey(nodeRoot);

                TreeNode node = treeView1.Nodes[indexProdType];


                if (node.Nodes.ContainsKey(nodeType))
                {
                    indexProdType = node.Nodes.IndexOfKey(nodeType);


                    node = node.Nodes[indexProdType];
                    if (iconStr != "")
                    {
                        prodNode = node.Nodes.Add(nodeName, nodeName, iconStr, iconStr);
                    }
                    else
                    {
                        prodNode = node.Nodes.Add(nodeName, nodeName, ICON_UNKNOW, ICON_UNKNOW);
                    }
                }
                else
                {

                    funcNode = node.Nodes.Add(nodeType, nodeType, ICON_TYPE, ICON_TYPE);

                    AddNodeToTreeView(nodeRoot, nodeType, nodeName, iconStr, filePath);
                }



            }
            else
            {
                TreeNode node = treeView1.Nodes.Add(nodeRoot, nodeRoot, ICON_TYPE, ICON_TYPE);

                AddNodeToTreeView(nodeRoot, nodeType, nodeName, iconStr, filePath);
            }

            if (prodNode != null)
            {
                prodNode.Tag = filePath;
            }
        }
    }
}

~~~

