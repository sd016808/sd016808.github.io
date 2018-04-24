---
layout: post
title: 'C# 錯誤	643	兩個輸出檔名解析成相同的輸出路徑: "obj\Debug\xxxForm.resources"
'
author: 'James Peng'
tags: ['C#']
---

# 問題： 錯誤	643	兩個輸出檔名解析成相同的輸出路徑 #


Two output file names resolved to the same output path: "obj\x86\Debug\xxxx.resources" DryWash


![](..\images\2016-04-18-CSharp_Two_output_filenames_resolved_same_output\40qBPbh.png)

----------


# 解答 #


您可以這樣做:

remove all .resx files, next build it to test.

![](..\images\2016-04-18-CSharp_Two_output_filenames_resolved_same_output\UwYi29Y.png)

或是 看是否有同名文件但不同資料夾

----------

參考：

- https://social.msdn.microsoft.com/Forums/zh-TW/44faa617-4de0-4249-b0bd-c02297210399/two-output-file-names-resolved-to-the-same-output-path-error?forum=vbgeneral
