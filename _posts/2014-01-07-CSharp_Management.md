---
layout: post
title: 'C# 取得硬體資訊 cpu id 記憶體大小 磁碟id'
author: 'James Peng'
tags: ['C#']
---

## 引用 ##

![](..\images\2014-01-07-CSharp_Management\RP7n460.png)



~~~csharp
using System.Management;  
using System.Runtime.InteropServices;  
~~~


## Get Disk ID ##

~~~csharp

        private static string GetDiskID()
        {
            try
            {
                String HDid = "";
                ManagementClass mc = new ManagementClass("Win32_DiskDrive");
                ManagementObjectCollection moc = mc.GetInstances();
                foreach (ManagementObject mo in moc)
                {
                    HDid = (string)mo.Properties["Model"].Value;
                }
                moc = null;
                mc = null;
                return HDid;
            }
            catch
            {
                return "UNKNOW";
            }
        }  
~~~


----------


## Get Available Physical Memory ##

~~~csharp

        public struct MemoryStatus
        {
            public uint Length;
            public uint MemoryLoad;
            public uint TotalPhysical;
            public uint AvailablePhysical;
            public uint TotalPageFile;
            public uint AvailablePageFile;
            public uint TotalVirtual;
            public uint AvailableVirtual;
        }

        [DllImport("kernel32.dll")]
        public static extern void GlobalMemoryStatus(out MemoryStatus stat);

        private static string GetAvailablePhysicalMemory()
        {
            MemoryStatus stat = new MemoryStatus();
            GlobalMemoryStatus(out stat);
            long ram = (long)stat.AvailablePhysical;
            ram = (long)(ram / (long)1024 / (long)1024);
            return ram.ToString() + " MB";
        }
~~~


----------


## Get Total Physical Memory ##

~~~csharp
       private static string GetTotalPhysicalMemory()
        {
            try
            {
                string st = "";
                ManagementClass mc = new ManagementClass("Win32_ComputerSystem");
                ManagementObjectCollection moc = mc.GetInstances();
                Double mem=0;
                foreach (ManagementObject mo in moc)
                {                    
                    st = mo["TotalPhysicalMemory"].ToString();                   
                }
                mem = (Double)((Double)Convert.ToDouble(st) / (Double)1024 / (Double)1024);
                moc = null;
                mc = null;
                return mem.ToString() + " MB";
            }
            catch
            {
                return "UNKNOW";
            }
        }
~~~



----------


## Get Cpu ID ##

~~~csharp
        private static string GetCpuID()
        {
            try
            {
                string cpuInfo = "";  
                ManagementClass mc = new ManagementClass("Win32_Processor");
                ManagementObjectCollection moc = mc.GetInstances();
                foreach (ManagementObject mo in moc)
                {
                    cpuInfo = mo.Properties["ProcessorId"].Value.ToString();
                }
                moc = null;
                mc = null;
                return cpuInfo;
            }
            catch
            {
                return "UNKNOW";
            }
        }
~~~

----------

參考：

- memory : https://msdn.microsoft.com/zh-tw/library/ms172518(v=vs.90).aspx
