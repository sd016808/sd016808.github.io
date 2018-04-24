---
layout: post
title: 'C# 避免應用程式檔案名稱被修改'
author: 'James Peng'
tags: ['C#']
---

在開發 Windows Form 的時候，有時會需要限制使用者不能隨意修改exe檔名。

## 先宣告 ##

- 要改成與執行檔檔名一樣(大寫,，副檔名為EXE)

~~~csharp
 public const string PROGRAM_FILE_NAME = "PROGRAM_FILE_NAME.EXE";
~~~


----------

## 用法 ##

- 在 Program.cs 應用程式的主要進入點 Main() 裡，加入以下code：

~~~csharp
                //avoid ProgramFileName.exe  was changed.
                string moduleName = Process.GetCurrentProcess().MainModule.FileName.ToUpper();
                moduleName = Path.GetFileName(moduleName);
                moduleName = moduleName.Replace(".VSHOST",""); //for visual studio debug
                if (moduleName != PROGRAM_FILE_NAME)
                {
                    MessageBox.Show("Program file name is incorrect!");
                }          
~~~
