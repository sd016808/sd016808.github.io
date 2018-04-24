---
layout: post
title: 'WIN7 使用批次檔 移除Win10升級提示 '
author: 'James Peng'
tags: ['Windows']
---

自從微軟發佈Win10後，會在右下角強制跳出提示用戶有免費升級取得Win10，如果有用過Windows系統用戶都知道，每次系統剛推出時，支援度是最大的問題點，或許微軟想模仿Apple方式，提供免費升級機制，藉此機會挽回使用者，但這也造成原本舊用戶的一大惡夢，大多數使用者是不想升級，除非是想嘗試新用戶的使用者來將就無任何差別，但該怎麼移除這升級提示呢？


## 解法： ##

存成bat批次檔

~~~text
@echo off
For /f "tokens=1-3 delims=/ " %%a in ('date /t') do set mydate=%%a-%%b-%%c
For /f "tokens=1-3 delims=:." %%a in ("%time%") do set mytime=%%a:%%b:%%c
set timestamp=%mydate% %mytime%
echo %timestamp% 開始停用Win10更新程序...>%UserProfile%\Desktop\%~n0.txt

echo 移除常駐服務 GWX 相關程序...
tasklist /fi "imagename eq GWX.exe" |find ":" > nul
if errorlevel 1 taskkill /f /im "GWX.exe"
tasklist /fi "imagename eq GWXUX.exe" |find ":" > nul
if errorlevel 1 taskkill /f /im "GWXUX.exe"
if exist C:\Windows\System32\GWX takeown /f GWX
if exist C:\Windows\System32\GWX icacls GWX /e /g everyone:f
if exist C:\Windows\System32\GWX rd/q/s GWX
echo 登錄檔加入停用升級之機碼...
reg add HKLM\SOFTWARE\Policies\Microsoft\Windows\GWX /f > nul
reg add HKLM\SOFTWARE\Policies\Microsoft\Windows\GWX /v DisableGWX /t REG_DWORD /d 00000001 /f > nul
reg add HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate /f > nul
reg add HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate /v DisableOSUpgrade /t REG_DWORD /d 00000001 /f > nul
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate\OSUpgrade /f > nul
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate\OSUpgrade /v ReservationsAllowed /t REG_DWORD /d 00000000 /f > nul
echo 移除 KB3012973 更新...
start "title" /b /wait wusa /uninstall /quiet /norestart /log /kb:3012973 >nul 2>&1
echo 移除 KB3021917 更新...
start "title" /b /wait wusa /uninstall /quiet /norestart /log /kb:3021917 >nul 2>&1
echo 移除 KB3035583 更新...
start "title" /b /wait wusa /uninstall /quiet /norestart /log /kb:3035583 >nul 2>&1
rem 刪除已下載的暫存檔
if not exist c:\$WINDOWS.~BT\nul goto :next1
echo 刪除 $WINDOWS.~BT 資料夾...
takeown /F C:\$Windows.~BT\* /R /A 
icacls C:\$Windows.~BT\*.* /T /grant everyone:F
rd /s/q c:\$WINDOWS.~BT
:next1
if not exist c:\$WINDOWS.~BT\nul goto :next2
echo 刪除 $WINDOWS.~WS 資料夾...
takeown /F C:\$Windows.~WS\* /R /A 
icacls C:\$Windows.~WS\*.* /T /grant everyone:F
rd /s/q c:\$WINDOWS.~WS
:next2
echo.
echo 移除Win10更新完成，請有空時重新開機
echo %timestamp% 移除Win10更新完成，請有空時重新開機>>%UserProfile%\Desktop\%~n0.txt
echo 下次執行Windows更新時，若出現下方名稱請將之隱藏：
echo %timestamp% 下次執行Windows更新時，若出現下方名稱請將之隱藏：>>%UserProfile%\Desktop\%~n0.txt
echo KB3012973, KB3021917, KB3035583
echo %timestamp% KB3012973, KB3021917, KB3035583>>%UserProfile%\Desktop\%~n0.txt
timeout 15
~~~   

點兩下執行完畢後會在桌面留下一個Log，重開機之後，就完成了

----------


## 參考 ##

- https://mrmad.com.tw/teaching-permanently-removed-within-10-seconds-windows10-reminds-free-upgrade-message

  

