---
layout: post
title: 'C# 資料型態 與 C++資料型態的對應關係'
author: 'James Peng'
tags: ['C# 觀念釐清']
---



C#調用C++的DLL搜集的資料型態轉換對應如下

~~~cpp
c++:HANDLE(void   *)  ----c#:System.IntPtr
c++:Byte(unsigned   char)   ----c#:System.Byte
c++:SHORT(short)   ----c#:System.Int16
c++:WORD(unsigned   short)  ----c#:System.UInt16
c++:INT(int)  ----c#:System.Int16
c++:INT(int)  ----c#:System.Int32
c++:UINT(unsigned   int)  ----c#:System.UInt16
c++:UINT(unsigned   int)  ----c#:System.UInt32
c++:LONG(long)  ----c#:System.Int32
c++:ULONG(unsigned   long)  ----c#:System.UInt32
c++:DWORD(unsigned   long) ----c#:System.UInt32
c++:DECIMAL  ----c#:System.Decimal
c++:BOOL(long)  ----c#:System.Boolean
c++:CHAR(char)  ----c#:System.Char
c++:LPSTR(char   *)----c#:System.String
c++:LPWSTR(wchar_t   *)----c#:System.String
c++:LPCSTR(const   char   *)  ----c#:System.String
c++:LPCWSTR(const   wchar_t   *)  ----c#:System.String
c++:PCAHR(char   *)----c#:System.String
c++:BSTR----c#:System.String
c++:FLOAT(float)----c#:System.Single
c++:DOUBLE(double) ----c#:System.Double 
c++:VARIANT  ----c#:System.Object
c++:PBYTE(byte   *)----c#:System.Byte[]
c++:BSTR----c#:StringBuilder
c++:LPCTSTR ----c#:StringBuilder
c++:LPCTSTR ----c#:string
c++:LPTSTR----c#:[MarshalAs(UnmanagedType.LPTStr)] string
c++:LPTSTR 輸出變數名稱  ----c#:StringBuilder  輸出變數名稱
c++:LPCWSTR----c#:IntPtr
c++:BOOL   ----c#:bool
c++:HMODULE----c#:IntPtr
c++:HINSTANCE  ----c#:IntPtr
c++:struct----c#:public struct aaa{};
c++:struct  ** 變數名稱----c#:out  變數名稱//C#中提前申明一個結構體實例化後的變量名
c++:struct  & 變數名稱 ----c#:ref struct  變數名稱

c++:WORD  ----c#:ushort
c++:DWORD ----c#:uint
c++:DWORD ----c#:int
c++:UCHAR ----c#:int
c++:UCHAR ----c#:byte
c++:UCHAR*----c#:string
c++:UCHAR*----c#:IntPtr
c++:GUID  ----c#:Guid
c++:Handle----c#:IntPtr
c++:HWND  ----c#:IntPtr
c++:DWORD ----c#:int
c++:COLORREF  ----c#:uint

c++:unsigned char ----c#:byte
c++:unsigned char *   ----c#:ref byte
c++:unsigned char *   ----c#:[MarshalAs(UnmanagedType.LPArray)] byte[]
c++:unsigned char *   ----c#:[MarshalAs(UnmanagedType.LPArray)] Intptr
c++:unsigned char &   ----c#:ref byte
c++:unsigned char  變數名稱  ----c#:byte  變數名稱
c++:unsigned short  變數名稱  ----c#:ushort  變數名稱
c++:unsigned int  變數名稱----c#:uint  變數名稱
c++:unsigned long  變數名稱   ----c#:ulong 變數名稱
c++:char  變數名稱----c#:byte  變數名稱   
c++:char  變數名稱 [size] 
   ----c#:MarshalAs(UnmanagedType.ByValTStr, SizeConst = size)]  public string  變數名稱 ;
c++:char *----c#:string   //傳入參數
c++:char *----c#:StringBuilder//傳出參數
c++:char * 變數名稱   ----c#:ref string  變數名稱
c++:char *輸入 變數名稱   ----c#:string 輸入 變數名稱
c++:char *輸出 變數名稱   ----c#:[MarshalAs(UnmanagedType.LPStr)] StringBuilder 輸出變數名稱
c++:char **   ----c#:string
c++:char ** 變數名稱  ----c#:ref string  變數名稱
c++:const char *  ----c#:string
c++:char[]----c#:string
c++:struct 結構體名 *變量名   ----c#:ref 結構體名 變量名
c++:委託 變量名   ----c#:委託 變量名
c++:int   ----c#:int
c++:int   ----c#:ref int
c++:int & ----c#:ref int
c++:int * ----c#:ref int  //C#中調用前需定義int 變量名 = 0;
c++:*int  ----c#:IntPtr
c++:int32 PIPTR * ----c#:int32[]
c++:float PIPTR * ----c#:float[]
   
c++:double** 數組名  ----c#:ref double 數組名
c++:double*[] 數組名  ----c#:ref double 數組名
c++:long  ----c#:int
c++:ulong ----c#:int

c++:UINT8 *   ----c#:ref byte   //C#中調用前需定義byte 變量名 = new byte();   

c++:handle----c#:IntPtr
c++:hwnd  ----c#:IntPtr


c++:void *----c#:IntPtr
c++:void * user_obj_param----c#:IntPtr user_obj_param
c++:void * 對像名稱----c#:([MarshalAs(UnmanagedType.AsAny)]Object 對像名稱


c++:char, INT8, SBYTE, CHAR----c#:System.SByte
c++:short, short int, INT16, SHORT   ----c#:System.Int16
c++:int, long, long int, INT32, LONG32, BOOL , INT  ----c#:System.Int32
c++:__int64, INT64, LONGLONG   ----c#:System.Int64
c++:unsigned char, UINT8, UCHAR , BYTE  ----c#:System.Byte
c++:unsigned short, UINT16, USHORT, WORD, ATOM, WCHAR , __wchar_t   ----c#:System.UInt16
c++:unsigned, unsigned int, UINT32, ULONG32, DWORD32, ULONG, DWORD, UINT - c#:System.UInt32  
c++:unsigned __int64, UINT64, DWORDLONG, ULONGLONG----c#:System.UInt64
c++:float, FLOAT  ----c#:System.Single
c++:double, long double, DOUBLE   ----c#:System.Double 
Win32 Types----  CLR Type
       
~~~


Struct需要在C#裡重新定義一個Struct

CallBack回調函數需要封裝在一個委託裡，delegate static extern int FunCallBack(string str);

unsigned char** ppImage替換成IntPtr ppImage

int& nWidth替換成ref int nWidth

int*, int&, 則都可用 ref int 對應

雙針指類型參數，可以用 ref IntPtr

函數指針使用c++: typedef double (*fun_type1)(double);

 對應 c#:public delegate double  fun_type1(double);

char* 的操作c++: char*; 對應 c#:StringBuilder;

c#中使用指針:在需要使用指針的地方 加 unsafe


----------

參考：

- http://cc-young.blogspot.tw/2012/08/c-cc-dll.html
- http://blog.yam.com/aluluba/article/40631110
