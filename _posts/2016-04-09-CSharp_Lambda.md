---
layout: post
title: 'C# 3.0 Lambda 運算子的用法'
author: 'James Peng'
tags: ['C# 3.0']
---



## 一般 delegate 用法 ##

- 新增一個方法
- 宣告一個delegate

~~~csharp

        delegate int MyDelegate(int i, int j);

        static int Add(int i, int j)
        {
            return i + j;
        }


        private void button1_Click(object sender, EventArgs e)
        {
            MyDelegate del = new MyDelegate(Add);
            MessageBox.Show(del(10, 20).ToString());            
        }
~~~

![](..\images\2016-04-09-CSharp_Lambda\MUklFfc.png)


----------

## 使用匿名方法設計 delegate 用法 ##

- 宣告一個delegate
- 使用匿名方法，省去先宣告 Add()

~~~csharp
        delegate int MyDelegate(int i, int j);

        private void button1_Click(object sender, EventArgs e)
        {
            MyDelegate del = delegate(int x, int y)
            {
                return x + y;
            };
            MessageBox.Show(del(10, 20).ToString());            
        }
~~~

![](..\images\2016-04-09-CSharp_Lambda\MUklFfc.png)



----------

## 使用 delegate + Lambda 用法 ##

~~~csharp
        delegate int MyDelegate(int i, int j);

        private void button1_Click(object sender, EventArgs e)
        {
            MyDelegate del = (i, j) =>
            {
                return i + j;
            };
            MessageBox.Show(del(10, 20).ToString());            
        }
~~~

![](..\images\2016-04-09-CSharp_Lambda\MUklFfc.png)

----------


## 使用 Func<T1, T2, TResult> 委派 用法 ##

有回傳值得  Func 委派。

~~~csharp
        static int Add(int i, int j)
        {
            return i + j;
        }

        private void button1_Click(object sender, EventArgs e)
        {
            Func<int, int, int> del = Add;
            MessageBox.Show(del(10, 20).ToString());                                 
        }
~~~

![](..\images\2016-04-09-CSharp_Lambda\MUklFfc.png)

----------


## 使用 Func<T1, T2, TResult> 委派+Lambda 用法 ##

有回傳值得  Func 委派+Lambda 。

~~~csharp
        private void button1_Click(object sender, EventArgs e)
        {
            Func<int,int,int> del = (x, y) => { return x + y; };
            MessageBox.Show(del(10, 20).ToString());            
        }
~~~

![](..\images\2016-04-09-CSharp_Lambda\MUklFfc.png)


----------

## 使用 Action<T1, T2> 委派 用法 ##

Action 為 沒有參數沒有回傳值得委派

~~~csharp
        static void Add(int i, int j)
        {            
            MessageBox.Show((i + j).ToString());
        }

        private void button1_Click(object sender, EventArgs e)
        {
            Action<int, int> del = Add;
            del(10,20);                             
        }
~~~

![](..\images\2016-04-09-CSharp_Lambda\MUklFfc.png)

----------

## 使用 Action<T1, T2> 委派+Lambda 用法 ##

Action 為 沒有參數沒有回傳值得委派+Lambda

~~~csharp
        private void button1_Click(object sender, EventArgs e)
        {            
            Action<int, int> del = (x, y) => { MessageBox.Show((x + y).ToString()); };

            del(10,20);
                     
        }
~~~

![](..\images\2016-04-09-CSharp_Lambda\MUklFfc.png)

----------

參考：

- http://ms-net.blogspot.tw/search/label/C%23%E7%AF%87
- https://msdn.microsoft.com/zh-tw/library/bb549311(v=VS.90).aspx
- https://msdn.microsoft.com/zh-tw/library/bb534647(v=VS.90).aspx

