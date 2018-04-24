---
layout: post
title: 'CToolBar 浮動功能表'
author: 'James Peng'
tags: ['Visual C++']
---

![](..\images\2012-01-03-MFC_FloatingMenu\uHHPvwL.png)

![](..\images\2012-01-03-MFC_FloatingMenu\dUBWfdc.png)


## 步驟1 ##

- 先建立一個 Doc / View 專案

----------

## 步驟2 ##

- 繼承 CToolBar 類別 ，產生一個 子類別 CFloatMenu。
- 定義 CMenu* m_pSubMenu; 指標

~~~cpp
class CFloatMenu : public CToolBar
{
// Construction
public:
	CFloatMenu();
	int MenuPopIndex;
	CMenu* m_pSubMenu;
~~~

----------

## 步驟3 ##

- 寫一個 AddButtonFromMenu() 方法：依據功能表新增工具列按鈕

~~~cpp
BOOL CFloatMenu::AddButtonFromMenu(UINT IDresource)
{
	CMenu Menu ;
	if (Menu.LoadMenu(IDresource))
	{
		//獲取選單頂層選單數
		UINT ButtonCount = Menu.GetMenuItemCount();
		UINT * array = new UINT[ButtonCount];

		CString text;
		for (int n = 0; n<ButtonCount;n++)
		{
			array[n]=ID_BUTTON1 +n;
		}
		SetButtons(array,ButtonCount);
		for (int i=0;i<ButtonCount;i++)
		{
			Menu.GetMenuString(i,text,MF_BYPOSITION);
			SetButtonText(i,text);
			SetButtonStyle(i,TBSTYLE_DROPDOWN);	
		}

		delete array;
		Menu.DestroyMenu();
		return true;
	}
	else
		return FALSE;
}
~~~


----------

## 步驟4 ##

- 寫一個 GetIndexFromPoint() 方法：依據鼠標確定工具列按鈕索引

~~~cpp
UINT CFloatMenu::GetIndexFromPoint(CPoint pot)
{
	CRect rect;
	for (int i = 0;i< GetToolBarCtrl().GetButtonCount()-1;i++)
	{
		GetItemRect(i,rect);
		if (rect.PtInRect(pot))
			return i;
	}
	return -1;
}
~~~

~~~cpp
CFloatMenu::CFloatMenu()
{
	MenuPopIndex = -1;
}
~~~

~~~cpp
void CFloatMenu::OnMouseMove(UINT nFlags, CPoint point) 
{
	CToolBar::OnMouseMove(nFlags, point);
	if (MenuPopIndex!= -1)
	{
		if(m_pSubMenu)
		{
			if (MenuPopIndex==GetIndexFromPoint(point))
				m_pSubMenu->DestroyMenu();
		}
	}
}


~~~

----------

## 步驟5 ##

- 在主框架 MainFrm.cpp 裡，：新增 message 如下

~~~cpp
	ON_NOTIFY(TBN_DROPDOWN,AFX_IDW_TOOLBAR,OnToolbarDropDown)
~~~

- 在主框架 MainFrm.cpp 裡：寫一個 OnToolbarDropDown() 方法，在使用者按一下工具列按鈕時彈出功能表。

~~~cpp
void CMainFrame::OnToolbarDropDown(NMTOOLBAR *pnmtb, LRESULT *plr)
{
	CWnd* pWnd = &m_wndFloatTool;
	UINT nID = IDR_MAINFRAME;

	CMenu menu;
	menu.LoadMenu(nID);
	CMenu* pPop = menu.GetSubMenu(pnmtb->iItem-ID_BUTTON1);
	
	m_wndFloatTool.m_pSubMenu = pPop;
	m_wndFloatTool.MenuPopIndex = pnmtb->iItem-ID_BUTTON1;

	CRect rc;
	m_wndFloatTool.GetToolBarCtrl().GetItemRect(pnmtb->iItem-ID_BUTTON1,rc);
	pWnd->ClientToScreen(&rc);
	pPop->TrackPopupMenu(TPM_LEFTALIGN|TPM_LEFTBUTTON|TPM_VERTICAL,rc.left,rc.bottom,this,&rc);	
	m_wndFloatTool.MenuPopIndex = -1;

}
~~~

