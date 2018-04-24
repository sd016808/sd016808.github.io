---
layout: post
title: 'C# using敘述'
author: 'James Peng'
tags: ['C#']
---

using敘述獲得一個或多個resource，執行一敘述，然後釋放該resource。



----------


## 例如，using敘述的格式： ##


~~~csharp
using (R r1 = new R ( )) {
	r1.F( );
}
~~~

完全相等於：

~~~csharp
R r1 = new R( );
try {
	r1.F( );
}
finally {
	if (r1 != null) ((IDisposable)r1).Dispose( );
}
~~~

----------

## resource-acquisition可能獲得給定型別的多個resource ##

~~~csharp
using (R r1 = new R( ), r2 = new R( )) {
	r1.F( );
	r2.F( );
}
~~~

完全相等於：

~~~csharp
using (R r1 = new R( ))
{
	using (R r2 = new R( )) {
		r1.F( );
		r2.F( );
	}
}
~~~

也完全相等於：

~~~csharp
R r1 = new R( );
try {
	R r2 = new R( );
	try {
		r1.F( );
		r2.F( );
	}
	finally {
		if (r2 != null) ((IDisposable)r2).Dispose( );
	}
}
finally {
	if (r1 != null) ((IDisposable)r1).Dispose( );
}
~~~

----------

參考：

- 書名：Microsoft C#語言白皮書, 出版日期：2001/11/28, 書號：957-819-017-4, ISBN：957-819-017-4, 原作者：Microsoft Corporation, 譯者：黃捷森

