---
layout: post
title: 'C# 取得硬碟序號 SerialNumber '
author: 'James Peng'
tags: ['C#']
---


## 先宣告 ##

~~~csharp
        [DllImport("kernel32.dll")]
        private static extern long GetVolumeInformation(
            string PathName,
            StringBuilder VolumeNameBuffer,
            UInt32 VolumeNameSize,
            ref UInt32 VolumeSerialNumber,
            ref UInt32 MaximumComponentLength,
            ref UInt32 FileSystemFlags,
            StringBuilder FileSystemNameBuffer,
            UInt32 FileSystemNameSize);
~~~

## getDiskSerialNumber() ##

~~~csharp
        private static string getDiskSerialNumber()
        {
            uint serial_number = 0;
            uint max_component_length = 0;
            StringBuilder sb_volume_name = new StringBuilder(256);
            UInt32 file_system_flags = new UInt32();
            StringBuilder sb_file_system_name = new StringBuilder(256);

            GetVolumeInformation("C:\\", sb_volume_name,
                (UInt32)sb_volume_name.Capacity, ref serial_number,
                ref max_component_length, ref file_system_flags,
                sb_file_system_name,
                (UInt32)sb_file_system_name.Capacity);

            return serial_number.ToString();
        }       
~~~

----------

## 用途 ##

- 拿來改一改可以變 軟體註冊碼
