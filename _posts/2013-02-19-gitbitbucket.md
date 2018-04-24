---
layout: post
title: 'Git bitbucket'
author: 'James Peng'
tags: ['Git']
---

# 新專案時：

已存在的專案，沒用過git先init，然後設定remote路徑

> mkdir /path/to/your/project
>
> cd /path/to/your/project
>
> git init
>
> git remote add origin
> ssh://git@bitbucket.org/**username**/**myproject**.git

因為新專案，需要先加入「所有檔案」

> git add **.   \# 注意有個「.」**
>
> git commit -m "First commit. Adding all files."     \#
> 藍色是註解請隨意

接著上傳到bitbucket

> git push -u origin master   \# to push changes for the first time

結果成功！

> *成功訊息如下*
>
> *Counting objects: 99, done.  
> Delta compression using up to 2 threads.  
> Compressing objects: 100% (92/92), done.  
> Writing objects: 100% (99/99), 900.82 KiB, done.  
> Total 99 (delta 22), reused 0 (delta 0)  
> remote: bb/acl: james is allowed. accepted payload.*
>
> *略…  
> *

 

* * * * *

如果發生：

> \$ git push -u origin master  
> The authenticity of host 'bitbucket.org (207.223.240.181)' can't be
> established.  
> RSA key fingerprint is
> 97:8c:1b:f2:6f:14:6b:5c:3b:ec:aa:46:46:74:7c:40.  
> Are you sure you want to continue connecting (yes/no)? yes  
> Warning: Permanently added 'bitbucket.org,207.223.240.181' (RSA) to
> the list of known hosts.  
> Permission denied (publickey).  
> fatal: The remote end hung up unexpectedly  

檢查bitbucket連線：

> ssh -T git@bitbucket.org

錯誤信息..”**Permission Denied (publickey)**”是沒有把**Public key**
加到**BitBucket** 上..

請參考「前置作業」

* * * * *

# **已存在的GIT專案：**

先檢查remote路徑

> git remote -v

不正確的話，移除remote路徑。

> git remote rm origin

* * * * *

重新建立remote路徑

只需要改動紅色**username**和藍色**myproject**部分。

> cd /path/to/my/repo  
> git remote add origin
> ssh://git@bitbucket.org/**username**/**myproject**.git  
> git push -u origin master   \# to push changes for the first time

push成功！

> \$ git push -u origin master  
> Counting objects: 99, done.  
> Delta compression using up to 2 threads.  
> Compressing objects: 100% (92/92), done.  
> Writing objects: 100% (99/99), 900.82 KiB, done.  
> Total 99 (delta 22), reused 0 (delta 0)  
> remote: bb/acl: james is allowed. accepted payload.  

成功！

* * * * *

如果發生：

> \$ git push -u origin master  
> The authenticity of host 'bitbucket.org (207.223.240.181)' can't be
> established.  
> RSA key fingerprint is
> 97:8c:1b:f2:6f:14:6b:5c:3b:ec:aa:46:46:74:7c:40.  
> Are you sure you want to continue connecting (yes/no)? yes  
> Warning: Permanently added 'bitbucket.org,207.223.240.181' (RSA) to
> the list of known hosts.  
> Permission denied (publickey).  
> fatal: The remote end hung up unexpectedly  

檢查bitbucket連線：

> ssh -T git@bitbucket.org

錯誤信息..”**Permission Denied (publickey)**”是沒有把**Public key**
加到**BitBucket** 上..

請參考「前置作業」

* * * * *

# 前置作業：

你需要先建立ssh key

> ssh-keygen -t rsa -C "**your\_email@youremail.com**"

接著把金鑰複製起來。

> cat \~/.ssh/id\_rsa.pub  

![image](http://lh6.ggpht.com/-tSo-F4K2tRo/USQrIKwlqWI/AAAAAAAATCA/kHYPmvmM9jk/image19%25255B1%25255D.png?imgmax=800 "image")

* * * * *

開啟網站，登入我們的 BitBucket Account..

[![image](http://lh3.ggpht.com/-Dq0Wvm7ibL4/USN293GUnaI/AAAAAAAASyE/6eH5puPfbTg/image_thumb%25255B10%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-Km7sHEuY0IU/USN29dwTqTI/AAAAAAAASx8/dURNeaApVH0/s1600-h/image%25255B18%25255D.png)

![image](http://lh6.ggpht.com/-j4YWGLmFmY8/USN2_SL9Z_I/AAAAAAAATCI/qd95TEwnPqM/image5%25255B1%25255D.png?imgmax=800 "image")

![image](http://lh6.ggpht.com/-7-x5oTDs6G8/USN3AxuT_CI/AAAAAAAATCM/RXlpP64_h2o/image10%25255B1%25255D.png?imgmax=800 "image")

存檔。

如過存檔過不了，注意可能是用cygwin複製時「自動斷行」了，ssh key
沒有斷行。

* * * * *

檢查bitbucket連線：

> ssh -T git@bitbucket.org

正常連線成功！

![image](http://lh3.ggpht.com/-qlFpt6LglKw/USN3Cj5FRII/AAAAAAAATCQ/K-odOUDWoMU/image24%25255B1%25255D.png?imgmax=800 "image")

* * * * *

參考：

-   <https://confluence.atlassian.com/display/BITBUCKET/Using+the+SSH+protocol+with+Bitbucket>
-   <http://www.therealtimeweb.com/index.cfm/2012/5/16/move-git-repo-to-bitbucket>
-   <http://blog.sharechiwai.com/2011/11/setup-git-for-bitbucket-on-windows-part-ii-%E5%9C%A8windows-%E4%B8%8A%E8%A8%AD%E5%AE%9Agit-part-ii/>

