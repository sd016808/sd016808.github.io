---
layout: post
title: 'Bejeweled2 Deluxe 製作遊戲修改器教學'
author: 'James Peng'
tags: ['Visual C++']
---





![](..\images\2015-07-23-Bejeweled2-Deluxe-Cheat-Tool\fztYy0z.png)



- 修改的遊戲：Bejeweled 2 Deluxe 1.0
- 工具：Cheat Engine 6.4



![](..\images\2015-07-23-Bejeweled2-Deluxe-Cheat-Tool\eLfIadg.png)


## Step1 ##
使用 Cheat Engine 6.4 選擇目標遊戲 (Select a process to open)
![](..\images\2015-07-23-Bejeweled2-Deluxe-Cheat-Tool\AlVKKtk.png)


## Step2 ##

首先試著找到位址
![](..\images\2015-07-23-Bejeweled2-Deluxe-Cheat-Tool\uElPBv1.png)

改變一下分數，會發現有兩個Address是我們要的

![](..\images\2015-07-23-Bejeweled2-Deluxe-Cheat-Tool\89ZLp2J.png)

雙擊加入(我認定你知道怎麼找到位址了)


## Step3 ##

![](..\images\2015-07-23-Bejeweled2-Deluxe-Cheat-Tool\F8lNKKj.png)


當你已經找到位址時，在表格區的位址按右鍵並選擇
"找出寫入此位址的位址(Find out what writes to this address)"

![](..\images\2015-07-23-Bejeweled2-Deluxe-Cheat-Tool\nioH9rk.png)


按More information，出現一個新的視窗

![](..\images\2015-07-23-Bejeweled2-Deluxe-Cheat-Tool\hsoEiP2.png)

裡面有一段話，寫說需要被找到pointer位址的值可能是061BA9F0 (記住此值)

還有一個重點[ebx+00015DF0]

offset = 00015DF0 (記住此值)


回到cheat engine 找這個值。
把Hex打勾(Hex打勾代表用16進位制)，在value:裡面輸入值61BA9F0（前面0不用打），按下First Scan。

![](..\images\2015-07-23-Bejeweled2-Deluxe-Cheat-Tool\8aSA8CQ.png)

成功的話會找到一條Address是綠色的，

恭喜你找到pointer位址：0059ACB8。



## Step4 ##
驗證一下

pointer位址：0059ACB8。
offset = 00015DF0 (記住此值)

- 按Add address manually(手動加入位址)
- Pointer打勾
- 先輸入offset
- 指針的位址0059ACB8 輸入再最下面一格
- 會看到算出的位址 = 061D07E0 和 搜尋到的位址一樣
- 這樣遊戲重開位址改變還是能修改成功！

![](..\images\2015-07-23-Bejeweled2-Deluxe-Cheat-Tool\Qyl6SMk.png)

## Step5 ##
source code 裡加入address和offset
![](..\images\2015-07-23-Bejeweled2-Deluxe-Cheat-Tool\koUtG7t.png)


![](..\images\2015-07-23-Bejeweled2-Deluxe-Cheat-Tool\3juYHce.png)

[點此下載遊戲修改器](https://drive.google.com/open?id=0BzUSEyOU2e3zQ000VW45QnIzcXc)


## 參考文獻 ##
- http://forum.gamer.com.tw/C.php?bsn=02450&snA=674
- http://blog.yam.com/celearning/article/35309209
- http://www.bcwhy.com/thread-21055-1-1.html
- http://bejeweled-2-deluxe.soft32.com/
