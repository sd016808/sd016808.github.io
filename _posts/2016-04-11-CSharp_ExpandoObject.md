---
layout: post
title: 'C# 4.0 使用 ExpandoObject 類別'
author: 'James Peng'
tags: ['C# 4.0']
---

從 .NET Framework 4.0 開始 C# 支援 dynamic


你可以建立 System.Dynamic.ExpandoObject 物件，來可動態新增、移除成員。成員包含屬性、方法、事件等。



## 範例1. 動態建立一個ExpandoObject物件 ##

~~~csharp
            dynamic dynClass = new System.Dynamic.ExpandoObject();

            dynClass.id = 1;
            dynClass.Name = "James";
            dynClass.Blog = "http://jia-hong-peng.github.io";

            dynClass.Print = (Action)(() => {
                string str = string.Empty;
                str = dynClass.id.ToString() + ","+ dynClass.Name + ","+ dynClass.Blog;

                MessageBox.Show(str);
            });


            MessageBox.Show(dynClass.id.ToString());
            MessageBox.Show(dynClass.Name);
            MessageBox.Show(dynClass.Blog);            
            dynClass.Print();
~~~

![](..\images\2016-04-11-CSharp_ExpandoObject\N6Fq2nW.png)


----------


## 範例2. 移除其成員 ##

~~~csharp
 ((System.Collections.Generic.IDictionary<String, Object>)dynClass).Remove("Print");
~~~

![](..\images\2016-04-11-CSharp_ExpandoObject\NlLsxhZ.png)


----------


## 範例3. 擴充現有 Class ##

現有 Class

~~~csharp
        public class Person
        {
            public int id { get; set; }
            public string name { get; set; }
            public DateTime today { get; set; }
        }
~~~


----------

轉成可以擴充的物件的方法

ToDynamic(object obj) 會將原本的 物件，轉成 Key Value 型態後 回傳 ExpandoObject

~~~csharp
        /// <summary>
        /// 轉成可以擴充的物件
        /// </summary>
        /// <param name="obj"></param>
        /// <returns></returns>
        public static dynamic ToDynamic(object obj)
        {
            System.Collections.Generic.IDictionary<string, object> result = new System.Dynamic.ExpandoObject();

            foreach (System.ComponentModel.PropertyDescriptor pro in System.ComponentModel.TypeDescriptor.GetProperties(obj.GetType()))
            {
                result.Add(pro.Name, pro.GetValue(obj));
            }

            return result as System.Dynamic.ExpandoObject;
        }
~~~


----------


就可以對 原本的 物件的Property進行擴充 或 移除


~~~csharp

                Person p1 = new Person()
                {
                    id = 1,
                    name = "James",
                    today = DateTime.Today
                };
                dynamic exP1 = (System.Collections.Generic.IDictionary<string, object>)ToDynamic(p1);
               
                exP1.Blog = "http://jia-hong-peng.github.io/";
                exP1.Description = "安安你好";

                MessageBox.Show(exP1.name + exP1.Description + ", \n " + exP1.Blog);
~~~


![](..\images\2016-04-11-CSharp_ExpandoObject\itwisSr.png)

![](..\images\2016-04-11-CSharp_ExpandoObject\Jh22mdm.png)

----------


## 參考： ##

- http://msdn.microsoft.com/en-us/library/system.dynamic.expandoobject.aspx
- https://dotblogs.com.tw/junegoat/2012/09/07/c-sharp-expandoobject
- http://vmiv.blogspot.tw/2013/01/expandoobject.html
- http://no2don.blogspot.com/2013/02/c-property.html
