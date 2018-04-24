---
layout: post
title: 'Windows 下使用 VC2010 編譯 OpenSSL'
author: 'James Peng'
tags: ['Visual Studio']
---


OpenSSL是一套提供許多免費加密函數的開放程式碼，這裡簡單介紹如何在Windows環境下透過Visual Studio編譯OpenSSL.exe。


----------

## [操作步驟] ##

**準備工具**

- VC2010：請自行安裝Visual Studio 6、Visual Studio 2010或其他版本。
- Perl：請自行依據自己機器使用的Windows環境，下載對應的x86或x64版本的ActivePerl http://www.activestate.com/activeperl/downloads
- OpenSSL：下載OpenSSL原始碼 http://www.openssl.org/source/
- NASM或MASM組合語言組譯器： 

	- NASM是一種可攜性高且可跨多個作業系統平台的組合語言組譯器，下載連結http://www.nasm.us/
	- MASM微軟推行的組合語言組譯器，不具備跨平台的特性，僅能在Intel平台上使用，下載連結http://www.masm32.com/


----------


## ActivePerl與NASM安裝與設定 ##

- ActivePerl安裝完成後，開啟Visual Studio Command Prompt (2010)，輸入Perl -v，如果有顯示Perl的版本，表示ActivePerl安裝完成。
P.S.若沒有顯示Perl的版本，請重開機後再將此步驟重做一次即可。

- NASM提供兩種方式安裝，一種需要透過exe安裝，另一則為直接使用已build好的exe，若是使用官網提供的已build好套件，僅需將套件中的exe存放至預設安裝路經下C:\Program Files (x86)\NASM，最後再設定環境變數即可，如下操作。 電腦 -> 系統內容 -> 進階系統設定 -> 環境變數，在系統變數欄位裡有一個變數名為Path，在變數值欄位裡的最後面先加一個分號 ";"，在分號後面加入NASM資料夾的路徑C:\Program Files (x86)\NASM


----------

## 編譯OpenSSL流程 ##

- 開啟Visual Studio Command Prompt (2010)視窗，切換至解壓縮的OpenSSL資料夾。

- 設定編譯OpenSSL.exe Configure設定。

	- x86環境使用：perl Configure VC-WIN32
	- x64環境使用：perl Configure VC-WIN64A

- P.S.若有需要將Build好的OpenSSL.exe及其相關檔案存其他路徑，請在VC-WIN32或VC-WIN64A後面加上--prefix=<資料夾路徑>。

	- x86環境使用：perl Configure VC-WIN32 --prefix=C:\TestOpenSSL
	- x64環境使用：perl Configure VC-WIN64A --prefix=C:\TestOpenSSL

- 建立Makefile文件，使用NASM組合語言編譯ms\do_nasm

	- 如果使用MASM，接著輸入：ms\do_masm
	- 如果使用NASM，接著輸入：ms\do_nasm
	- 如果不使用組合語言編譯器，輸入:ms\do_ms

- 選擇OpenSSL.exe編譯的類型。

	- 動態庫連結nmake -f ms\ntdll.mak
	- 靜態庫連結nmake -f ms\nt.mak

- P.S. 編譯過程發生錯誤，參考如下方式處理

	- (1) 確認NASM是否有添加環境變數到系統
	- (2) 環境變數已添加，請重開機再試一次


- 驗證編譯好的檔案是否可以正常運作。

	- 動態庫測試nmake -f ms\ntdll.mak test
	- 靜態庫測試nmake -f ms\nt.mak test

- 安裝編譯好的檔案。

	- 動態庫安裝nmake -f ms\ntdll.mak install
	- 靜態庫安裝nmake -f ms\nt.mak install
	- 此部分完成後，若在步驟2沒有設定需要另外輸出的資料夾路徑，將會在openssl資料夾外產生一個user的資料夾來存放openssl.exe、libeay32.dll、ssleay32.dll


- 若要清除編譯的檔案，可參考如下指令。

	- 動態庫清除nmake -f ms\ntdll.mak clean
	- 靜態庫清除nmake -f ms\nt.mak clean

- 如果以上步驟都成功，可在openssl看到out32、out32dll資料夾產生，裡面openssl.exe、libeay32.dll、libeay32.lib、ssleay32.dll、ssleay32.lib

	- 動態庫編譯檔案輸出目錄\openssl\out32dll
	- 靜態庫編譯檔案輸出目錄\openssl\out32


----------

## [Openssl.exe指令] ##

IV key產生

    openssl.exe rand -out iv.bin 16

使用aes-128-cbc方法，對指定檔案進行加密

    openssl.exe aes-128-cbc -nopad -K <CEK key> -iv <IV key> -in <指定檔案> -out <輸出檔案>



----------

## [Q & A] ##

Q：當移出openssl.exe至其他資料夾執行時，若出現以下訊息，該如何處理?

    WARNING: can't open config file: /usr/local/ssl/openssl.cnf


A：重新指定OPENSSL_CONF路徑，將openssl.cnf所在的路徑，存入環境變數OPENSSL_CONF即可。

    set OPENSSL_CONF=%CD%\openssl.cnf
