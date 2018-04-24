---
layout: post
title: 'API 函數 ShellExecute 與 ShellExecuteEx 用法'
author: 'James Peng'
tags: ['Visual C++']
---

## ShellExecute： ##

1.函數功能： 

你可以給它任何文件的名字，它都能識別出來並打開它。


----------


 
2．函數原型：

~~~cpp 
HINSTANCE ShellExecute( 
HWND hwnd, 
LPCTSTR lpOperation, 
LPCTSTR lpFile, 
LPCTSTR lpParameters, 
LPCTSTR lpDirectory, 
INT nShowCmd 
);  
~~~


----------


3．參數說明： 

- hwnd: 
用於指定父窗口句柄。當函數調用過程出現錯誤時，它將作為Windows消息窗口的父窗口。 

- lpOperation: 
用於指定要進行的操作。 
「open」操作表示執行由lpFile參數指定的程序，或打開由lpFile參數指定的文件或文件夾； 
「print」操作表示打印由lpFile參數指定的文件； 
「explore」操作表示瀏覽由lpFile參數指定的文件夾。 
當參數設為NULL時，表示執行默認操作「open」。  


- lpFile: 
用於指定要打開的文件名、要執行的程序文件名或要瀏覽的文件夾名。 

- lpParameters: 
若lpFile參數是一個可執行程序，則此參數指定命令行參數，否則此參數應為NULL. 

- lpDirectory: 
用於指定默認目錄. 

- nShowCmd: 
若lpFile參數是一個可執行程序，則此參數指定程序窗口的初始顯示方式，否則此參數應設置為0。 

這個參數常用的常數：
 
	- SW_HIDE 隱藏窗口，活動狀態給令一個窗口 
	- SW_MINIMIZE 最小化窗口，活動狀態給令一個窗口 
	- SW_RESTORE 用原來的大小和位置顯示一個窗口，同時令其進入活動狀態 
	- SW_SHOW 用當前的大小和位置顯示一個窗口，同時令其進入活動狀態 
	- SW_SHOWMAXIMIZED 最大化窗口，並將其激活 
	- SW_SHOWMINIMIZED 最小化窗口，並將其激活 
	- SW_SHOWMINNOACTIVE 最小化一個窗口，同時不改變活動窗口 
	- SW_SHOWNA 用當前的大小和位置顯示一個窗口，不改變活動窗口 
	- SW_SHOWNOACTIVATE 用最近的大小和位置顯示一個窗口，同時不改變活動窗口 
	- SW_SHOWNORMAL 與SW_RESTORE相同 
 
若ShellExecute函數調用成功，則返回值為被執行程序的實例句柄。若返回值小於32，則表示出現錯誤。  



----------


4.使用方法： 


例如： 

~~~cpp
ShellExecute(NULL,"open","iloveu.bmp",NULL,NULL,SW_SHOWNORMAL);
~~~
  
用預設的位圖編輯器打開一個叫iloveu.bmp的位圖文件，這個缺省的位圖編輯器可能是 Microsoft Paint, Adobe Photoshop, 或者 Corel PhotoPaint。 

這個函數能打開任何文件，甚至是桌面和URL快捷方式（ .ink或 .url）。ShellExecute解析系統註冊表HKEY_CLASSES_ROOT中所有的內容，判斷啟動那一個執行程序，並且啟動一個新的實例或使用DDE將文件名連到一打開的實例。

然後，ShellExecute 返回打開文件的應用的實例句柄。 

~~~cpp
ShellExecute(NULL, "open", "http://www.microsoft.com", NULL, NULL, SW_SHOWNORMAL);
~~~
  
這個代碼使你能訪問微軟的主頁。當ShellExecute遇到文件名前面的「http:」時，可以判斷出要打開的文件是Web文件，隨之啟動Internet Explorer 或者 Netscape Navigator 或者任何你使用的別的瀏覽器打開文件。 

ShellExecute還能識別其它協議，像FTP、GOPHER。甚至識別「mailto」，如果文件名指向「mailto:zxn@hq.cninfo.net」,它啟動電子郵件程序並打開一個待編輯的新郵件，例如： 

~~~cpp
ShellExecute(NULL, "open",「mailto:zxn@hq.cninfo.net」, NULL, NULL, SW_SHOWNORMAL); //打開新郵件窗口。
~~~ 

總之，ShellExecute函數就是如此簡單地打開磁盤文件和Internet文件。如果將第二個參數「OPEN」改為「PRINT」或者「EXPLORE」，ShellExecute將能打印文件和打開文件夾。ShellExecute還有一個擴展函數ShellExecuteEx，所帶參數中有一個特殊的結構，功能更強，或者任何你使用的別的瀏覽器打開文件。

----------

Q: 如何打開一個應用程序？

~~~cpp 
ShellExecute(this->m_hWnd,"open","calc.exe","","", SW_SHOW );或 ShellExecute(this->m_hWnd,"open","notepad.exe","c:\\MyLog.log","",SW_SHOW );
~~~

正如您所看到的，我並沒有傳遞程序的完整路徑。 


----------

Q: 如何打開一個同系統程序相關連的文檔？

~~~cpp
ShellExecute(this->m_hWnd,"open","c:\\abc.txt","","",SW_SHOW ); 
~~~

----------

Q: 如何打開一個網頁？ 

~~~cpp
ShellExecute(this->m_hWnd,"open","http://www.google.com","","", SW_SHOW ); 
~~~

----------

Q: 如何激活相關程序，發送EMAIL？ 

~~~cpp
ShellExecute(this->m_hWnd,"open","mailto:nishinapp@yahoo.com","","", SW_SHOW ); 
~~~

----------

Q: 如何用系統打印機打印文檔？ 

~~~cpp
ShellExecute(this->m_hWnd,"print","c:\\abc.txt","","", SW_HIDE); 
~~~

----------

Q: 如何用系統查找功能來查找指定文件？ 

~~~cpp
ShellExecute(m_hWnd,"find","d:\\nish",NULL,NULL,SW_SHOW); 
~~~

----------

Q: 如何啟動一個程序，直到它運行結束？ 

~~~cpp
SHELLEXECUTEINFO ShExecInfo = {0}; 
ShExecInfo.cbSize = sizeof(SHELLEXECUTEINFO); 
ShExecInfo.fMask = SEE_MASK_NOCLOSEPROCESS; 
ShExecInfo.hwnd = NULL; 
ShExecInfo.lpVerb = NULL; 
ShExecInfo.lpFile = "c:\\MyProgram.exe"; 
ShExecInfo.lpParameters = ""; 
ShExecInfo.lpDirectory = NULL; 
ShExecInfo.nShow = SW_SHOW; 
ShExecInfo.hInstApp = NULL; 
ShellExecuteEx(&ShExecInfo); 
WaitForSingleObject(ShExecInfo.hProcess,INFINITE); 
~~~

或： 

~~~cpp
PROCESS_INFORMATION ProcessInfo; 
STARTUPINFO StartupInfo; //This is an [in] parameter 
ZeroMemory(&StartupInfo, sizeof(StartupInfo)); 
StartupInfo.cb = sizeof StartupInfo ; //Only compulsory field 
if(CreateProcess("c:\\winnt\\notepad.exe", NULL, 
NULL,NULL,FALSE,0,NULL, 
NULL,&StartupInfo,&ProcessInfo)) 
{ 
WaitForSingleObject(ProcessInfo.hProcess,INFINITE); 
CloseHandle(ProcessInfo.hThread); 
CloseHandle(ProcessInfo.hProcess); 
}  
else 
{ 
MessageBox("The process could not be started..."); 
} 
~~~

----------

Q: 如何顯示文件或文件夾的屬性？ 

~~~cpp
SHELLEXECUTEINFO ShExecInfo ={0}; 
ShExecInfo.cbSize = sizeof(SHELLEXECUTEINFO); 
ShExecInfo.fMask = SEE_MASK_INVOKEIDLIST ; 
ShExecInfo.hwnd = NULL; 
ShExecInfo.lpVerb = "properties"; 
ShExecInfo.lpFile = "c:\\"; //can be a file as well 
ShExecInfo.lpParameters = ""; 
ShExecInfo.lpDirectory = NULL; 
ShExecInfo.nShow = SW_SHOW; 
ShExecInfo.hInstApp = NULL; 
ShellExecuteEx(&ShExecInfo);
~~~

----------

參考:

- http://blog.sina.com.cn/s/blog_54cae6d70100gh0u.html
