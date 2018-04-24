---
layout: post
title: 'Thread 裡調用 UpdateData'
author: 'James Peng'
tags: ['Visual C++']
---


在嘗試線程更新界面時，在線程中調用UpdateData(FALSE)後出現如下錯誤：

![](..\images\2015-10-16-MFC_ThreadUpdata\rVSAYNX.png)

# 原因： #

　　MFC對像不支持多線程操作，不能供多個線程進程使用。子線程調用pDlg-> UpdateData(FALSE)時主線程（界麵線程）會阻塞，更新必須由它完成，這樣就形成死鎖。UpdateData()函數屬於CDialog類的保護成員函數，在工作線程中不能使用UpdateData來更新主線程中的數據。更改界面的操作最好用主線程（界麵線程），要想在子線程（工作線程）裡執行界麵線程的操作，可以通過向主線程發送消息來解決。


----------

# 解決辦法： #

## 方法1 ##

創建線程時使用AfxBeginThread創建CWinThread繼承類，參數傳遞時最好傳遞窗口句柄（主線程向子線程傳遞對像不安全），使用PostMessage或SendMessage消息傳遞函數向主窗口發送自定義消息，在主窗口中對自定義消息進行處理並更新。

創建線程：

~~~cpp
    void CEditTestDlg::OnBnClickedButton4()
    { 
	    CWinThread* pThread;
	    pThread = AfxBeginThread(SendMsgThread, this );
    }
~~~

發送消息（推薦使用SendMessageTimeout）：


~~~cpp
    static UINT SendMsgThread(LPVOID lpParam)
    {
	    CEditTestDlg *dlg = (CEditTestDlg* ) lpParam;
	     int i = 0 ;
	     while (i < 100 )
	    {
		    Sleep( 20 );
		    i += 1 ;
		    dlg ->m_value2.Format(_T( " %d " ), i);
		     // PostMessage(dlg->m_hWnd,WM_UPDATEDATA,FALSE,NULL);
		     // SendMessage(dlg->m_hWnd,WM_UPDATEDATA,FALSE,NULL); 
		    SendMessageTimeout(dlg->m_hWnd, WM_UPDATEDATA, FALSE, NULL, SMTO_BLOCK, 1000 , NULL);
	    }
	    return  0 ;
    }
~~~

消息處理：

~~~cpp
    LRESULT CEditTestDlg::OnUpdateData(WPARAM wParam, LPARAM lParam)
    {
	    UpdateData(wParam);
	    return  0 ;
    }
~~~


----------

## 方法2 ##

使用control變量或直接獲取控件指針進行更新：

~~~cpp
    dlg->m_editCtl.SetWindowText(dlg->m_value2);
    dlg->GetDlgItem(IDC_EDIT2)->SetWindowText(dlg->m_value2);
~~~

----------

## 方法3 ##

最簡單的辦法，將Debug改為Release正常運行。

----------

## 參考: ##

- http://www.cnblogs.com/skywatcher/p/3506158.html
