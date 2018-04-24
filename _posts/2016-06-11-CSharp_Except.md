---
layout: post
title: 'C# Except'
author: 'James Peng'
tags: ['C#']
---

## 題目 ##

1. 請實作一個靜態方法 GetTerminateNodeList(), 找出 myTree 中所有的最終節點的id, 回傳型態 List＜int＞


~~~csharp
using System.Collections.Generic;
using System.Linq;

namespace ConsoleApplication
{
    public class Node
    {
        public int id = -1; //唯一Key
        public int PreviousNodeId = -1; //紀錄前一個Node id
        public string Name = string.Empty; //節點名稱
    }

    public class Program
    {
        static void Main(string[] args)
        {
            List<Node> myTree = new List<Node>
            {
                new Node() { id = 0, PreviousNodeId = -1 , Name = "Project Root" },
                new Node() { id = 1, PreviousNodeId = 0 , Name = "Master_1" },
                new Node() { id = 2, PreviousNodeId = 0 , Name = "Master_2" },
                new Node() { id = 3, PreviousNodeId = 0 , Name = "Master_3" },
                new Node() { id = 4, PreviousNodeId = 1 , Name = "Slave_1" },
                new Node() { id = 5, PreviousNodeId = 1 , Name = "Slave_2" },
                new Node() { id = 6, PreviousNodeId = 2 , Name = "Slave_3" },
                new Node() { id = 7, PreviousNodeId = 3 , Name = "Slave_4" },
                new Node() { id = 8, PreviousNodeId = 4 , Name = "Sub_Slave_1" },
                new Node() { id = 9, PreviousNodeId = 5 , Name = "Sub_Slave_2" },
                new Node() { id = 10, PreviousNodeId = 7 , Name = "Sub_Slave_3" },
            };

            List<int> terminateNodeList = GetTerminateNodeList(myTree);
        }
    }
}

~~~


----------

## 答案1 ##

![](..\images\2016-06-11-CSharp_Except\LacqrlC.png)

## 使用LINQ ##

~~~csharp                          
        public static List<int> GetTerminateNodeList(List<Node> nodeList)
        {
            var allNodeId = (from node in nodeList
                             select node.id);

            var allPreviousNodeId = (from node in nodeList
                                     select node.PreviousNodeId);

            return allNodeId.Except(allPreviousNodeId).ToList();
        }
~~~

## 不用LINQ 使用兩個FOR 也可以 ##

~~~csharp
        public static List<int> GetTerminateNodeList(List<Node> nodeList)
        {
            List<int> result = new List<int>();
            foreach (var node in nodeList)
            {
                bool foundPrevId = false;
                foreach (var nodePid in nodeList)
                {
                    if (node.id == nodePid.PreviousNodeId)
                        foundPrevId = true;
                }
                if (!foundPrevId)
                    result.Add(node.id);
            }
            return result;
        }
~~~


----------

## 題目 ##

2. 請使用遞迴(recursive)來實作一個靜態方法 RemoveNode(int nodeId)，在 myTree 中刪除 Master_3 並把底下所有關連節點一併刪除, 回傳沒被刪除的 List＜Node＞。

## 答案2 ##

~~~csharp
            public static List<Node> RemoveNode(List<Node> nodeList, int nodeId)
            {
                List<Node> result = nodeList;
                var node = nodeList.Find(x => x.id == nodeId);

                var connectedList = (from n in nodeList
                                    where n.PreviousNodeId == node.id
                                    select n).ToList();

                if (connectedList.Count != 0)
                {
                    foreach (var c in connectedList)
                    {
                        result = RemoveNode(nodeList, c.id);
                        result.Remove(c);
                    }
                }
                result.Remove(node);
                return result;
            }
~~~
