---
layout: post
title: 'Ubuntu 11.04 真．雙系統，獨立磁碟分割，讓 Ubuntu 不再受到 Windows 拖累'
author: 'James Peng'
tags: ['Linux','Windows']
---

 

> e04，Windows 又要重灌了，害我的 Wubi Ubuntu 也他馬的要跟著重灌

 

  
在 Windows 桌機上，安裝 Ubuntu - Windows 雙系統的方式，有三種可以選擇：

1.  虛擬雙系統：  
    在 Windows 裡面安裝虛擬軟體 Virtualbox，然後在 Virtualbox
    所設定的虛擬機器中，安裝 Ubuntu。  
    每次開機時，要先開 Windows，再開 Virtualbox，最後才是虛擬的 Ubuntu。
2.  偽．雙系統：  
    在 Windows 裡面安裝雙開機軟體 Wubi，將 Ubuntu 當作 Windows
    中的一個程式來執行。  
    每次開機時，可以選擇要進入 Windows 或是 Ubuntu。（Windows 開機選單）
3.  真．雙系統：  
    在 Windows 裡面將 C 槽切割，撥出兩個分割區域，獨立安裝 Ubuntu。  
    每次開機時，也是可以選擇要進入 Windows 或是 Ubuntu。（GRUB
    開機選單）

 

* * * * *

先確定為 368GB 的分割區，正是我準備要拿來安裝 Ubuntu 的磁碟分割區。

[![DSCF6553](http://lh3.ggpht.com/-x24pVeODa_k/TkNAhxywB8I/AAAAAAAAK5Q/p-XZlRNCSN4/DSCF6553_thumb%25255B4%25255D.jpg?imgmax=800 "DSCF6553")](http://lh6.ggpht.com/-GtGHOa_lv5I/TkNAThcIToI/AAAAAAAAK5M/pWtwafvm0QM/s1600-h/DSCF6553%25255B13%25255D.jpg)

F12光碟開機

[![DSCF6554](http://lh5.ggpht.com/-p38LCbrVQRs/TkNA1c30q5I/AAAAAAAAK5Y/HB1jJ2f6qLY/DSCF6554_thumb%25255B2%25255D.jpg?imgmax=800 "DSCF6554")](http://lh3.ggpht.com/-NEHZOsZGVE8/TkNAsjotXEI/AAAAAAAAK5U/yG5upQdx2bc/s1600-h/DSCF6554%25255B11%25255D.jpg)

[![DSCF6555](http://lh3.ggpht.com/-YUNsZezvJvA/TkNBLxRUSRI/AAAAAAAAK5g/AvcAJUeVzpQ/DSCF6555_thumb%25255B1%25255D.jpg?imgmax=800 "DSCF6555")](http://lh5.ggpht.com/-AtZVZj4GZfw/TkNBJ-kOjJI/AAAAAAAAK5c/UC2zi46Gr3U/s1600-h/DSCF6555%25255B7%25255D.jpg)

安裝的方式分為三種：  
A. 偽．雙系統：  
在 Windows 裡面安裝雙開機軟體 Wubi，將 Ubuntu 當作 Windows
中的一個程式來執行。  
每次開機時，可以選擇要進入 Windows 或是 Ubuntu。（Windows 開機選單）

  
B. 消除Windows

  
C. 自行分割磁碟。

[![DSCF6556](http://lh5.ggpht.com/-obyaGLUvyfc/TkNBl34eCpI/AAAAAAAAK5o/Oevfo2XMkS8/DSCF6556_thumb%25255B1%25255D.jpg?imgmax=800 "DSCF6556")](http://lh6.ggpht.com/-5i1pJmB_eZs/TkNBh7fvAKI/AAAAAAAAK5k/uvkOrtY2jFM/s1600-h/DSCF6556%25255B7%25255D.jpg)

「偽．雙系統」的可憐宿命，由於 Wubi 是讓 Ubuntu 以寄生的方式，存活在
Windows 之中，所以，一旦 Windows 宿主掛掉重灌，內部的 Wubi - Ubuntu
也會隨之消逝。重灌 Windows 的話，這個安裝在 Windows 內的 Wubi - Ubuntu
是否可以保留呢？Google 過後，答案是：不行！

而必須採用「真．雙系統」的作法，徹底的把兩個作業系統安裝在獨立的磁碟分割區才行，請選擇「其他」。

 

確定為第三個分割區，先刪除。

[![DSCF6557](http://lh5.ggpht.com/-k3FFMUz0bew/TkNCc18iqdI/AAAAAAAAK5w/qHjs7wwIM0g/DSCF6557_thumb%25255B1%25255D.jpg?imgmax=800 "DSCF6557")](http://lh6.ggpht.com/-vuWyPEVZv2o/TkNCawNDWDI/AAAAAAAAK5s/TXlQ1gqLuW4/s1600-h/DSCF6557%25255B7%25255D.jpg)

點選「加入」

[![DSCF6558](http://lh4.ggpht.com/-H5-ZPxbcps4/TkNC9DdYBhI/AAAAAAAAK54/OQfV0mAVBqM/DSCF6558_thumb%25255B1%25255D.jpg?imgmax=800 "DSCF6558")](http://lh5.ggpht.com/--75Gw1JvCq8/TkNCvDvVDZI/AAAAAAAAK50/959BnRQ5_Uw/s1600-h/DSCF6558%25255B7%25255D.jpg)

由於 主分割區(primary)，同一顆硬碟上頂多只能有 4
個，於是把這個分割區設定為 邏輯分割區(logical)。

減去電腦的記憶體 RAM 的容量（4G）之後當swap，就是這個 Ubuntu
系統分割區應該有的大小。

 

> 新分割區的類型：邏輯分割區
>
> 分割區的大小：390247MB
>
> 新分割區的位置：（位於可用空間的）開始位置。
>
> 用途：Ext4日誌式檔案系統。據說是蠻不賴的一種檔案系統。
>
> 掛載點：「/」 把這個分割區設定為 Ubuntu 系統的根目錄。
>
> 按「確定」。

 

> 新分割區的類型：邏輯分割區
>
> 分割區的大小：5000MB
>
> 新分割區的位置：（位於可用空間的）開始位置。
>
> 用途：置換空間
>
> 掛載點：無。
>
> 按「確定」。

[![DSCF6565](http://lh3.ggpht.com/-qwkZQha6ZEI/TkNDOr2db0I/AAAAAAAAK6A/G5DZkyxWAr4/DSCF6565_thumb.jpg?imgmax=800 "DSCF6565")](http://lh4.ggpht.com/-viL5Hj8Z5aI/TkNDM31toWI/AAAAAAAAK58/R22c5QpXW8c/s1600-h/DSCF6565%25255B3%25255D.jpg)

到這邊，硬碟分割的設定就完成了。按「向前」繼續。

 

安裝完成即可。

 

參考：

-   <http://etblue.blogspot.com/2011/02/steps-install-real-independent-ubuntu.html>

