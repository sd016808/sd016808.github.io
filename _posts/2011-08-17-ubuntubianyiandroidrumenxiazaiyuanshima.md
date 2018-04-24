---
layout: post
title: 'ubuntu編譯Android入門：下載原始碼'
author: 'James Peng'
tags: ['Android']
---

 

#### 安裝 Repo

Repo 是 Google 為 Android source tree 的管理而寫的一個
script工具，以方便處理 Android 源碼包含的上百個 git
repositories，可以更容易地使用Git，更多關於 Repo 的資訊，請參考 [Version
Control](http://source.android.com/source/version-control.html)。

請按照下列步驟：

-   請確保在你的主目錄有一個 bin /目錄，並且它有設定在path路徑：

> `$ mkdir ~/bin        $ PATH=~/bin:$PATH `

 

[![image](http://lh3.ggpht.com/-S4wJhJiTz7o/Tktr3iDFBtI/AAAAAAAAK68/ieDs-Q4tIsQ/image_thumb%25255B4%25255D.png?imgmax=800 "image")](http://lh6.ggpht.com/-7fUQTdQgC3M/Tktr1mTgf8I/AAAAAAAAK64/z7l3gpWvGhE/s1600-h/image%25255B10%25255D.png)

-   下載 Repo script 並確保它是可執行文件：

> `$ curl https://android.git.kernel.org/repo > ~/bin/repo `
>
> `$ chmod a+x ~/bin/repo `

 

[![image](http://lh4.ggpht.com/--dIWh44mwyg/Tktr6S6_3SI/AAAAAAAAK7E/X79UdTjgMEg/image_thumb%25255B6%25255D.png?imgmax=800 "image")](http://lh6.ggpht.com/-DHYEX4mIHfU/Tktr5HLGnZI/AAAAAAAAK7A/L9ieppAE7XQ/s1600-h/image%25255B14%25255D.png)

 

#### 初始化 Repo client

安裝完成Repo後， 設置用戶端存取 android 原始碼庫：

-   建一個空目錄來保存你的文件：

> `$ mkdir WORKING_DIRECTORY `
>
> `$ cd WORKING_DIRECTORY `

``

-   執行 `repo init` 去獲取最新最新版本的Repo 與 所有最新的修正檔。
    您必須指定manifest的URL， 指出包含在 Android source的各種資源庫
    將被放置在您的目錄。

> `$ repo init -u git://android.git.kernel.org/platform/manifest.git `

要[開 branch 分支](http://ihower.tw/blog/archives/2620)， 加上參數 -b：

> `$ repo init -u git://android.git.kernel.org/platform/manifest.git -b froyo `

-   出現提示時，請設定您的真實姓名和電子郵件地址。

 

#### 獲取檔案

> `$ repo sync `

 

* * * * *

 

### 下一篇: 編譯原始碼

 

* * * * *

參考：

<http://source.android.com/source/downloading.html>

<http://ihower.tw/blog/archives/2620>

