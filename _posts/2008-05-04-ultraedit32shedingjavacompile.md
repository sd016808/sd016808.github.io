---
layout: post
title: 'UltraEdit-32 設定 java compile'
author: 'James Peng'
tags: ['Java']
---

java compile  
  
 設定UltraEdit：[Advance] → [Tool Configuration]  
[![](http://bp0.blogger.com/_AnTT9cbXdqY/R1OrIJjHjZI/AAAAAAAAAZs/Ftd12prhVfo/s320/fetch.php.gif)](http://bp0.blogger.com/_AnTT9cbXdqY/R1OrIJjHjZI/AAAAAAAAAZs/wQDeANl4c28/s1600-R/fetch.php.gif)  
  
[![](http://bp1.blogger.com/_AnTT9cbXdqY/R1OrdZjHjaI/AAAAAAAAAZ0/AYvjkjc_RMA/s320/fetch.php.gif)](http://bp1.blogger.com/_AnTT9cbXdqY/R1OrdZjHjaI/AAAAAAAAAZ0/bpF-Scei-B4/s1600-R/fetch.php.gif)  
  
功能意義：  
  
功能表項目名稱(Menu Item Name)：自己取的名字  
命令行(Command Line)：設定執行檔  
工作資料夾(Working Directory)：設定預設路徑  
  
請填入  
  
功能表項目名稱(Menu Item Name)：compile
(或是你想輸入中文”執行”也沒關係)  
命令行(Command Line)：C:\\j2sdk1.4\\bin\\javac.exe "%f"  
工作資料夾(Working Directory)：%p  
  
  
[![](http://bp1.blogger.com/_AnTT9cbXdqY/R1OwcZjHjbI/AAAAAAAAAZ8/eHlmTyuoLDs/s320/fetch.php.gif)](http://bp1.blogger.com/_AnTT9cbXdqY/R1OwcZjHjbI/AAAAAAAAAZ8/Vi6BmJ37w04/s1600-R/fetch.php.gif)  
  
然後把下面 Output to List Box 圈起來  
Capture Output 勾起來  
這樣才可以看到執行後出現的文字  
  
按下右邊的 Insert 就可以加進去當成熱鍵了
