---
layout: post
title: 'C# 使用 NLog'
author: 'James Peng'
tags: ['C# 3rd-Party Library']
---

NLog 是一種開放原始碼的的日誌紀錄程式元件，可以結合到 C# 開發的專案中，追蹤程式運行的異常情況。

NLog 的功能相當齊全，設定也相當簡單、容易上手，所以接下來將會介紹如何在網站專案中安裝與設定 NLog。


----------

## 安裝 ##

![](..\images\2016-05-15-CSharp_NLog\fpG13ew.png)

開啟 套件管理主控台

![](..\images\2016-05-15-CSharp_NLog\FWkn8Ps.png)

輸入:

    Install-Package NLog.Config


![](..\images\2016-05-15-CSharp_NLog\6owB9QO.png)

按下 ENTER 會開始安裝

![](..\images\2016-05-15-CSharp_NLog\IeuIFZz.png)

會多出這些檔案

----------

## 配置檔設定 ##

開啟 NLog.config

![](..\images\2016-05-15-CSharp_NLog\GCso9iu.png)

一般來說，只要設定兩個地方就好，分別是 「targets」 跟 「rules」

![](..\images\2016-05-15-CSharp_NLog\UV6eCum.png)

## targets ##
 
~~~xml
  <targets>
    <target name="file" xsi:type="File"
          layout="${longdate} | ${level:uppercase=true} | ${callsite:methodName=true} | ${message} ${onexception:${newline}${exception:format=tostring}} ${newline}"
          fileName="${basedir}/Logs/logfile.txt"
          archiveFileName="${basedir}/Logs/archives/log.{#}.txt"
          archiveEvery="Day"
          archiveNumbering="Rolling"
          maxArchiveFiles="7"
          concurrentWrites="true"
          keepFileOpen="false"
          encoding="UTF-8" />
  </targets>
~~~

## rules ##

~~~xml
  <rules>
    <logger name="*" minlevel="Trace" writeTo="file" />
  </rules>
~~~

----------

## 使用範例 ##

~~~csharp
using NLog;
using System;
using System.Windows.Forms;

namespace TestNLog
{
    public partial class Form1 : Form
    {
        private static Logger logger = LogManager.GetCurrentClassLogger();

        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            logger.Fatal("This is fatal msg");
            logger.Error("This is error msg");
            logger.Warn("This is warn msg");
            logger.Info("This is info msg");
            logger.Debug("This is debug msg");
            logger.Trace("This is trace msg");
        }
    }
}
~~~


----------



## 產生結果: ##

~~~text
2016-05-18 10:33:49.4673 | FATAL | TestNLog.Form1.button1_Click | This is fatal msg  

2016-05-18 10:33:49.5443 | ERROR | TestNLog.Form1.button1_Click | This is error msg  

2016-05-18 10:33:49.5443 | WARN | TestNLog.Form1.button1_Click | This is warn msg  

2016-05-18 10:33:49.5443 | INFO | TestNLog.Form1.button1_Click | This is info msg  

2016-05-18 10:33:49.5443 | DEBUG | TestNLog.Form1.button1_Click | This is debug msg  

2016-05-18 10:33:49.5443 | TRACE | TestNLog.Form1.button1_Click | This is trace msg  

~~~


----------

## 參考: ##

- https://www.nuget.org/packages/NLog.Config
- http://nlog-project.org/download/
- http://iverson127.github.io/NLog_Install/
- http://iverson127.github.io/NLog_Config/
- http://iverson127.github.io/NLog_Example(1)/
- http://blog.miniasp.com/post/2010/07/18/Useful-Library-NLog-Advanced-NET-Logging.aspx
- 
