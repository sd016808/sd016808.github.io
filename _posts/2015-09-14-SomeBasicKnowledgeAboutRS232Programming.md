---
layout: post
title: 'Some basic knowledge about RS232 programming'
author: 'James Peng'
tags: ['Visual C++','C#']
---

## 串列埠存取工具（串口助手） ##

寫 RS-485、RS-232 COM Port (SerialPort) 通訊時，可以用來測試的軟體：

1.AccessPort： [http://www.sudt.com/download/AccessPort137.zip](http://www.sudt.com/download/AccessPort137.zip)

![](..\images\2015-09-14-SomeBasicKnowledgeAboutRS232Programming\fJsGUDI.png)

![](..\images\2015-09-14-SomeBasicKnowledgeAboutRS232Programming\qdyoeH7.png)

2.串口調試助手 （C#開發 附原始碼）

![](https://raw.githubusercontent.com/wenhuix/COMDBG/screenshot/Screenshot.jpg)

CODE：[https://github.com/wenhuix/COMDBG](https://github.com/wenhuix/COMDBG)，為Visual Studio 2010工程，可以被更高版本的VS打開。

EXE：[COMDBG.exe](https://github.com/wenhuix/COMDBG/releases)

介紹 [http://wenhuix.github.io/project/comdbg.html](http://wenhuix.github.io/project/comdbg.html) 


----------

## 串列埠模擬工具 ##

Virtual Serial Ports Emulator

用它來模擬 COM Port 之間的通訊。

下載：

[https://www.dropbox.com/s/7o9qzpddd9eorl9/SetupVSPE.zip?dl=0](https://www.dropbox.com/s/7o9qzpddd9eorl9/SetupVSPE.zip?dl=0)

使用方法：

![](..\images\2015-09-14-SomeBasicKnowledgeAboutRS232Programming\0iurVU9.png)

![](..\images\2015-09-14-SomeBasicKnowledgeAboutRS232Programming\je38zQN.png)

![](..\images\2015-09-14-SomeBasicKnowledgeAboutRS232Programming\JUH81S0.png)

![](..\images\2015-09-14-SomeBasicKnowledgeAboutRS232Programming\8ll5OfN.png)

打開兩個串口助手 一個連com2  一個連com3  即可互傳資料
![](..\images\2015-09-14-SomeBasicKnowledgeAboutRS232Programming\v1R9mnu.png)

----------


## 10進位int 轉成 16進位string ##

C++ 語法

~~~cpp
    int i = 100;
    CString strHex;
    strHex.Format(_T("%02X"), i);
~~~

C# 語法

~~~csharp
    int i = 100;
    string strHex = Convert.ToString(i, 16);
~~~

----------

## 16進位string 轉成 10進位int ##

C++ 語法

~~~cpp
    CString hexStr(_T("64"));
    wchar_t *end = NULL;
    int iData = (int)wcstol (hexStr, &end, 16);
~~~

C# 語法

~~~csharp
    string hexStr = "64";
    int iData = Convert.ToInt32(hexStr, 16);
~~~


----------

## 將字元或字串轉成ASCII code ##

C++ 語法

~~~cpp
	char strA = 'a';
	int asciiCode = strA;

	CStringA strA("abc");
	unsigned char *asciiBytes = (unsigned char*)strA.GetBuffer(strA.GetLength() );
~~~

C# 語法

~~~csharp
	char strA = 'a';
	int asciiCode = Convert.ToInt32(charWord);
	
	string stringWord = "abc";
	byte[] asciiBytes = Encoding.ASCII.GetBytes(stringWord);
~~~

參考：
- http://yeahpingchi.blogspot.tw/2012/04/cconvert-to-ascii.html

----------

## 取一個Byte(int)裡bit6的值 ##

C++ 語法 
   
~~~cpp
    int bitNumber = 6;
    int iData = 0xBF; //10111111
    int iBit = (iData >> bitNumber) & 1; //>> ：向右移位, a >> n 即 a 向右移 n 個 bits， ＆1就是取bit0
~~~

C# 語法

~~~csharp
    int bitNumber = 6;
    int iData = 0xBF; //10111111
    int iBit = (iData >> bitNumber) & 1;
~~~



C 語法  
  
~~~c
typedef union{
	unsigned short usValue;
	struct{
		unsigned char BIT0:1; //後面那個1 是代表佔用的 位元長度 1bit
		unsigned char BIT1:1; 
		unsigned char BIT2:1;
		unsigned char BIT3:1;
		unsigned char BIT4:1;
		unsigned char BIT5:1;
		unsigned char BIT6:1;
		unsigned char BIT7:1;
	};
}MyBYTE;

MyBYTE mybyte;
mybyte.usValue = 0xBF;
int iBit = mybyte.BIT6;
~~~

----------

## 取得一個word 的low byte 和 high byte ##

基礎知識:

一個 Char = 1 byte = 8bit  = 舉例二進制 1111 0001 = 十六進制 0xF1(F1h) = 十進制 241

一個 Word = 2 byte = 16bit = 舉例二進制 1001 0110 1111 0001 = 十六進制 0x96F1(96F1h) = 十進制 38641

兩個 Char ＝ 一個 Word

1 byte，在C++語法為 unsigned char，C# 變為了 byte。

2 byte，在C++語法為 unsigned short，C# 變為了 ushort。

以0x96F1為例子，HighByte=0x96，LowByte=0xF1


C++ 語法    

~~~cpp
unsigned short x = 0x96F1;
unsigned char HighByte = x >> 8; // 利用右移 8 bits 擠掉 low byte:
unsigned char LowByte = x & 0xff; // & 11111111 用來取最後8bits ,利用 AND 運算過濾掉 high byte 的部分。
~~~

C# 語法

~~~csharp
ushort x = 0x96F1;
byte HighByte = x >> 8; // 利用右移 8 bits 擠掉 low byte:
byte LowByte = x & 0xff; // & 11111111 用來取最後8bits ,利用 AND 運算過濾掉 high byte 的部分。
~~~


C 語法   
 
~~~c
typedef union{
	unsigned short usValue;
	struct{
		unsigned char ucLow;
		unsigned char ucHight;
	};
	struct{
		unsigned char BIT0:1; //後面那個1 是代表佔用的 位元長度 1bit
		unsigned char BIT1:1; 
		unsigned char BIT2:1;
		unsigned char BIT3:1;
		unsigned char BIT4:1;
		unsigned char BIT5:1;
		unsigned char BIT6:1;
		unsigned char BIT7:1;

		unsigned char BIT8:1;
		unsigned char BIT9:1;
		unsigned char BIT10:1;
		unsigned char BIT11:1;
		unsigned char BIT12:1;
		unsigned char BIT13:1;
		unsigned char BIT14:1;
		unsigned char BIT15:1;
	};
}MyWORD;

MyWORD x;
x.usValue = 0x96F1;
unsigned char HighByte = x.ucHight;
unsigned char LowByte = x.ucLowf; 
~~~



參考：
- [http://shukaiyang.myweb.hinet.net/cpp/bitwiseshift.zhtw.htm](http://shukaiyang.myweb.hinet.net/cpp/bitwiseshift.zhtw.htm)

----------

## Queue 用法 ##

C++ 語法  
  
~~~cpp
//匯入Queue的命名空間
#include <deque> 
using namespace std;

//宣告一個Queue
std::deque<CString> myCommandQueue;

//加入資料
myCommandQueue.push_back(L"加入第一個項目");
myCommandQueue.push_back(L"加入第二個項目");
myCommandQueue.push_back(L"加入第三個項目");
myCommandQueue.push_back(L"加入第四個項目");

//取出資料
CString strData = myOscSendQueue.pop_front();

//放到Thread執行序裡，存取時需要先lock起來。
#include <windows.h>
HANDLE g_Mutex = ::CreateMutex(NULL, FALSE, _T("MY_MUTEX_FOR_QUEUE"));
#define INFINITE            0xFFFFFFFF  // Infinite timeout

::WaitForSingleObject(g_Mutex, INFINITE);
	pDlg->myOscSendQueue.pop_front();
::ReleaseMutex(g_Mutex);
~~~

C# 語法

佇列(Queue)是用先進先出的方式處理物件的集合；而堆疊(Stack )是後進先出。

~~~csharp
//匯入Queue的命名空間
using System.Collections;

//宣告一個Queue
Queue myQueue = new Queue();

//加入資料
myQueue.Enqueue("加入第一個項目");
myQueue.Enqueue("加入第二個項目");
myQueue.Enqueue("加入第三個項目");
myQueue.Enqueue("加入第四個項目");

//取出資料
string strData = myQueue.Dequeue();

//放到Thread執行序裡，存取時需要先lock起來。
lock (myQueue.SyncRoot)  
{  
    string strData = myQueue.Dequeue();
}  
~~~

參考： [http://www.dotblogs.com.tw/yc421206/archive/2009/01/23/6930.aspx](http://www.dotblogs.com.tw/yc421206/archive/2009/01/23/6930.aspx)


----------

## 執行緒 Thread 用法 ##

C++ 語法   
 
~~~cpp
//thread
CWinThread* workerThread;
BOOL bRun = TRUE;
UINT DoWork(LPVOID pVar)
{
    while (bRun)
    {
        //do some thing
    }	
	return 0;
}

//開始執行緒
workerThread = AfxBeginThread(DoWork, (LPVOID)this);

//停止執行緒
bRun = FALSE;
~~~

C# 語法

~~~csharp
using System.Threading;

Thread workerThread;
bool bRun = true;
public void DoWork()
{
    while (bRun)
    {
		//do some thing
       	// Console.WriteLine("worker thread: working...");
    }
    //Console.WriteLine("worker thread: terminating gracefully.");
}

//開始執行緒
workerThread = new Thread(workerObject.DoWork);
workerThread.Start();

//停止執行緒
bRun = false;
~~~


參考：
- [https://msdn.microsoft.com/zh-tw/library/7a2f3ay4(v=vs.90).aspx](https://msdn.microsoft.com/zh-tw/library/7a2f3ay4(v=vs.90).aspx)

----------

## Serial Port 用法 ##


C++ 語法    
使用網路上找的class，https://github.com/jia-hong-peng/SerialPortApi

~~~cpp
//宣告一個 SerialPort
CSerialPortApi COMPort;

//打開
COMPort.OpenPort(CString sPort,DWORD dwBaudRate,BYTE byDataBits,BYTE byParity,BYTE byStopBits);

//關閉
COMPort.ClosePort();

//寫字串：
COMPort.Send(CString str);

//寫byte
COMPort.Send( uchar str[], size_t SendLength);

//讀字串
if(COMPort.ReceiveFlag)
{
    CString str += COMPort.ReadRecv();    
}

//讀數據
COMPort.ReadRecvByte();    
~~~


C# 語法

~~~csharp
//宣告一個 SerialPort
public SerialPort serialPort = new SerialPort();

//打開 SerialPort
serialPort.PortName = "COM4"; // 設定使用的 PORT
if (!serialPort.IsOpen) serialPort.Close(); // 檢查 PORT 是否關閉
serialPort.BaudRate = 9600;            // baud rate = 9600
serialPort.Parity = Parity.None;       // Parity = none
serialPort.StopBits = StopBits.One;    // stop bits = one
serialPort.DataBits = 8;               // data bits = 8
serialPort.DataReceived += new SerialDataReceivedEventHandler(SerialPort_DataReceived); // 設定 PORT 接收事件
serialPort.Open();

//寫入字串
serialPort.Write("m");

//寫入字串+換行
serialPort.WriteLine("m");

//寫入數據（位元組 byte）
serialPort.Write(byte[] buffer, int offset, int count);
//offset=寫入byte[]的起始的位元組位移，通常設0
//count＝寫入byte[]的長度

//寫入數據（字元 char）
serialPort.Write(Char[] buffer, int offset, int count);

//讀數據
int bytes = serialPort.BytesToRead;
byte[] buffer = new byte[bytes];
char[] test = new char[bytes];
serialPort.Read(test, 0, bytes);

//關閉 SerialPort
serialPort.DiscardInBuffer();       // RX  清空 serial port 的緩存
serialPort.DiscardOutBuffer();      // TX  清空 serial port 的緩存
serialPort.Close();
~~~


參考：

- http://ffyy99.pixnet.net/blog/post/32640408-c%23-%E4%BA%8B%E4%BB%B6%E8%A7%B8%E7%99%BC%E7%9A%84rs232
- https://msdn.microsoft.com/zh-tw/library/System.IO.Ports.SerialPort_methods(v=vs.110).aspx


----------
