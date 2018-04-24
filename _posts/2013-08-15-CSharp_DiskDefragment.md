---
layout: post
title: 'C# 停用 關閉 Disk Defragment 透過 command line'
author: 'James Peng'
tags: ['C# 3rd-Party Library']
---

## 確保 Windows 裡有安裝 “Disk Defragment” ##

![](..\images\2013-08-15-CSharp_DiskDefragment\H4CVcNl.jpg)

See if it will work if you specify a full path: 

~~~csharp
System.Diagnostics.Process.Start(@"c:\windows\system32\dfrgui.exe");
~~~

![](..\images\2013-08-15-CSharp_DiskDefragment\Ygyeyge.jpg)


----------

## 改登陸檔 關閉 disk defragmentation 以及 auto-layout ##

Add the following registry keys to your run-time image:

Disable Background disk defragmentation:

- Key Name:HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Dfrg\BootOptimizeFunction\ 
- Name: Enable 
- Type: REG_SZ 
- Value: N

Disable Background auto-layout:

- Key Name: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\OptimalLayout             
- Value Name: EnableAutoLayout 
- Type: REG_DWORD 
- Value: 0

For more information about how to add this registry key to your configuration, see Adding Registry Data to a Configuration in Windows XP Embedded Studio Help.


----------


## C#.NET 程式碼來修改登錄機碼 ##

1)   using Microsoft.Win32 命名空間 

2)   使用 Registry 物件的 Getvalue 及 SetValue 方法 

3)    範例: 寫入資料到 Registry

~~~csharp
           // 寫入單一值 
            string sDir = "HKEY_LOCAL_MACHINE\\SOFTWARE\\SONY\\JVC";
            Registry.SetValue(sDir, "Developer", "MEGA", RegistryValueKind.String);
            // 寫入字串陣列值 
            string[] ss = { "Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"};
            Registry.SetValue(sDir, "TestArray", ss, RegistryValueKind.MultiString);
           // 寫入整數資料 
            Registry.SetValue(sDir, "TestInt", 10, RegistryValueKind.DWord);
~~~

4)    範例: 讀取 Registry 

~~~csharp
            Object readvalue; 
            readvalue = Registry.GetValue("HKEY_LOCAL_MACHINE\\SOFTWARE\\HOPA\\UL123 Contract", "aa","無資料"); 
            MessageBox.Show(readvalue.ToString());
~~~

![](..\images\2013-08-15-CSharp_DiskDefragment\jQtljz7.jpg)

![](..\images\2013-08-15-CSharp_DiskDefragment\eZazROQ.jpg)

----------

## 用sc指令來關閉 Disk Defragmenter ##

    Startup type : Windows Service 
    Service Name : defragsvc 
    Display Name : Disk Defragmenter 
    Dll path : C:\Windows\System32\defragsvc.dll 
    Image path : C:\Windows\system32\svchost.exe -k defragsvc 
    Current Status : Manual/Stopped


How to disable this service.

~~~text
sc stop "defragsvc"
~~~

How to delete this service. 

~~~text
sc delete "defragsvc"
~~~

![](..\images\2013-08-15-CSharp_DiskDefragment\2iaxp75.jpg)

~~~text
C:\Users\JamesJH_Peng&gt;sc /?
錯誤:  無法辨識的命令
描述:
        SC 是一個用來和服務控制管理員及服務溝通的命令列程式。
使用方式:
        sc &lt;server&gt; [command] [service name] &lt;option1&gt; &lt;option2&gt;...
        &lt;server&gt; 選項的格式是 "\\ServerName"
命令的進一步說明可經由輸入 "sc [command]" 取得
命令:
          query-----------查詢服務的狀態，或列舉服務類型的狀態。
          queryex---------查詢服務的延伸狀態，或列舉服務類型的狀態。
          start-----------啟動服務。
          pause-----------傳送 PAUSE 控制要求到服務。
          interrogate-----傳送 INTERROGATE 控制要求到服務。
          continue--------傳送 CONTINUE 控制要求到服務。
          stop------------傳送 STOP 要求到服務。
          config----------變更服務 (持續) 的設定。
          description-----變更服務的描述。
          failure---------變更失敗時服務執行的動作。
          failureflag-----變更服務的失敗動作旗標。
          sidtype---------變更服務的服務 SID 類型。
          privs-----------變更服務的必要權限。
          qc--------------查詢服務的設定資訊。
          qdescription----查詢服務的描述。
          qfailure--------查詢失敗時服務執行的動作。
          qfailureflag----查詢服務的失敗動作旗標。
          qsidtype--------查詢服務的服務 SID 類型。
          qprivs----------查詢服務的必要權限。
          qtriggerinfo----查詢服務的觸發程序參數。
          qpreferrednode--查詢服務的慣用 NUMA 節點。
          delete----------刪除服務 (從登錄中)。
          create----------建立服務 (新增到登錄中)。
          control---------傳送控制到服務。
          sdshow----------顯示服務的安全性描述元。
          sdset-----------設定服務的安全性描述元。
          showsid---------顯示對應至任意名稱的服務 SID 字串。
          triggerinfo-----設定服務的觸發程序參數。
          preferrednode---設定服務的慣用 NUMA 節點。
          GetDisplayName--取得服務的 DisplayName。
          GetKeyName------取得服務的 ServiceKeyName。
          EnumDepend------列舉服務依存性。
下列命令不要求服務名稱:
        sc &lt;server&gt; &lt;command&gt; &lt;option&gt;
          boot------------(ok | bad) 表示是否要將上次開機儲存為
上次正確的開機設定
          Lock------------鎖定服務資料庫
          QueryLock-------查詢 SCManager 資料庫的 LockStatus
範例:
        sc start MyService
您是否要參閱 QUERY 與 QUERYEX 命令的說明? [ y | n ]:
~~~


----------

## 用schtasks 指令關閉排程 ##

Disable Disk Defragmenter Service:

1.RunWait 

~~~text
"schtasks /change /tn ""microsoft\windows\defrag\ScheduledDefrag"" /disable" 
~~~

2.oShell.RegWrite 

~~~text
"HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Dfrg\BootOptimizeFunction\Enable", "N", "REG_SZ" 
~~~

RunWait 

~~~text
"sc config defragsvc start= disabled"
~~~

實驗：

~~~text
schtasks /change /tn ""microsoft\windows\defrag\ScheduledDefrag"" /disable
~~~

![](..\images\2013-08-15-CSharp_DiskDefragment\AMXTmrq.jpg)

![](..\images\2013-08-15-CSharp_DiskDefragment\4YhmIlJ.jpg)

~~~text
schtasks /change /tn ""microsoft\windows\defrag\ScheduledDefrag"" /enable
~~~

![](..\images\2013-08-15-CSharp_DiskDefragment\Eyfp41a.jpg)


![](..\images\2013-08-15-CSharp_DiskDefragment\m3hY6iz.jpg)

----------

## 用sc指令方式 關閉 排程  ##


~~~text
sc config defragsvc start= disabled
~~~

![](..\images\2013-08-15-CSharp_DiskDefragment\yYJORcl.jpg)

----------


參考：

- http://www.c-sharpcorner.com/Forums/Thread/189380/how-to-open-disk-defrag-in-C-Sharp-net.aspx
- http://msdn.microsoft.com/en-us/library/ms932871(v=winembedded.5).aspx
- http://windowsvc.com/bbs/board.php?bo_table=windowsvc&wr_id=129
- http://blogs.technet.com/b/jeff_stokes/archive/2012/10/18/deconstructing-the-pfe-vdi-optimization-script.aspx
- http://alen1985.pixnet.net/blog/post/27591860-c%23.net-%E7%A8%8B%E5%BC%8F%E7%A2%BC%E4%BE%86%E4%BF%AE%E6%94%B9%E7%99%BB%E9%8C%84%E6%A9%9F%E7%A2%BC 
- http://welkingunther.pixnet.net/blog/post/29309240-(c%23)%E4%BF%AE%E6%94%B9%E7%99%BB%E9%8C%84%E6%AA%94
 
