---
layout: post
title: 'C# 移除檔案的唯讀屬性'
author: 'James Peng'
tags: ['C#']
---

![](..\images\2016-05-09-CSharp_SetAttributes\8SV9TS6.png)

##  起因  ##

在操作檔案 COPY DELETE 時 出現 Access is Denied 錯誤訊息: 

![](..\images\2016-05-09-CSharp_SetAttributes\AdnNoiU.png)

----------


## 解決辦法 ##

I also had the problem, hence me stumbling on this post. I added the following line of code before and after a Copy / Delete.

## Delete ##

~~~csharp
File.SetAttributes(file, FileAttributes.Normal);
File.Delete(file);
~~~


## Copy ##

~~~csharp
File.Copy(file, dest, true);
File.SetAttributes(dest, FileAttributes.Normal);
~~~


----------

## 檔案屬性操作 ##

(1)取得檔案屬性

~~~csharp
string filePath = @"c:\test.txt"; 
FileAttributes fileAttributes = File.GetAttributes(filePath);
~~~


(2)設定檔案屬性

~~~csharp
// 清除所有檔案屬性
File.SetAttributes(filePath, FileAttributes.Normal);

// 只設定封存及唯讀屬性
File.SetAttributes(filePath, FileAttributes.Archive | FileAttributes.ReadOnly);
~~~


(3)檢查檔案屬性

~~~csharp
// 檢查檔案是否有唯讀屬性
bool isReadOnly = ((File.GetAttributes(filePath) & FileAttributes.ReadOnly) == FileAttributes.ReadOnly);

// 檢查檔案是否有隱藏屬性
bool isHidden = ((File.GetAttributes(filePath) & FileAttributes.Hidden) == FileAttributes.Hidden);

// 檢查檔案是否有封存屬性
bool isArchive = ((File.GetAttributes(filePath) & FileAttributes.Archive) == FileAttributes.Archive);

// 檢查檔案是否有系統屬性
bool isSystem = ((File.GetAttributes(filePath) & FileAttributes.System) == FileAttributes.System);
~~~



(4)加入某些屬性給檔案

~~~csharp
// 設定隱藏屬性   
File.SetAttributes(filePath, File.GetAttributes(filePath) | FileAttributes.Hidden);

// 設定封存及唯讀屬性  
File.SetAttributes(filePath, File.GetAttributes(filePath) | (FileAttributes.Archive | FileAttributes.ReadOnly));
~~~

----------

參考：

- http://stackoverflow.com/questions/26577038/deleting-file-but-is-access-denied
- http://fecbob.pixnet.net/blog/post/39098073-%E7%A7%BB%E9%99%A4%E6%AA%94%E6%A1%88%E7%9A%84%E5%94%AF%E8%AE%80%E5%B1%AC%E6%80%A7
- http://deepbrainstudio.blogspot.tw/2009/12/blog-post.html
