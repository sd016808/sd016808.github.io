---
layout: post
title: '介面與執行序Only the original thread that created a view hierarchy can touch its views.'
author: 'James Peng'
tags: ['Android']
---

> 01-01 00:52:58.330: E/AndroidRuntime(1996):
> android.view.ViewRoot\$CalledFromWrongThreadException: Only the
> original thread that created a view hierarchy can touch its views.

你不能在thread 裡更新view，你的thread 不能操作組件

![image](http://lh6.ggpht.com/-fguAtJss0hg/Tvk-ANxk3aI/AAAAAAAAMGA/gqWpW-w2LIE/image%25255B10%25255D.png?imgmax=800 "image")

如果我們想要在另一個Thread中操控 UI
，在Swing中我們可以直接使用(可能會有些問題，不過還是可以直接用)，不過，在Android就不行了。  
如果我們直接使用的話，會丟出一個
android.view.ViewRoot\$CalledFromWrongThreadException: Only the original
thread that created a view hierarchy can touch its views 錯誤

 

結果其實很簡單，

Android不能在另外的Thread裡
setContentView(R.layout.main)，只能在主線程裡調用（即是Activity的Thread）。你可以在你的Activity的類或者自定義View的類裡加入一個Handler的內部類來處理。

 

說白了就是對於ui的操作必須放在主Thread，你新開Thread做的最多也就是傳個Message給handler來觸發對ui的操作。

 

所以要用android.os.Handler配合android.os.Message來處理。但也許這在android開發是一般常識，不過因為我們學習的方法，並沒有從基本原理學起，而是直接看許多範例程式，才會不知道要怎麼讓Model跟View互動。

 

> Handler handler = new Handler()   
> {
>
>      public void handleMessage(Message msg)   
>     {  
>         switch (msg.what)   
>          {  
>                 case MEG\_INVALIDATE:  
>                     // 重繪UI  
>                     break;
>
>             }  
>         super.handleMessage(msg);  
>         }
>
>     };

 

我們必須透過Message的傳遞來達到目的。

> handler.sendMessage(m);

 

 

 

 

參考:

<http://ccanywhere.pixnet.net/blog/post/25520924-android%E4%B8%AD-thread-%E8%88%87-view%E7%9A%84%E6%BA%9D%E9%80%9A-(%E4%B8%80)>

<http://www.javaworld.com.tw/roller/yaocl/entry/android_multithread_%E7%9A%84%E8%A8%8A%E6%81%AF%E5%82%B3%E9%81%9E_viewroot_calledfromwrongthreadexception>

<http://www.eoeandroid.com/thread-7703-1-1.html>

