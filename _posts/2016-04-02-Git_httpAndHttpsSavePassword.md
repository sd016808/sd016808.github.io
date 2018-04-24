---
layout: post
title: 'Windows 透過 HTTP 與 HTTPS 連接 Git 儲存庫時如何記憶常用密碼'
author: 'James Peng'
tags: ['Git']
---

在 Windows 平台使用 Git 的時候有沒有這種困擾，每次要 git push 的時候都要不斷的輸入帳號密碼，我覺得經年累月之下對工作生產力的損失其實還蠻大的。其實透過 HTTP / HTTPS 連接 Git 儲存庫，有好多種方法，以下我們就來說明 Windows 平台：

在 CodePlex 上面有個 [Windows Credential Store for Git 專案](http://gitcredentialstore.codeplex.com/)，他提供一個實作 git credentials API 的工具，可以跟 [Git for Windows](http://git-scm.com/) 工具整合在一起，並且提供在 Windows 平台上更為安全的快取密碼方式。


----------


其安裝與設定的方式如下：

1.到 http://gitcredentialstore.codeplex.com/ 點選右邊的 download 下載此工具執行檔 

![](..\images\2016-04-02-Git_httpAndHttpsSavePassword\pbmCfwn.png)


----------


2.直接執行 git-credential-winstore.exe 進行安裝

![](..\images\2016-04-02-Git_httpAndHttpsSavePassword\221FEuP.png)


----------

3.設定完成後，下在你在執行 git push 時，他就會自動開啟 Windows 內建的「Windows 安全性」視窗，並且讓你輸入帳號密碼儲存。

![](..\images\2016-04-02-Git_httpAndHttpsSavePassword\8SK3fKo.png)


----------

比起你去設定 store 認證輔助方法，用這個工具儲存的密碼安全多了！因為這些密碼並不是用明碼儲存，而是儲存在 Windows 作業系統內建的「認證管理員」之中 (可以從控制台中找到)：

![](..\images\2016-04-02-Git_httpAndHttpsSavePassword\krVdu2E.png)


----------

如果你想要移除先前記憶過的密碼，也可以直接從「認證管理員」移除特定網站下的密碼即可：

![](..\images\2016-04-02-Git_httpAndHttpsSavePassword\EFkxSOT.png)



----------

再搭配BAT批次檔，更方便了~

~~~text
cd jia-hong-peng.github.io\
git add .
git commit -am 'add'
git push -u origin master
pause
~~~


------------

## 參考: ##

* http://blog.miniasp.com/post/2014/05/22/Credential-Store-for-Git-HTTP-HTTPS.aspx
* http://gitcredentialstore.codeplex.com/
* http://git-scm.com/
