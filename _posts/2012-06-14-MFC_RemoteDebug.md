---
layout: post
title: 'Remote Debug 設定'
author: 'James Peng'
tags: ['Visual Studio']
---

# Remote Debug 設定 #

裝有Visual C++ 6.0/Visual Studio 2010者，稱Host電腦.

被Remote Debug者，稱Remote電腦.


----------


## 前置作業 Step 1. ##

將Remote電腦新增/修改為有密碼之使用者帳戶. (遠端連線需設密碼)

![](..\images\2012-06-14-MFC_RemoteDebug\VDSgD3J.png)



----------


##  前置作業 Step 2. ##

連接Host、Remote電腦，即兩台電腦直接由網路線連接.


----------


## 前置作業 Step 3. ##

於Remote電腦C槽新增進行Debug工作的資料夾(ex. RemoteDbg)，並將其資料夾設置共用. 

操作 : 

右鍵/內容/共用/進階共用:共用此資料夾打勾，點選權限並開啟Everyone權限.

允許完全控制

變更


![](..\images\2012-06-14-MFC_RemoteDebug\04fdhTt.png)


----------

## 前置作業 Step 4. ##

Host電腦於 我的電腦/網路 查看是否有看到Remote電腦所開啟的共用資料夾，若有請將其設置為網路磁碟機 (右鍵/連接網路磁碟機)，屆時會指定此磁碟機名稱(ex. Z:)，設置完成後可在【我的電腦】看到此磁碟機.

![](..\images\2012-06-14-MFC_RemoteDebug\05SzlIT.png)

![](..\images\2012-06-14-MFC_RemoteDebug\FhMSsHv.png)


----------

## 前置作業 Step 5. ##

將Remote Debug之程式執行檔(ex. forRemote.exe)放入此網路磁碟機，即放置於Remote電腦進行Debug工作的資料夾中.

----------


## 前置作業 Step 6-a. (Visual Studio 2010適用) ##


複製Visual Studio Remote Debugging Monitor 【註1】於Remote電腦，開啟並在Tools/Options中勾選 No Authentication (native only)與Allow any user to debug.

![](..\images\2012-06-14-MFC_RemoteDebug\aoZuAEb.png)

註1：Visual Studio Remote Debugging monitor可在Host電腦找到，通常放置於 Program Files\Microsoft Visual Studio 10.0\Common7\IDE\Remote Debugger中，再依作業系統選擇適合的版本，例如win7 32bit則使用x86.



----------

## 前置作業 Step 6-b. (Visual C++ 6.0適用) ##

複製Visual C++ Debug Monitor等檔案【註2】於Remote電腦，開啟並在Settings/Target machine name 設定Host電腦之電腦名稱。於程式Debug前執行Connect即可.

![](..\images\2012-06-14-MFC_RemoteDebug\US6dwkz.png)

註2：Visual C++ Debug Monitor等檔案，通常放置於

Program Files\Microsoft Visual Studio\Common\MSDev98\Bin中，複製六個檔案.

1. MSVCMON.EXE
2. DM.DLL
3. MSDIS110.DLL
4. msvcp60.DLL
5. psapi.DLL
6. TLN0T.DLL


----------

# Project Properties Setting #


## 【Visual Studio 2010】 Step 1. ##

位置：Project/Properties 

Configuration Properties/General/Output Directory設定為網路磁碟機(ex. Z:)

![](..\images\2012-06-14-MFC_RemoteDebug\vH6Qfz0.png)


----------

## 【Visual Studio 2010】 Step 2. ##

位置：Configuration Properties/Debugging

選擇【Remote Windows Debugger】

參數設定如下:

- Remote Command 	設置程式執行`檔的絕對位置 (以Remote電腦的角度) 例如 C:\Remotedbg\forRemote.exe
- Working Directory 	設置Remote電腦之共用資料夾路徑 (以Remote電腦的角度) 例如 C:\Remotedbg
- Remote Server Name 	設置Remote電腦之【電腦名稱】 例如 B-PC
- Connection			點選Remote with no authentication (Native only)

![](..\images\2012-06-14-MFC_RemoteDebug\Q9dDWIg.png)


----------


## 【Visual Studio 2010】 Step 3. ##

按F5 Start Debugging 。第一次建置專案Remote電腦可能會提示缺少某些DLL檔(ex. mfc100d.dll、msvcr100d.dll)，請在Host電腦system32資料夾中找，並複製到Remote電腦system32資料夾中；另外，Remote電腦的防火牆可能會影響debug的進行，若有提示再關掉即可.


----------

## 【Visual C++ 6.0】 Step 1. ##

位置：Build/Debugger Remote Connection
Connection項目選擇 Network(TCP/IP)，並在Settings/Target machine name欄位填入Remote電腦之電腦名稱.(ex. B-PC)

![](..\images\2012-06-14-MFC_RemoteDebug\j64qx6U.png)


----------

## 【Visual C++ 6.0】 Step 2. ##

位置：Project/Settings/Debug

參數設定如下:

- Executable for debug session 		設置程式執行檔的位置 (以Host電腦角度)	例如 Z:\forRemote.exe
- Working directory					設置共用資料夾的位置 (以Remote電腦角度) 	例如 C:\Remotedbg
- Remote executable path and file name	設置程式執行檔的絕對位置 (以Remote電腦角度) 	例如 C:\Remotedbg\forRemote.exe
- Project/Settings/Link Output file name				設置Debug程式執行檔的位置 (以Host電腦角度)	例如 Z:\forRemote.exe

![](..\images\2012-06-14-MFC_RemoteDebug\TVaZist.png)


----------

## 【Visual C++ 6.0】 Step 3. ##

按F5 Start Debugging 。第一次建置專案Remote電腦可能會提示缺少某些DLL檔(ex. mfc100d.dll、msvcr100d.dll)，請在Host電腦system32資料夾中找，並複製到Remote電腦system32資料夾中；另外，Remote電腦的防火牆可能會影響debug的進行，若有提示再關掉即可.


----------

## Remote Debug與DLL相關問題  ##

進行Remote Debug時，當程式有使用到其他額外的DLL檔，則需將此DLL放置於同一資料夾中(Remote電腦)；此外，若出現LoadLibrary抓不到此DLL檔的情況，可能是發生DLL Dependency問題，也就是使用此DLL檔亦需要其他DLL檔才能有效執行，而Remote電腦中並沒有這些DLL檔，導致程式無法正常執行.

解決方法：

可以利用Visual C++ 6.0 Tools中的【Dependency Walker】來查詢DLL檔間的相依性質，通常放置於Program Files\Microsoft Visual Studio\Common\Tools\DEPENDS.EXE 

操作：

將【Dependency Walker】放置於Remote電腦中，開啟欲使用的DLL檔來查詢相依性，其中有警告標示則代表缺少的相依DLL檔，可於Host電腦的system32資料夾中搜尋，並複製於Remote電腦的System32資料夾.

例如 程式需要使用MyRemote.dll，透過Dependency Walker載入，可以查詢缺少的相依DLL檔，在此例為MFC100.dll，如下圖所示：

![](..\images\2012-06-14-MFC_RemoteDebug\qwDiyOM.png)


----------


## How to set up remote debugging quickly by using Visual C++ 5.0 or Visual C++ 6.0 ##

http://support.microsoft.com/?scid=kb%3Ben-us%3B241848&x=11&y=9

This article was previously published under Q241848

## SUMMARY ##

This article describes how to set up remote debugging quickly when you use Microsoft Visual C++ 5.0 or Microsoft Visual C++ 6.0 to debug applications on non-development computers.

## MORE INFORMATION ##

Note In these steps, the target computer is the computer that does not have Visual C++ installed on it, and it is the computer that is usually called the "clean" computer. The host computer is the computer on which Visual C++ is installed where the debugging with the integrated development environment (IDE) occurs.

Make sure that both computers can see each other on the network. This makes it easier than setting up a serial connection between the two and makes the remote debugging much faster.

On the target computer, create a share so that the host computer can access it. For example, it debug on the host like such as \\server\share\somedll.dll.

Copy the following files to the target computer share created or the system folder. On a computer that is running Microsoft Windows 95, Microsoft Windows 98, Microsoft Windows NT 4.0, or Microsoft Windows 2000, the remote debug monitor consists of the following files:

	- o Msvcmon.exe
	- o Msvcrt.dll
	- o Tln0t.dll
	- o Dm.dll
	- o Msvcp6o.dll
	- o Msdis110.dll
	

Note In Windows NT, the remote debugger also requires the Psapi.dll file. The files are located in the following folders:

<table>
<tbody><tr>
<td style="width:116px; height:26px; border-style:solid; border-color:#000000; border-width:1px 1px 1px 1px">
<span style="font-size:12pt; color:#000000">File </span></td>
<td style="width:405px; height:26px; border-style:solid; border-color:#000000; border-width:1px 1px 1px 1px">
<span style="font-size:12pt; color:#000000">Copy from location </span></td>
</tr>
<tr>
<td style="width:116px; height:25px; border-style:solid; border-color:#000000; border-width:1px">
<span style="font-size:12pt; font-weight:normal; color:#000000">Msvcmon.exe </span></td>
<td style="width:405px; height:25px; border-style:solid; border-color:#000000; border-width:1px">
<span style="font-size:12pt; font-weight:normal; color:#000000">\Common\Msdev98\Bin </span></td>
</tr>
<tr>
<td style="width:116px; height:25px; border-style:solid; border-color:#000000; border-width:1px">
<span style="font-size:12pt; font-weight:normal; color:#000000">Tln0t.dll </span></td>
<td style="width:405px; height:25px; border-style:solid; border-color:#000000; border-width:1px">
<span style="font-size:12pt; font-weight:normal; color:#000000">\Common\Msdev98\Bin </span></td>
</tr>
<tr>
<td style="width:116px; height:25px; border-style:solid; border-color:#000000; border-width:1px">
<span style="font-size:12pt; font-weight:normal; color:#000000">Dm.dll </span></td>
<td style="width:405px; height:25px; border-style:solid; border-color:#000000; border-width:1px">
<span style="font-size:12pt; font-weight:normal; color:#000000">\Common\Msdev98\Bin </span></td>
</tr>
<tr>
<td style="width:116px; height:25px; border-style:solid; border-color:#000000; border-width:1px">
<span style="font-size:12pt; font-weight:normal; color:#000000">Msdis110.dll </span></td>
<td style="width:405px; height:25px; border-style:solid; border-color:#000000; border-width:1px">
<span style="font-size:12pt; font-weight:normal; color:#000000">\Common\Msdev98\Bin </span></td>
</tr>
<tr>
<td style="width:116px; height:25px; border-style:solid; border-color:#000000; border-width:1px">
<span style="font-size:12pt; font-weight:normal; color:#000000">Msvcrt.dll </span></td>
<td style="width:405px; height:25px; border-style:solid; border-color:#000000; border-width:1px">
<span style="font-size:12pt; font-weight:normal; color:#000000">%SYSTEMROOT%\System(32) </span></td>
</tr>
<tr>
<td style="width:116px; height:49px; border-style:solid; border-color:#000000; border-width:1px">
<span style="font-size:12pt; font-weight:normal; color:#000000">Msvcp60.dll </span></td>
<td style="width:405px; height:49px; border-style:solid; border-color:#000000; border-width:1px">
<span style="font-size:12pt; font-weight:normal; color:#000000">%SYSTEMROOT%\System(32) (Msvcp50.dll for VC++ 5.0) </span></td>
</tr>
<tr>
<td style="width:116px; height:73px; border-style:solid; border-color:#000000; border-width:1px 1px 1px 1px">
<span style="font-size:12pt; font-weight:normal; color:#000000">Psapi.dll </span></td>
<td style="vertical-align:top; width:405px; height:73px; border-style:solid; border-color:#000000; border-width:1px 1px 1px 1px">
<span style="font-size:12pt; font-weight:normal; color:#000000">%SYSTEMROOT%\System32 (Do not copy this file to Windows 95-based computer or Windows 98-based computers.) </span></td>
</tr>
</tbody></table>

ote If the Msvcrt.dll file is on the host computer and is an earlier version than the version of the Msvcrt.dll file that is on the target computer, do not copy the Msvcrt.dll file to the target. If the Msvcrt.dll file is copied to the target, restart the target computer because the Msvcrt.dll file is a known DLL.

Copy your DLLs and .exe files to the share. Be sure to copy any third-party information that you may need. Also copy any dependent DLLs such as Mfc42d.dll and Msvcrtd.dll to the share. Also, make sure all that all COM DLL and .exe files being used by your program are registered on the target.

Copy the .pdb files to the share. This includes all of the .pdb files in the debug or release folders for the project on the host computer. If you do not have any .pdb files because you are doing release builds, you can add debug info to the release project. To do this, follow these steps:

- On the Project menu, click Settings.
- Click the C/C++ tab.
- Click Category, and then click General.
- Change Debug info from None to Program Database.
- Click the Link tab.
- Click Category, and then click General.
- Click to select the Generate Debug Info check box.
- Rebuild and run.

On the host computer, the one with the source code, change the debug target in the Remote executable path and file name box in the Project Settings Debug tab to reflect the DLL/EXE that is to be debugged.
For example: \\server\share\somedll.dll

Example 2: You may link \\server\share\ as a Net Disk, say Y:\, And the shared folder in the server (target) is:
c:\users\stdsid\desktop\redshare\ The program is ReD2.exe


![](..\images\2012-06-14-MFC_RemoteDebug\gY7mduo.png)

and

![](..\images\2012-06-14-MFC_RemoteDebug\KORglm6.png)

In the Visual C++ IDE, click Debugger Remote Connection on the Build menu select . Change it from Local to Remote. Click Settings, and then change the Target machine name or address. You can also give an IP address instead.

Set breakpoints in the source code that you want to debug. Do this before you start the remote monitor.

On the target computer, run Msvcmon.exe. This has to be running before you try to connect. Click Settings, and then change the Target machine name or address to the host computer name. You can also give an IP address instead. Always be sure that the target has this running before wondering why the debugger is not working.

You are ready to debug now. On the host computer, click Start Debug on the Build menu, and then click Go.

![](..\images\2012-06-14-MFC_RemoteDebug\pwEwG3W.png)

In most cases, you can just uncheck the “Try to locate other DLLs” and press “Cancel”.

![](..\images\2012-06-14-MFC_RemoteDebug\9rg9UCX.png)


----------

## REFERENCES ##

For more information, click the following article number to view the article in the
Microsoft Knowledge Base: 131058 (http://support.microsoft.com/kb/131058/) Descriptions of tips for remote debugging with Visual C++ versions 2.x, 4.0, 5.0, and 6.0
Search for "Debugging Remote Applications" in the Visual C++ Programmer's Guide.


----------


## APPLIES TO ##

- Microsoft Visual C++, 32-bit Learning Edition 6.0
- Microsoft Visual C++ 5.0 Enterprise Edition
- Microsoft Visual C++ 6.0 Enterprise Edition
- Microsoft Visual C++ 5.0 Professional Edition
- Microsoft Visual C++ 6.0 Professional Edition

Keywords: kbhowto kbbug kbdebug KB241848


----------

## WinPE Settings: ##

- On the WinPE machine (target), for example: create a folder ReDebug on disk X:\.
- Copy the DLLs, the EXEs and the PDB into it (use USB drive).
- Be sure that, disable the firewall on the WinPE machine using the command: x:\>wpeutil disablefirewall
- On the host, don’t forget to “change Debugger Remote Connection from Local to Remote. Click Settings, and then change the Target machine name or address. You can also give an IP address instead.

Now you can run the debugging process.

![](..\images\2012-06-14-MFC_RemoteDebug\gCSMP2e.png)


----------

## Share Folder for the Target ##

Host computer shares a folder and target computer links the folder as a local drive.

![](..\images\2012-06-14-MFC_RemoteDebug\OgeKJIH.png)

User of Host machine: Tom_Lee on the domain compal On the target machine please type:

![](..\images\2012-06-14-MFC_RemoteDebug\TrN0XT7.png)

Then the program will ask you type the password for ‘compal\Tom_Lee’ to connect to ‘Tom_Lee2’: If success: it says:
The command completed successfully.
Ortherwise:

![](..\images\2012-06-14-MFC_RemoteDebug\UozIjrB.png)

Figure A.

![](..\images\2012-06-14-MFC_RemoteDebug\Sqy3A4J.png)
