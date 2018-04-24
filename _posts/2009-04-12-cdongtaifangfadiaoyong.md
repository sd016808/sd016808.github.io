---
layout: post
title: 'C#動態方法調用'
author: 'James Peng'
tags: ['C#']
---

**Dynamic Method Invocation**  
One very useful feature related to Reflection is the ability to create
objects dynamically and call methods on them.  

note : 
Class1.cs has methods which will be dynamically invoked at
runtime from the DynaInvoke.cs  

## Class1.cs ##  
~~~cs
using System;  
class Class1{  
       public static String method1()  
       {  
           return "I am Static method (method1) in class1";  
       }  
       public String method2()  
       {  
           return "I am a Instance Method (method2) in Class1";  
       }  
       public String method3(String s)  
       {  
          return "Hello " + s;  
       }  
}  
~~~
save this file as Class1.cs and Compile c:/\>csc /t:library Class1.cs  

## DynaInvoke.cs ##  
~~~cs
using System;  
using System.Reflection;  
class DynamicInvoke  
{  
public static void Main(String [] args)  
{  
String path = "Class1.dll"  
Assembly a = Assembly.Load(path);  
//Invoking a static method  
Type mm = a.GetType("Class1");  
String i = (String) mm.InvokeMember ("method1",BindingFlags.Default |
BindingFlags.InvokeMethod,null,null,new object [] {});
Console.WriteLine(i);  
//Invoking a non-static method  
object o = Activator.CreateInstance(mm);  
i = (String) mm.InvokeMember("method2",BindingFlags.Default |
BindingFlags.InvokeMethod,null,o,new object [] {});  
Console.WriteLine(i);  
//Invoking a non-static method with parameters  
object [] par = new object[] {"kunal"};  
i = (String) mm.InvokeMember("method3",BindingFlags.Default |
BindingFlags.InvokeMethod,null,o,par);  
Console.WriteLine(i);  
}  
}  
~~~
save this file as DynaInvoke.cs and Compile c:/\>csc DynaInvoke.cs 

and
run C:\\\>DynaInvoke

