---
layout: post
title: 'C# 設置資料夾許可權'
author: 'James Peng'
tags: ['Refactoring']
---

對資料夾設置為Everyone的許可權，首先需要先增加參考

using System.Security.AccessControl;

System.Security.AccessControl 命名空間提供控制存取和稽核安全物件上的安全性相關動作的程式設計項目。

採用下面的方法對資料夾設置Everyone許可權

**Solution**

~~~csharp
        /// <summary>
        /// 設置資料夾許可權，處理為Everyone擁有權限
        /// </summary>
        /// <param name="folderPath">資料夾路徑</param>
        public static void SetFileRole(string folderPath)
        {
            DirectorySecurity folderSecurity = new DirectorySecurity();
            folderSecurity.AddAccessRule(new FileSystemAccessRule("Everyone", FileSystemRights.FullControl,
            InheritanceFlags.ContainerInherit | InheritanceFlags.ObjectInherit, PropagationFlags.None, AccessControlType.Allow));
            System.IO.Directory.SetAccessControl(folderPath, folderSecurity);
        }
~~~

----------

參考:

- http://fecbob.pixnet.net/blog/post/39098077-c%23%E8%A8%AD%E7%BD%AE%E8%B3%87%E6%96%99%E5%A4%BE%E8%A8%B1%E5%8F%AF%E6%AC%8A