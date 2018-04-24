---
layout: post
title: 'C# 讀寫 INI'
author: 'James Peng'
tags: ['C#']
---

## INI取值 ##

~~~csharp
        [DllImport("kernel32")]
        private static extern int GetPrivateProfileString(string section, string key, string def, StringBuilder retVal, int size, string filePath);

        public static string GetKeyValue(string INI_NAME, string strSection, string strKey)
        {
            if (_FilePath == string.Empty) SetIniFileName(INI_NAME);

            StringBuilder temp = new StringBuilder(255);
            int i = GetPrivateProfileString(strSection, strKey, "", temp, 255, _FilePath);
            return temp.ToString();
        }



        public static string GetKeyValue(string INI_NAME, string Section, string Key, string DefaultValue)
        {
            if (_FilePath == string.Empty) SetIniFileName(INI_NAME);

            StringBuilder sbResult = null;
            try
            {
                sbResult = new StringBuilder(255);
                GetPrivateProfileString(Section, Key, "", sbResult, 255, _FilePath);
                return (sbResult.Length > 0) ? sbResult.ToString() : DefaultValue;
            }
            catch
            {
                return string.Empty;
            }
        }
~~~


----------


## INI寫值 ##

~~~csharp
        [DllImport("kernel32")]
        private static extern long WritePrivateProfileString(string section, string key, string val, string filePath);

        public static void SetKeyValue(string INI_NAME, string strSection, string strKey, string strValue)
        {
            if (_FilePath == string.Empty) SetIniFileName(INI_NAME);

            WritePrivateProfileString(strSection, strKey, strValue, _FilePath);
        }
~~~

