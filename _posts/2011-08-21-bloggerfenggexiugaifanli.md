---
layout: post
title: 'Blogger風格修改範例'
author: 'James Peng'
tags: ['Google']
---

到後台的範本裡，先「下載完整範本」

[![image](http://lh4.ggpht.com/-iJTKl2874-M/TlEbwjZMrpI/AAAAAAAAK8Q/ubnyuXvBkGs/image_thumb%25255B6%25255D.png?imgmax=800 "image")](http://lh5.ggpht.com/-KnOCoVwjRdk/TlEbwIZPq3I/AAAAAAAAK8M/aWwpH6ZjlfA/s1600-h/image%25255B8%25255D.png)

下載 xml範本檔案

[![image](http://lh6.ggpht.com/-w32CvSX3Nng/TlEbxrKqctI/AAAAAAAAK8Y/_3k7ju5yaEg/image_thumb%25255B9%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-h1j7FKkq5QI/TlEbxCxKzQI/AAAAAAAAK8U/eAxX7dQHilk/s1600-h/image%25255B15%25255D.png)

使用文字編輯器打開xml

[![image](http://lh6.ggpht.com/-R076G6xSggU/TlEbya-DCTI/AAAAAAAAK8g/LnHDRBmNSrQ/image_thumb%25255B11%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-UrdafrD7850/TlEbx91PCzI/AAAAAAAAK8c/MvBykxPfGwY/s1600-h/image%25255B19%25255D.png)

 

* * * * *

 

Blogger 官方閱讀功能 靠右對齊

> .jump-link{text-align: right;}

CSS放在body{}之後

[![image](http://lh6.ggpht.com/-glIdsu2FHlY/TlEbzT6nL7I/AAAAAAAAK8o/nP3UlFeAPRo/image_thumb%25255B13%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-5HmUpc-fFxA/TlEby97L8CI/AAAAAAAAK8k/Sk2pfHKykKY/s1600-h/image%25255B23%25255D.png)

 

* * * * *

blogger增加facebook讚

 

CTEL+F 尋找

[![image](http://lh5.ggpht.com/-0pxqkBUsXdY/TlEb0Sh6sEI/AAAAAAAAK8w/99LAtaXRe40/image_thumb%25255B17%25255D.png?imgmax=800 "image")](http://lh6.ggpht.com/-51x-nPQ8SL0/TlEbz7FVR3I/AAAAAAAAK8s/Tmmnlmk929Y/s1600-h/image%25255B29%25255D.png)

 

找

> \<b:if cond='data:post.hasJumpLink'\>

 

[![image](http://lh6.ggpht.com/-qZY3IKtXzRY/TlEb1cTrdtI/AAAAAAAAK84/7ZucjEwLHFU/image_thumb%25255B29%25255D.png?imgmax=800 "image")](http://lh6.ggpht.com/-BaTnLbbdwY0/TlEb0469S-I/AAAAAAAAK80/me-2539nir0/s1600-h/image%25255B49%25255D.png)

 

在此行的上面，貼上下列語法：

> \<script\>  
> document.write(&\#39;&lt;iframe
> src=&quot;http://www.facebook.com/plugins/like.php?href=\<data:post.url/\>&amp;layout=standard&amp;show\_faces=true&amp;width=450&amp;action=like&amp;font=verdana&amp;colorscheme=light&quot;
> scrolling=&quot;no&quot; frameborder=&quot;0&quot;
> allowTransparency=&quot;true&quot; style=&quot;border:none;
> overflow:hidden; width:450px;
> height:65px&quot;&gt;&lt;/iframe&gt;&\#39;);  
> \</script\>

 

[![image](http://lh5.ggpht.com/-3fInfDl9nsI/TlEb2LR3nGI/AAAAAAAAK9A/Fm4mDb_MCY0/image_thumb%25255B31%25255D.png?imgmax=800 "image")](http://lh5.ggpht.com/-YleRiZXE-XM/TlEb1mfxurI/AAAAAAAAK88/wzbcz083HNk/s1600-h/image%25255B53%25255D.png)

 

* * * * *

 

blogger增加facebook留言

找

> \<html

在該行最後面加上

> **xmlns:fb='http://www.facebook.com/2008/fbml'**

[![image](http://lh5.ggpht.com/-VauiezIfkeM/TlEb3Opny5I/AAAAAAAAK9I/JvSMdbCtLIQ/image_thumb%25255B37%25255D.png?imgmax=800 "image")](http://lh6.ggpht.com/-ImqvTF85Kls/TlEb2lY69tI/AAAAAAAAK9E/hlZM9FMuiug/s1600-h/image%25255B69%25255D.png)

變成如下

> \<html b:version='2' class='v2' expr:dir='data:blog.languageDirection'
> xmlns='<http://www.w3.org/1999/xhtml'>
> xmlns:b='<http://www.google.com/2005/gml/b'>
> xmlns:data='<http://www.google.com/2005/gml/data'>
> xmlns:expr='<http://www.google.com/2005/gml/expr'>
> xmlns:fb='<http://www.facebook.com/2008/fbml'>  \>

* * * * *

找

> \</head\>

 

\</head\>上方，加上

> **\<script
> src='http://static.ak.connect.facebook.com/js/api\_lib/v0.4/FeatureLoader.js.php'
> type='text/javascript'/\>**

 

[![image](http://lh5.ggpht.com/-a3ZVM_Qh508/TlEb4HX36YI/AAAAAAAAK9Q/YcGMapQnZ-Y/image_thumb%25255B39%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-rhRleUAq_tI/TlEb3lkwC9I/AAAAAAAAK9M/pxHTf7JjP8I/s1600-h/image%25255B73%25255D.png)

 

* * * * *

找

> \</body\>

在\</body\>之前加上

> \<script type='text/javascript'\>  
> //\<![CDATA[  
> FB.init('2ed3593254230a5794aab632431eda5f', '');  
> //]]\>  
> \</script\>

 

[![image](http://lh6.ggpht.com/-ujvxe9XQi1o/TlEe4nwC_2I/AAAAAAAAK-A/gU_TvG3ruo4/image_thumb%25255B52%25255D.png?imgmax=800 "image")](http://lh3.ggpht.com/-jftlwbEzLRM/TlEe4UMgHyI/AAAAAAAAK98/5lfi4GTb64I/s1600-h/image%25255B100%25255D.png)

 

* * * * *

找

> \<data:post.body/\>

 

後面加上

 

> **\<center\>  
> \<fb:comments width='620'/\>  
> \</center\>**

 

[![image](http://lh3.ggpht.com/-e00P2740cQo/TlEb57VeGNI/AAAAAAAAK9g/XZNlomfXu2U/image_thumb%25255B43%25255D.png?imgmax=800 "image")](http://lh3.ggpht.com/-yrUHHk26eJY/TlEb5W2cmTI/AAAAAAAAK9c/X0LCKgJ50ss/s1600-h/image%25255B81%25255D.png)

 

* * * * *

最後

「上載」範本

 

[![image](http://lh3.ggpht.com/-YMJAeIR6huQ/TlEb6hxrKoI/AAAAAAAAK9o/MQXyNy3N6H4/image_thumb%25255B45%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-Eq_3G9dwB0E/TlEb6NoCH8I/AAAAAAAAK9k/s3qLUpSgyYo/s1600-h/image%25255B85%25255D.png)

 
