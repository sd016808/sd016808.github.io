---
layout: post
title: 'C# 執行 Outlook 並且把檔案加入附件'
author: 'James Peng'
tags: ['C# 3rd-Party Library']
---

## C# 執行 Outlook 並且把檔案加入附件 ##

~~~csharp
        public static void ToOutlook(string MailTo, string subject, string filePath)
        {
            Microsoft.Win32.RegistryKey rKey = Microsoft.Win32.Registry.ClassesRoot.OpenSubKey(@"mailto\shell\open\command");
            if (rKey != null)
            {
                string path = rKey.GetValue("").ToString() + " ";
                path = path.Substring(0, path.IndexOf(" "));
                path = path.Replace("\"", "");
                rKey.Close();
                try
                {
                    System.Diagnostics.Process.Start("\"" + path + "\"", " /c ipm.note /m " + MailTo + "&subject=" + subject + " /a \"" + filePath + "\"");
                }
                catch
                {
                }
            }
        }
~~~



