---
layout: post
title: 'Effective C#： (1) 使用 Properties 屬性 代替 Public Field 公有欄位 (Accessible Data Members)'
author: 'James Peng'
tags: ['Effective C#']
---


## Effective C# 條款一 item1 ##

第一條建議是

> 用 屬性(Properties) 來替代 公有欄位(Accessible Data Members)


原因：

1. 符合物件導向封裝概念
2. 支援資料繫結
3. 具修改彈性

----------

# 1.符合物件導向封裝概念 #

屬性(Properties) 是對 Get讀取/Set修改 內部 Value 的一種方法的擴展，從表面看來就像是 資料成員(Data Members)，但內部卻是以 方法實現。

在程式編譯後，編譯器在把程式編譯成MSIL時，會自動把屬性中的get區塊與set區塊編譯成兩個分離的方法。

所以使用屬性能穫得函式的全部好處!!!!

## 透過 Ildasm.exe (IL Disassembler) 工具查看 MSIL ##

IL 反組譯工具是 IL 組譯工具 (Ilasm.exe) 的附屬工具。 Ildasm.exe 會使用一個包含中繼語言 (IL) 程式碼的可攜式執行檔 (PE)，並建立適合輸入至 Ilasm.exe 的文字檔。

此工具會自動與 Visual Studio 一起安裝。 若要執行此工具，請使用 [開發人員命令提示字元] (或 Windows 7 中的 [Visual Studio 命令提示字元])。

![](..\images\2016-06-03-EffectiveCSharp_item1\yNh41hL.png)

![](..\images\2016-06-03-EffectiveCSharp_item1\TxtrR2z.png)

----------

## 屬性(Property) 與 欄位(Field) ##

Field 與 Property 都是屬於物件的資料成員，明明兩種都是可以去儲存以及取得物件中的資料，但是到底兩者有什麼差別呢？

雖然欄位和屬性從用戶端應用程式的觀點來看並無分別，但它們在類別中的宣告方式卻不一樣。

----------


## 欄位(Field) ##

欄位只是由類別 (Class) 公開 (Expose) 的公用變數，

宣告如下:

**TestClass.cs**

~~~csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace EffectiveCSharp_Item1
{
    public class TestClass
    {
        public string NameField;
    }
}

~~~

可以看出 MSIL 裡就這個變數就是一個 Field

![](..\images\2016-06-03-EffectiveCSharp_item1\q0ZaPSC.png)

![](..\images\2016-06-03-EffectiveCSharp_item1\a5n81TM.png)

我可以在外部存取 NameField 這個公開的成員 (Accessible Data Members)

使用方式：

**Form1.cs**

~~~csharp
using System;
using System.Windows.Forms;

namespace EffectiveCSharp_Item1
{
    public partial class Form1 : Form
    {        
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            TestClass tc = new TestClass();
            tc.NameField = "111111111";            
        }
    }
}
~~~

![](..\images\2016-06-03-EffectiveCSharp_item1\uZ6jNyg.png)


在物件導向裡，Field 通常的修飾都是為private（私有），只有開放給物件內部使用，對外則不開放的，

除非是定義為一個常數(const)或是固定的變數(readonly)，我們才會將此 Field 修飾為 Public(公開)，

否則一旦將 Field 的修飾設為 Public 而且不對其存取加以限制，那麼只要使用此物件時都可以任意的修改這個 Field，這就失去了物件封裝的意義，也危及資料的安全。



----------

## 屬性(Property) ##

屬性 (Property) 是欄位和方法的綜合體，是一個方法或一對方法，

如果是物件的使用者，屬性會以欄位出現，和存取屬性一樣方式。

而 MSIL 內部實作時，屬性則是一或兩個 方法，一個代表 get 存取子，一個代表 set 存取子，同時具有這兩種存取子的屬性則為可讀寫。

- get：用來讀取屬性，不含 get 存取子的屬性會被視為唯寫。
- set：用來寫入新值給該屬性，不含 set 存取子的屬性會被視為唯讀。

屬性並不會歸類為變數，這點與欄位不同。因此，不可能將屬性當做 ref (C# 參考) 或 out (C# 參考) 參數來傳遞。

宣告如下:

**TestClass.cs**

~~~csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace EffectiveCSharp_Item1
{
    public class TestClass
    {
        public string NameProperty
        {
            get;
            set;
        }
    }
}

~~~

可以看 MSIL 裡

![](..\images\2016-06-03-EffectiveCSharp_item1\6hpqA9m.png)

產生出 兩個方法 get_NameProperty() 和 set_NameProperty(string)

![](..\images\2016-06-03-EffectiveCSharp_item1\yjI1JfH.png)

![](..\images\2016-06-03-EffectiveCSharp_item1\qlEuFzT.png)

Set裡 會用到一個 private 的 Field

![](..\images\2016-06-03-EffectiveCSharp_item1\e1F1x7C.png)


這些東西 合起來 是一個 Property

![](..\images\2016-06-03-EffectiveCSharp_item1\h0J6lZr.png)


使用方式：

**Form1.cs**

~~~csharp
using System;
using System.Windows.Forms;

namespace EffectiveCSharp_Item1
{
    public partial class Form1 : Form
    {        
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            TestClass tc = new TestClass();            
            tc.NameProperty = "111111111";
        }
    }
}
~~~

MSIL 裡面其實是 Call 方法 set_NameProperty(string)

![](..\images\2016-06-03-EffectiveCSharp_item1\pg4VGnX.png)

----------

# 2.支援資料繫結 #

NET中的資料繫結 (Data Binding) 只支援屬性，並不支援公有欄位。

這是因為資料繫結機制用反射(reflection)來實作時只尋找類別的屬性。

而之所以不支援公有欄位，主要是因為公有欄位把數據成員直接鋪暴露給外界較不符合物件導向的封裝原則。


Data Binding 使用 Property ：


**TestClass.cs**

~~~csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace EffectiveCSharp_Item1
{
    public class TestClass
    {
        public string NameField;
        public string NameProperty
        {
            get;
            set;
        }
    }
}
~~~


**Form1.cs**

~~~csharp
using System;
using System.Windows.Forms;

namespace EffectiveCSharp_Item1
{
    public partial class Form1 : Form
    {        
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            TestClass tc = new TestClass();
            tc.NameProperty = "111111111";
            textBox1.DataBindings.Add("Text", tc, "NameProperty");
        }
    }
}
~~~

結果正常：

![](..\images\2016-06-03-EffectiveCSharp_item1\c9Q8bJC.png)

但

Data Binding 使用 Field ：

**Form1.cs**

~~~csharp
using System;
using System.Windows.Forms;

namespace EffectiveCSharp_Item1
{
    public partial class Form1 : Form
    {        
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            TestClass tc = new TestClass();
            tc.NameField = "111111111";
            textBox1.DataBindings.Add("Text", tc, "NameField");
        }
    }
}
~~~

可以編譯，但執行時會出現錯誤訊息 「無法繫結至 DataSource 上的屬性或欄位 NameField。」

![](..\images\2016-06-03-EffectiveCSharp_item1\nwiNbDV.png)

----------

# 3.具修改彈性 #

使用屬性在修改上也較一般公有欄位更具彈性。


## 情況一：增加判斷防止公有欄位值為空時 ##

試想當程式中類別的公有欄位被頻繁的使用，則當我們要增加判斷防止公有欄位值為空時，我們勢必需要把散在程式中所有用到的地方都加上判斷才行。

同樣的情況對於屬性來說，我們只需在屬性的get區塊增加判斷即可。


**TestClass.cs**

~~~csharp
namespace EffectiveCSharp_Item1
{
    public class TestClass
    {
        private string _nameProperty;
        public string NameField;
        public string NameProperty
        {
            get
            {
                if (string.IsNullOrEmpty(_nameProperty))
                {
                    return string.Empty;
                }
                return _nameProperty;
            }
            set
            {
                _nameProperty = value;
            }
        }
    }
}
~~~


**Form1.cs**

~~~
using System;
using System.Windows.Forms;

namespace EffectiveCSharp_Item1
{
    public partial class Form1 : Form
    {        
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            TestClass tc = new TestClass();

            tc.NameProperty = null;
            textBox1.DataBindings.Add("Text", tc, "NameProperty");
        }
    }
}
~~~

編譯 執行 都正常，null 會變 空字串。

![](..\images\2016-06-03-EffectiveCSharp_item1\emVuxw5.png)


----------


## 情況二：要改成多執行緒程式時 ##



**TestClass.cs**

~~~csharp

namespace EffectiveCSharp_Item1
{
    public class TestClass
    {
        private string _nameProperty;
        public string NameField;
        public string NameProperty
        {
            get
            {
                lock (this)
                {
                    if (string.IsNullOrEmpty(_nameProperty))
                    {
                        return string.Empty;
                    }
                    return _nameProperty;
                }
            }
            set
            {
                lock (this)
                {
                    _nameProperty = value;
                }
            }
        }
    }
}

~~~

----------

## 情況三：要增加觸發事件時 ##


**TestClass.cs**

~~~csharp

namespace EffectiveCSharp_Item1
{
    public class TestClass
    {
        public event EventHandler NameChanging;
        public event EventHandler NameChanged;

        private string _nameProperty;
        public string NameField;
        public string NameProperty
        {
            get
            {
                lock (this)
                {
                    if (string.IsNullOrEmpty(_nameProperty))
                    {
                        return string.Empty;
                    }
                    return _nameProperty;
                }
            }
            set
            {
                lock (this)
                {
                    NameChanging(this, new EventArgs());
                    _nameProperty = value;
                    NameChanged(this, new EventArgs());
                }
            }
        }
    }
}

~~~

----------

## 參考： ##

- http://www.clicktocontinue.com/books/EffectvCShrp.pdf
- https://dotblogs.com.tw/larrynung/archive/2009/07/08/9215.aspx
- https://msdn.microsoft.com/zh-tw/library/f7dy01k1(v=vs.110).aspx
- http://kevintsengtw.blogspot.tw/2011/09/property-field.html
- https://msdn.microsoft.com/zh-tw/library/a67b72ea(v=VS.90).aspx
- https://msdn.microsoft.com/zh-tw/library/9d65as2e(v=VS.90).aspx
- https://msdn.microsoft.com/zh-tw/library/w86s7x04.aspx
- https://wizardforcel.gitbooks.io/effective-csharp/content/1.html
