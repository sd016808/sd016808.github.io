---
layout: post
title: 'C# 取得 Namespace 下所有 Class 名稱 以及 Method 資訊'
author: 'James Peng'
tags: ['C#']
---

## C# 可以藉由傳入class名稱 宣告List<class名稱>嗎? ##

如題

我想寫一個功能是使用者選擇class名稱按下新增後

會 new List<所選class>

目前用switch暴力解

switch(Name) 

case"a" : List<a> X=new List<a>();

case"b" : List<b> X=new List<b>();

這種方式

因為class太多想請問有沒有其他方式可以做出類似的功能


已編輯 KEN155015 2016年4月18日 上午 05:13

來源：[msdn論壇](https://social.msdn.microsoft.com/Forums/zh-TW/de4d817c-1ecc-4e5a-8886-4478760679b1/c-class-listclass?forum=233)


----------

## 解法 ##

因為 class 太多 不可能自己手動輸入進去，這樣寫死的也不好維護

這裡透過利用 System.Reflection 來取得 Namespace下的 Class 名稱

----------


## 先加入命名空間 ##

~~~csharp
using System.Reflection;
~~~


----------

## 自訂取得類別的方法 GetAllType ##

~~~csharp

        private Type[] GetAllType(string mynamespace)
        {
            Type[] types = null;
            try
            {
                types = Assembly.Load(mynamespace).GetTypes();            
            }
            catch
            {

            }
            return types;
        }


        private MethodInfo[] GetAllMethod(string mynamespace,string className)
        {
            MethodInfo[] methods = null;
            try
            {
                Type[] types = Assembly.Load(mynamespace).GetTypes();

                for (int i = 0; i < types.Length; i++)
                {
                    if (types[i].Name.Equals(className))
                    {
                        methods = types[i].GetMethods();                        
                    }
                }
            }
            catch
            {

            }
            return methods;
        }
~~~


----------

## 範例 class ##

~~~csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace WindowsFormsApplication3
{
    class Class1
    {
        public bool Method1()
        { 
            return false; 
        }
        public void Method2() { }
        public static void Method3() { }
        public void Method4() { }
        public void Method5() { }
        public void Method6() { }
    }

    class Class2
    {
    }

    class Class3
    {
    }

    class Class4
    {
    }

    class Class5
    {
    }

    class Class6
    {
    }
}

~~~

----------


## 用法 ##

~~~csharp
        private void button1_Click(object sender, EventArgs e)
        {

            Type[] types = GetAllType("WindowsFormsApplication3");
            foreach (Type t in types)
            {
                MessageBox.Show(t.Name);
            }



            MethodInfo[] methods = GetAllMethod("WindowsFormsApplication3", "Class1");
            foreach (MethodInfo m in methods)
            {
                MessageBox.Show( "方法名=" + m.Name + " , 回傳=" + m.ReturnType + " , 是否靜態=" + m.IsStatic );
            }

        }
~~~

![](..\images\2016-04-21-CSharp_GetClassName\ggSmkIs.png)

![](..\images\2016-04-21-CSharp_GetClassName\rkvbAJU.png)

----------

參考：

- https://social.msdn.microsoft.com/Forums/zh-TW/de4d817c-1ecc-4e5a-8886-4478760679b1/c-class-listclass?forum=233
- https://msdn.microsoft.com/en-us/library/system.reflection.assembly.load%28v=vs.110%29.aspx?f=255&MSPPError=-2147217396
- http://www.lai18.com/content/2496907.html
