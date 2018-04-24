---
layout: post
title: '安裝 Git for Windows'
author: 'James Peng'
tags: ['Git']
---

這是一個可以在 命令提示字元 (Command Prompt) 下執行的一套指令列工具，目前市面上所有 Git 的 GUI 工具，其實骨子裡都是執行這些較為底層的 Git 工具。

## 安裝過程： ##

先連到官網 http://git-scm.com/downloads ，下載windows安裝檔

![](..\images\2015-09-15-Git_Windows_install\i9Q5v0Y.png)

直接點擊下載的檔案進行安裝

![](..\images\2015-09-15-Git_Windows_install\qQEXfEh.png)

安裝程式歡迎頁面

![](..\images\2015-09-15-Git_Windows_install\Xk8ZOYK.png)

同意 GPL 授權條款

![](..\images\2015-09-15-Git_Windows_install\24NW9V5.png)

選擇安裝路徑

![](..\images\2015-09-15-Git_Windows_install\22kdWMg.png)

選取元件，保留預設選項即可

![](..\images\2015-09-15-Git_Windows_install\zqUhbV5.png)

設定程式集名稱，保留預設選項即可

![](..\images\2015-09-15-Git_Windows_install\3sHFHh5.png)

第一項 Use Git Bash only，不會去修改到 PATH 環境變數，是最穩定安全的選項，但是如果有需要在 console mode 命令提示字元 裡使用Git，還是要設PATH。

![](..\images\2015-09-15-Git_Windows_install\Fc9UfKQ.png)

所以這裡請選擇第二項：Run Git from the Windows Command Prompt ，會幫你設定好PATH環境變數。

![](..\images\2015-09-15-Git_Windows_install\DH1nK47.png)

換行字元轉換，設定換行字元的轉換方式，Windows 環境建議選擇第一種（請參考原文提示）。

![](..\images\2015-09-15-Git_Windows_install\sRuSucs.png)

開始進行安裝

![](..\images\2015-09-15-Git_Windows_install\6Q56p6o.png)

安裝完成

![](..\images\2015-09-15-Git_Windows_install\qLsnp6s.png)


安裝完成後，直接開啟命令提示字元，就可以開始使用

![](..\images\2015-09-15-Git_Windows_install\Xoa7zHR.png)


你可以輸入 git --version 指令查詢目前工具程式的版本

~~~text
git --version
~~~

![](..\images\2015-09-15-Git_Windows_install\wxM1FhB.png)

----------------------------

## 安裝 TortoiseGit

先到烏龜Git的官方網站下載最新版本

https://code.google.com/p/tortoisegit/

![](..\images\2015-09-15-Git_Windows_install\hJFtVFx.png)

下載了TortoiseGit主程式和 TortoiseGit中文語系

![](..\images\2015-09-15-Git_Windows_install\uhaL0Tw.png)

先安裝主程式

![](..\images\2015-09-15-Git_Windows_install\xSeJMSA.png)

軟體安裝過程就是一直下一步而已，這裡就不再詳細敘述

![](..\images\2015-09-15-Git_Windows_install\Tdnjoh6.png)

這裡是詢問要使用的SSH客戶端，預設是使用 TortoisePLink，和 Windows 的相容性較好，而且可以直接在視窗模式下設定SSH私鑰，不用到文字模式下設定，建議選擇此選項。

![](..\images\2015-09-15-Git_Windows_install\JHqGSqR.png)

選擇安裝路徑，用預設值就可以了

![](..\images\2015-09-15-Git_Windows_install\3011zUv.png)

點選 Install

![](..\images\2015-09-15-Git_Windows_install\alzyZ3q.png)


安裝中

![](..\images\2015-09-15-Git_Windows_install\nKi77V2.png)


TortoiseGit主程式安裝完成

![](..\images\2015-09-15-Git_Windows_install\g8bFnXf.png)

安裝完成

-----------------------

## 安裝TortoiseGit中文語系

![](..\images\2015-09-15-Git_Windows_install\2Epkx2D.png)

一下就裝好了

![](..\images\2015-09-15-Git_Windows_install\7oVncvM.png)


接著 在任意資料夾，點滑鼠右鍵選單，選TortoiseGit 進入Settings

![](..\images\2015-09-15-Git_Windows_install\vp1PGtU.png)

General 裡的 Language 改為 中文

![](..\images\2015-09-15-Git_Windows_install\iYXsaH2.png)

完成

![](..\images\2015-09-15-Git_Windows_install\yHQiGX6.png)

----------------------------

參考：

- https://code.google.com/p/tortoisegit/
- http://git-scm.com/downloads
