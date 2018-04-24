---
layout: post
title: 'C# 使用 Generic Delegate'
author: 'James Peng'
tags: ['C#']
---

用delegate時可以改用generic delegate(Action/Func)

使用上更直觀也更簡潔,不需先宣告就可使用

## 原來的寫法 ##

~~~csharp
public delegate void myDelegate(); // 使用前要先宣告

private void button1_Click(object sender, EventArgs e)
{
    myDelegate myd = new myDelegate(PopMsg);
    myd += new myDelegate(PopMsg1);
    myd.Invoke();
}

private void PopMsg()
{
    MessageBox.Show("hello");
}
private void PopMsg1()
{
    MessageBox.Show("hello my friend");
}

~~~

----------

## 使用 generic delegate ##

~~~csharp
private void button1_Click(object sender, EventArgs e)
{
    Action d1 = PopMsg; // 直接使用
    d1 += PopMsg1;
    d1();
}
~~~


----------


若是delegate所呼叫的method很簡短可直接使用 generic delegate + Lamda expression:

## generic delegate + Lamda expression: ##

~~~csharp
private void button1_Click(object sender, EventArgs e)
{
    Action d1 = () => { MessageBox.Show("hello"); };// 直接使用,且不需要多訂一個method
    d1 += () => { MessageBox.Show("hello my friend"); };
    d1();
} 
~~~

