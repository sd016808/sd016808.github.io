---
layout: post
title: 'CMenu 依據資料表中資料動態產生功能表'
author: 'James Peng'
tags: ['Visual C++']
---

![](..\images\2012-01-02-MFC_datatable_dynamically_generated_menu\51RBy37.png)

MFC本身有提供CMenu類別


## 用 AppendMenu 用來新增功能表 ##

在 OnInitDialog() 裡

~~~cpp

	CMenu* pSysMenu = GetSystemMenu(FALSE);
	if (pSysMenu != NULL)
	{
		CString strAboutMenu;
		strAboutMenu.LoadString(IDS_ABOUTBOX);
		if (!strAboutMenu.IsEmpty())
		{
			pSysMenu->AppendMenu(MF_SEPARATOR);
			pSysMenu->AppendMenu(MF_STRING, IDM_ABOUTBOX, strAboutMenu);
		}
	}

~~~



----------


## 判斷指定的功能表是否有子功能表選項 ##

~~~cpp
BOOL CDynamicMenuDlg::IsHaveSubMenu(CString str)
{
	_ConnectionPtr m_con;
	_RecordsetPtr m_record;
	m_con.CreateInstance("ADODB.Connection");
	m_record.CreateInstance("ADODB.Recordset");
	m_con->ConnectionString = m_pCon->ConnectionString;
	m_con->Open("","","",-1);
	CString sql;
	sql.Format("select * from tb_menuinfo where 上層功能表 = '%s'",str);
	m_record = m_con->Execute((_bstr_t)sql,NULL,adCmdText);
	if ((!m_record->BOF) &&(! m_record->ADOEOF))
	{
		return TRUE;
	}
	return FALSE;
}
~~~

----------


## 從資料庫中載入功能表 ##

~~~cpp
void CDynamicMenuDlg::LoadMenuFromDatabase()
{

	CString sql;
	sql.Format( "select * from tb_menuinfo where 上層功能表 is NULL");
	m_pRecord = m_pCon->Execute((_bstr_t)sql,NULL,adCmdText);
	CMenu m_menu;
	
	CString c_menustr;
	while (! m_pRecord->ADOEOF)
	{
		c_menustr = m_pRecord->GetCollect("功能表名稱").bstrVal;
		//menu.AppendMenu(MF_STRING,-1,c_menustr);
		LoadSubMenu(&menu,c_menustr);	
		m_pRecord->MoveNext();
	}
	SetMenu(&menu);
}
~~~


----------

## 利用遞迴的方式載入子功能表 ##

~~~cpp
void  CDynamicMenuDlg::LoadSubMenu(CMenu* m_menu,CString str)
{
	_ConnectionPtr m_con;
	_RecordsetPtr m_record;
	m_con.CreateInstance("ADODB.Connection");
	m_record.CreateInstance("ADODB.Recordset");
	m_con->ConnectionString = m_pCon->ConnectionString;
	m_con->Open("","","",-1);
	CString sql;
	sql.Format("select * from tb_menuinfo where 上層功能表 = '%s'",str);
	m_record = m_con->Execute((_bstr_t)sql,NULL,adCmdText);
	CMenu m_tempmenu;
	m_tempmenu.CreatePopupMenu();

	while (!m_record->ADOEOF)
	{
		CString c_menustr = m_record->GetCollect("功能表名稱").bstrVal;
		m_tempmenu.AppendMenu(MF_STRING,-1,c_menustr);
		if (IsHaveSubMenu(c_menustr))
			LoadSubMenu(&m_tempmenu,c_menustr);
		m_record->MoveNext();
	}

	m_menu->AppendMenu(MF_POPUP,(UINT)m_tempmenu.m_hMenu,str);
	m_tempmenu.Detach();

	if (m_record != NULL)
		m_record.Release();
	if (m_con!= NULL)
		m_con.Release();
}
~~~


----------

