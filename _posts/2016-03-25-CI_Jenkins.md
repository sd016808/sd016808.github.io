---
layout: post
title: 'CI Server - Jenkins'
author: 'James Peng'
tags: ['Continuous Integration']
---

在這次的系列教學的目的是 順手幫TEAM裡 架一個 CI Server，

所使用的 OS 為 VM虛擬機器 Win7 ，

而.Net的開發環境版本 VS2013 + 其他第三方套件

版本控制系統使用SVN

在建置CI Server之前，我們還需要一些軟體來進行.Net專案的建置，

大家也可以依照自己實際建置專案的需求安裝所需的軟體。



## 安裝Jenkins ##

- 到官方網站 [https://jenkins.io/](https://jenkins.io/) 下載 Windows 版。
- 過程就是 一直下一步。
- 完成後連接到 [http://localhost:8080/](http://localhost:8080/) 
- 即可看到安裝Jenkins的畫面。


----------



## 公司連不上 Plugin  ##

安裝所需的PlugIn，點選左側的管理Jenkins，選擇管理外掛程式

![](..\images\2016-03-28-CI_Jenkins\w0NoK0M.png)

這是後會發現 根本連不上...

需要設定 proxy

![](..\images\2016-03-28-CI_Jenkins\k1ipxVN.png)

設定後記得重開 jenkins

----------


## 推薦套件 Plugin ##

- [Multijob plugin](http://wiki.jenkins-ci.org/display/JENKINS/Multijob+Plugin)
- [MSBuild Plugin](http://wiki.jenkins-ci.org/display/JENKINS/MSBuild+Plugin)
- [MSTest plugin](http://wiki.jenkins-ci.org/display/JENKINS/MSTest+Plugin)
- [Role-based Authorization Strategy](http://wiki.jenkins-ci.org/display/JENKINS/Role+Strategy+Plugin)
- [HTML Publisher plugin](http://wiki.jenkins-ci.org/display/JENKINS/HTML+Publisher+Plugin)
- [Git plugin](http://wiki.jenkins-ci.org/display/JENKINS/Git+Plugin)
- [Email Extension Plugin](http://wiki.jenkins-ci.org/display/JENKINS/Email-ext+plugin)
- [DRY Plug-in](http://wiki.jenkins-ci.org/x/X4IuAg)
- [Build Timestamp Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Build+Timestamp+Plugin)
- [Authorize Project](https://wiki.jenkins-ci.org/display/JENKINS/Authorize+Project+plugin)
- [Log Parser Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Log+Parser+Plugin)
- [Task Scanner Plug-in](http://wiki.jenkins-ci.org/x/XoAs)
- [ThinBackup](https://wiki.jenkins-ci.org/display/JENKINS/thinBackup)
- [Violation Columns(List View Columns)](http://wiki.jenkins-ci.org/display/JENKINS/Violation+Columns+Plugin)


----------


## 透過公司 SMTP 寄信  ##

要先設定自己的 email 否則 會無法寄出

因為公司的 email smtp 會檢查 寄件者帳號是否存在

![](..\images\2016-03-28-CI_Jenkins\XaC20FD.png)

![](..\images\2016-03-28-CI_Jenkins\Vkze6f4.png)

![](..\images\2016-03-28-CI_Jenkins\Ys1hob1.png)

收到了

![](..\images\2016-03-28-CI_Jenkins\tlJ7bnA.png)


## 啟動登入註冊功能 ##

請先安裝 [Role-based Authorization Strategy](http://wiki.jenkins-ci.org/display/JENKINS/Role+Strategy+Plugin)

![](..\images\2016-03-28-CI_Jenkins\SRKxvM7.png)

![](..\images\2016-03-28-CI_Jenkins\qIwgXFC.png)

![](..\images\2016-03-28-CI_Jenkins\AROK5W3.png)


新增一個user

![](..\images\2016-03-28-CI_Jenkins\2BGESeh.png)

設定權限

![](..\images\2016-03-28-CI_Jenkins\ADLiE0V.png)

![](..\images\2016-03-28-CI_Jenkins\I9amCJK.png)

![](..\images\2016-03-28-CI_Jenkins\ROJXwUP.png)

![](..\images\2016-03-28-CI_Jenkins\cQ9CopF.png)

![](..\images\2016-03-28-CI_Jenkins\eSa1wiD.png)

----------

## 專案 1.Checkout SVN Code ##

1.新增作業 -> 建置 Free-Style 軟體專案

![](..\images\2016-03-28-CI_Jenkins\OwP0jIE.png)

2.因為要連線的地方 是 區網的網路磁碟機，我用內建 Subversion  怎麼試就是連不上
![](..\images\2016-03-28-CI_Jenkins\IYs0rqQ.png)

最後改用 批次檔的方式，終於正常

![](..\images\2016-03-28-CI_Jenkins\QWm3s78.png)

![](..\images\2016-03-28-CI_Jenkins\gZf52Bu.png)

 

### C:\CI_Tools\SuperDel.bat ###

 SuperDel.bat 用途是，強制刪除 DeltaIABG 目錄

~~~text
Echo Start Time - %Time%
:_START
@IF EXIST "DeltaIABG" (
  DEL /F /A /Q DeltaIABG\*.*
  RD /S /Q DeltaIABG
  if "%errorlevel%" EQU "0" (GOTO _EXIT) else if "%errorlevel%" NEQ "0" (cacls "DeltaIABG\*" /E /P everyone:f)
  GOTO _START
)
Echo NO File

:_EXIT
Echo End Time - %Time%
@pause
~~~


### C:\CI_Tools\Checkout.bat ###


需要先安裝 Subversion for Windows 才能再 CMD.exe 裡 Checkout 

官網: [https://sourceforge.net/projects/win32svn/](https://sourceforge.net/projects/win32svn/)

安裝後記得重開 jenkins


Checkout.bat 用途是：

1. 先登入網路芳鄰 \\172.99.99.99 自動輸入 帳號密碼
2. 如果 目錄 DeltaIABG 存在 就把它 砍了，若是 有 process 佔用把他關閉。
3. 確定目錄不存在，才 svn checkout 出程式碼。

~~~text
:_START
net use \\172.99.99.99 mypassword /USER:admin-PC\James.Peng
if "%errorlevel%" NEQ "0" (net use \\172.99.99.99 mypassword /USER:admin-PC\James.Peng) else GOTO _DEL

@IF EXIST "DeltaIABG" (
  GOTO _DEL
)

@IF NOT EXIST "DeltaIABG" (
  GOTO _CHECKOUT
)


:_DEL
@IF EXIST "DeltaIABG" (
	cacls "DeltaIABG\*" /E /P everyone:f
	
	tasklist /FI "IMAGENAME eq svn.exe" 2>NUL | find /I /N "svn.exe">NUL
	if "%ERRORLEVEL%"=="0" taskkill /F /IM svn.exe	
	
	tasklist /FI "IMAGENAME eq TSVNCache.exe" 2>NUL | find /I /N "TSVNCache.exe">NUL
	if "%ERRORLEVEL%"=="0" taskkill /F /IM TSVNCache.exe		
		
	DEL /F /A /Q DeltaIABG\*.*
	RD /S /Q DeltaIABG
	if "%errorlevel%" EQU "0" (GOTO _START) else if "%errorlevel%" NEQ "0" GOTO _DEL 
)

:_CHECKOUT
@IF EXIST "DeltaIABG" (  
  GOTO _DEL
)

@IF NOT EXIST "DeltaIABG" (
svn checkout file://172.99.99.99/DeltaIABG
if "%errorlevel%" EQU "0" (GOTO _EXIT) else if "%errorlevel%" NEQ "0" GOTO _DEL
)

:_EXIT
pause
~~~


----------

## 專案 2.Rebuild Debug ##

先自訂工作區 指定到 共用的工作區

![](..\images\2016-03-28-CI_Jenkins\tw6rHbP.png)

![](..\images\2016-03-28-CI_Jenkins\gnW4I6C.png)

### 設置三個 MSBuild ###

![](..\images\2016-03-28-CI_Jenkins\flbWE7z.png)


~~~text
v4.0.30319 DEBUG rebuild 
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\MSBuild.exe
/t:rebuild /p:configuration="Debug"
~~~


~~~text
v4.0.30319 RELEASE rebuild 
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\MSBuild.exe
/t:rebuild /p:configuration="Release"
~~~

~~~text
v4.0.30319
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\MSBuild.exe

~~~

1.安裝 [MSBuild Plugin](http://wiki.jenkins-ci.org/display/JENKINS/MSBuild+Plugin)

2.新增作業 -> 建置 Free-Style 軟體專案

3.新增建置步驟 -> Build a Visual Studio project or solution using MSBuild

![](..\images\2016-03-28-CI_Jenkins\DlG7mWo.png)


----------

## 專案 3.Rebuild Release ##

先自訂工作區 指定到 共用的工作區

![](..\images\2016-03-28-CI_Jenkins\tw6rHbP.png)

![](..\images\2016-03-28-CI_Jenkins\uME3Xat.png)


----------


## 專案 4.Check Coding Style ##

到 [StyleCop CodePlex 下載](http://stylecop.codeplex.com/) StyleCop 並在 Jenkins Server 上安裝起來。

先自訂工作區 指定到 共用的工作區

![](..\images\2016-03-28-CI_Jenkins\tw6rHbP.png)

![](..\images\2016-03-28-CI_Jenkins\dxxU6Fx.png)


StyleCopSetting.xml

~~~text
<?xml version="1.0" encoding="utf-8"?>
<Project Defaulttargets="StyleCop" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>    
	<!-- file want to scan -->
	<AnalysisFileIncludes>				
	$(MSBuildStartupDirectory)\DeltaIABG\Project\EIPBuilder\*.cs;
	$(MSBuildStartupDirectory)\DeltaIABG\Project\EIPBuilder\**\*.cs;		
	$(MSBuildStartupDirectory)\DeltaIABG\Project\EIPBuilder\**\**\*.cs;
	$(MSBuildStartupDirectory)\DeltaIABG\Component\**\*.cs;		
	$(MSBuildStartupDirectory)\DeltaIABG\Component\**\**\*.cs;	
	$(MSBuildStartupDirectory)\DeltaIABG\Component\**\**\**\*.cs;
	$(MSBuildStartupDirectory)\DeltaIABG\Experiment\Experiment\*.cs;
	</AnalysisFileIncludes>
	

    <!-- file dont want to scan -->
	<AnalysisFileExcludes>$(MSBuildStartupDirectory)\**\*Test.cs</AnalysisFileExcludes>
  </PropertyGroup>

  <UsingTask AssemblyFile="$(MSBuildExtensionsPath)\..\StyleCop 4.7\StyleCop.dll" TaskName="StyleCopTask"/>

  <Target Name="StyleCop">    
    <!-- Create a collection of files to scan -->
	<CreateItem Include="$(AnalysisFileIncludes)" Exclude="$(AnalysisFileExcludes)">
		<Output TaskParameter="Include" ItemName="StyleCopFiles" />
	</CreateItem>

	<!-- Execute stylecop scan -->
    <StyleCopTask
			ProjectFullPath="$(MSBuildProjectFile)"
			SourceFiles="@(StyleCopFiles)"
			ForceFullAnalysis="true"
			TreatErrorsAsWarnings="true"
			OutputFile="$(MSBuildStartupDirectory)\StyleCopReport.xml"
			CacheResults="true" />    
  </Target>
</Project>
~~~


----------

## 專案 5.Static Code Analysis ##


FxCop是一套由微軟所開發的靜態程式碼分析工具，
不同於StyleCop是針對程式碼做掃描，FxCop則是會掃描已經建置好的dll，
檢查我們的程式碼是否符合
[Net Framework Design Guidelines](http://msdn.microsoft.com/en-us/library/vstudio/ms229042)，
透過這套工具，能夠讓開發人員更容易寫出容易維護擴充的程式碼，
也可以避免掉一些效能或安全性上不正確的寫法而引發問題。

※安裝FxCop

1. 首先必須安裝 [Microsoft Windows SDK for Windows 7 and .NET Framework 4](http://www.microsoft.com/en-us/download/details.aspx?id=8442)
2. 在 C:\Program Files (x86)\Microsoft SDKs\Windows\v7.0A\FXCop\FxCopSetup.exe 找到安裝檔並安裝

先自訂工作區 指定到 共用的工作區

![](..\images\2016-03-28-CI_Jenkins\tw6rHbP.png)


![](..\images\2016-03-28-CI_Jenkins\cVlc0uR.png)

~~~text
"C:\Program Files (x86)\Microsoft Visual Studio 12.0\Team Tools\Static Analysis Tools\FxCop\FxCopCmd.exe" /file:"C:\Program Files (x86)\Jenkins\jobs\Build SVN Code\workspace\DeltaIABG\Project\EIPBuilder\bin\Debug\Delta*.dll" /rule:"C:\Program Files (x86)\Microsoft Visual Studio 12.0\Team Tools\Static Analysis Tools\FxCop\Rules\DesignRules.dll" /out:FxCopReport.xml
pause
~~~

----------

## 專案 6.Check Duplicated Code ##

偵測重複程式碼的部分

我首先嘗試了Simian

不過使用結果會跑出Error: Big 5

猜測是因為我的程式中 部分有顯示中文 或是註解有中文，導致Simian無法使用

因此我改用 PMD 的 CPD

首先下載PMD：[官方網址](https://pmd.github.io/)



先自訂工作區 指定到 共用的工作區

![](..\images\2016-03-28-CI_Jenkins\tw6rHbP.png)


新增建置步驟 -> 執行 Windows 批次檔

~~~text
C:\CI_Tools\pmd-bin\bin\cpd.bat --minimum-tokens 100 --language cs --format xml --files "C:\Program Files (x86)\Jenkins\jobs\Build SVN Code\workspace\DeltaIABG" >duplicatecode.xml || exit 0
~~~

![](..\images\2016-03-28-CI_Jenkins\Mwno8F4.png)

----------


## 專案 7.Check Complex Methods ##

在Source Monitor的[官方網站](http://www.campwoodsw.com/sourcemonitor.html)下載並安裝。

這裡需要額外的一個工具 [Command Line Transformation Utility (msxsl.exe)](https://www.microsoft.com/en-us/download/details.aspx?id=21714)，到微軟的官網直接下載使用即可，它的作用在於將 SourceMonitor 的結果轉成 html。



先自訂工作區 指定到 共用的工作區

![](..\images\2016-03-28-CI_Jenkins\tw6rHbP.png)


![](..\images\2016-03-28-CI_Jenkins\l0cnhhi.png)

![](..\images\2016-03-28-CI_Jenkins\uOqVeXe.png)


### SourceMonitor.bat ###

~~~text
"C:\Program Files (x86)\SourceMonitor\SourceMonitor.exe" /C "C:\CI_Tools\SourceMonitorCommand.xml"
"C:\CI_Tools\msxsl.exe" SourceMonitorReport.xml "C:\CI_Tools\SourceMonitorSummaryGeneration.xsl" -o SourceMonitorSummaryGeneration.xml
"C:\CI_Tools\msxsl.exe" SourceMonitorSummaryGeneration.xml "C:\CI_Tools\SourceMonitor.xsl" -o SourceMonitorResult.html
~~~

請先下載 .xsl 及 .xml 並存放在C:\CI_Tools\下

### SourceMonitorCommand.xml ###

~~~text
<?xml version="1.0" encoding="utf-8"?>
<sourcemonitor_commands>
    <write_log>true</write_log>
    <command>
        <project_file>SourceMonitor.smp</project_file>
        <checkpoint_name>Baseline</checkpoint_name>
        <project_language>C#</project_language>
        <!-- 要分析專案的相對路徑 -->
        <source_directory>C:\Program Files (x86)\Jenkins\jobs\Build SVN Code\workspace\DeltaIABG\</source_directory>
        <source_subdirectory_list>
            <exclude_subdirectories>true</exclude_subdirectories>
            <source_subtree>bin\</source_subtree>
            <source_subdirectory>obj\</source_subdirectory>
        </source_subdirectory_list>
        <parse_utf8_files>True</parse_utf8_files>
        <ignore_headers_footers>True</ignore_headers_footers>
        <export>
            <!-- 最後輸出的檔名 -->
            <export_file>SourceMonitorReport.xml</export_file>
            <export_type>2</export_type>
        </export>
    </command>
</sourcemonitor_commands>
~~~

### SourceMonitorSummaryGeneration.xsl ###

~~~text
<?xml version="1.0" encoding="UTF-8" ?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
   
  <!--- SourceMonitor Summary Xml File Generation created by Eden Ridgway -->
  <xsl:output method="xml"/>
   
  <xsl:template match="/">
    <xsl:apply-templates select="/sourcemonitor_metrics" />
  </xsl:template>
   
  <!-- Transform the results into a simpler more intuitive and summarised format -->
  <xsl:template match="sourcemonitor_metrics">
      <SourceMonitorComplexitySummary>
        <MostComplexMethods>
          <xsl:call-template name="MostComplexMethods"/>
        </MostComplexMethods>
         
        <LinePerMethod>
          <xsl:call-template name="LinesPerMethod"/>
        </LinePerMethod>
 
        <MostDeeplyNestedCode>
          <xsl:call-template name="MostDeeplyNestedCode"/>
        </MostDeeplyNestedCode>
         
      </SourceMonitorComplexitySummary>
  </xsl:template>
 
  <!-- Complexity Metrics -->
  <xsl:template name="MostComplexMethods">
    <xsl:for-each select=".//file">
      <xsl:sort select="metrics/metric[@id='M10']" order="descending" data-type="number" />
       
      <xsl:choose>
        <xsl:when test="position() &lt; 16">
           <Method>
              <File><xsl:value-of select="@file_name"/></File>
              <Name><xsl:value-of select="metrics/metric[@id='M9']"/></Name>
              <Line><xsl:value-of select="metrics/metric[@id='M8']"/></Line>
              <Complexity><xsl:value-of select="metrics/metric[@id='M10']"/></Complexity>
           </Method>
        </xsl:when>
      </xsl:choose>
    </xsl:for-each>
  </xsl:template>
 
  <!-- Complexity Metrics -->
  <xsl:template name="LinesPerMethod">
    <xsl:for-each select=".//file">
      <xsl:sort select="metrics/metric[@id='M5']" order="descending" data-type="number" />
       
      <xsl:choose>
        <xsl:when test="position() &lt; 16">
           <Method>
              <File><xsl:value-of select="@file_name"/></File>
              <Lines><xsl:value-of select="metrics/metric[@id='M5']"/></Lines>
           </Method>
        </xsl:when>
      </xsl:choose>
    </xsl:for-each>
  </xsl:template>
 
  <!-- Nesting Metrics -->
  <xsl:template name="MostDeeplyNestedCode">
    <xsl:for-each select=".//file">
        <xsl:sort select="metrics/metric[@id='M12']" order="descending" data-type="text" />
       
        <xsl:choose>
          <xsl:when test="position() &lt; 16">
             <Block>
                <File><xsl:value-of select="@file_name"/></File>
                <Depth><xsl:value-of select="metrics/metric[@id='M12']"/></Depth>
                <Line><xsl:value-of select="metrics/metric[@id='M11']"/></Line>
             </Block>
          </xsl:when>
        </xsl:choose>
    </xsl:for-each>
  </xsl:template>
   
</xsl:stylesheet>
~~~

### SourceMonitor.xsl ###

~~~text
<?xml version="1.0" encoding="utf-8" ?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns="http://www.w3.org/TR/xhtml1/strict">
  <xsl:output method="html"/>
   
  <xsl:template match="/">
  <html>
        <head>
        <style type="text/css">
    /* 
 Web20 Table Style
 written by Netway Media, http://www.netway-media.com
*/
    table
    {
        border-collapse: collapse;
        border: 1px solid #666666;
        font: normal 11px verdana, arial, helvetica, sans-serif;
        color: #363636;
        background: #f6f6f6;
        text-align: left;
    }
    caption
    {
        text-align: center;
        font: bold 16px arial, helvetica, sans-serif;
        background: transparent;
        padding: 6px 4px 8px 0px;
        color: #CC00FF;
        text-transform: uppercase;
    }
    thead, tfoot
    {
        background: url(bg1.png) repeat-x;
        text-align: left;
        height: 30px;
    }
    thead th, tfoot th
    {
        padding: 5px;
    }
    table a
    {
        color: #333333;
        text-decoration: none;
    }
    table a:hover
    {
        text-decoration: underline;
    }
    tr.odd
    {
        background: #f1f1f1;
    }
    tbody th, tbody td
    {
        padding: 5px;
        padding-left: 10px;
    }
    td.sectionheader
    {
        color: Navy;
        font-size: 20px;
        font-weigth: bold;
        text-align: center;
    }
</style>
        </head>
        <body>
    <xsl:apply-templates select="//SourceMonitorComplexitySummary" />     
    </body>
    </html>
  </xsl:template>
   
 <!-- The most complex methods -->
  <xsl:template match="SourceMonitorComplexitySummary">
    <table class="section-table" cellpadding="2" cellspacing="0" border="0" width="98%">
      <!--<colgroup>
        <col width="300px" />
        <col width="50px" />
        <col />
        <col width="50px" />
      </colgroup>-->
      <tr>
        <td class="sectionheader" colspan="4">
           SourceMonitor - <xsl:value-of select="count(MostComplexMethods/Method)" /> Most Complex Methods
        </td>
      </tr>
      <tr>
        <th class="header-label">
          Complexity
        </th>
        <th class="header-label">
          File
        </th>
        <th class="header-label">
          Method
        </th>        
        <th class="header-label">
          Line
        </th>
      </tr>
      <xsl:apply-templates select="MostComplexMethods/Method" />
    </table>
     
     
 
     
    <!-- The most deeply nested code blocks -->
    <table class="section-table" cellpadding="2" cellspacing="0" border="0" width="98%">
      <!--<colgroup>
        <col width="50px"/>
        <col />
        <col width="50px" />
      </colgroup>-->
      <tr>
        <td class="sectionheader" colspan="4">           
           SourceMonitor - <xsl:value-of select="count(MostDeeplyNestedCode/Block)" /> Most Deeply Nested Code Blocks
        </td>
      </tr>
      <tr>
        <th class="header-label">
          Depth
        </th>
        <th class="header-label">
          File
        </th>
        <th class="header-label">
          Line
        </th>
      </tr>
      <xsl:apply-templates select="MostDeeplyNestedCode/Block" />
    </table>
  </xsl:template>
 
  <!-- Complex Method List -->
  <xsl:template match="MostComplexMethods/Method">
      <tr>
        <xsl:if test="position() mod 2 = 1">
          <xsl:attribute name="class">section-oddrow odd</xsl:attribute>          
        </xsl:if>
        <td>
          <xsl:value-of select="Complexity" />
        </td>
        <td >
          <xsl:value-of select="File" />
        </td>
        <td>
          <xsl:value-of select="Name"/>
        </td>        
        <td>
          <xsl:value-of select="Line" />
        </td>
      </tr>
  </xsl:template>
 
  <!-- Deeply Nested Blocks List -->
  <xsl:template match="MostDeeplyNestedCode/Block">
      <tr>
        <xsl:if test="position() mod 2 = 1">
          <xsl:attribute name="class">section-oddrow odd</xsl:attribute>
        </xsl:if>
        <td>
          <xsl:value-of select="Depth" />
        </td>
        <td>
          <xsl:value-of select="File" />
        </td>
        <td>
          <xsl:value-of select="Line" />
        </td>
      </tr>
  </xsl:template>
 
</xsl:stylesheet>
~~~


----------

## 整合 MultiJob Project  ##

![](..\images\2016-03-28-CI_Jenkins\TNfAPe4.png)


先自訂工作區 指定到 共用的工作區

![](..\images\2016-03-28-CI_Jenkins\tw6rHbP.png)


### 定期建置 ###

~~~text
0 08 * * *
0 09 * * *
0 10 * * *
0 11 * * *
0 12 * * *
0 13 * * *
0 14 * * *
0 15 * * *
0 16 * * *
0 17 * * *
0 18 * * *
~~~

### 加入現有的 MutiJob Phase ###

![](..\images\2016-03-28-CI_Jenkins\0MNetRm.png)

![](..\images\2016-03-28-CI_Jenkins\BdKdbNQ.png)

### 找到重複代碼的報告 ###

![](..\images\2016-03-28-CI_Jenkins\FRsP1td.png)

### 程式複雜度的報告 ###

![](..\images\2016-03-28-CI_Jenkins\tsMG9u4.png)

### 程式風格 和 靜態程式碼分析的報告 ###

![](..\images\2016-03-28-CI_Jenkins\LT9xfZG.png)


### 寄出報告信 ###

![](..\images\2016-03-28-CI_Jenkins\nE3Ebek.png)

加入 Triggers

![](..\images\2016-03-28-CI_Jenkins\1Nfa5Hn.png)


#### 失敗時 ####

Subject

~~~html
[Failure] Daily build $BUILD_TIMESTAMP
~~~


Content ： Failure 字體紅色

~~~html
<spen style="font:Calibri;">Dear all,<br/>
<br/>
Daily Build Result : <br/>
<span style="color:#FF0000;font-size:40px;"><b>Failure</b></span><br/>
[$BUILD_TIMESTAMP]<br/>
<br/>
Report Login Account: user<br/>
Login Password: user<br/>
<br/>
Result1: <a href="http://172.99.99.99:8080/job/Build%20SVN%20Code/lastBuild/consoleFull">Checkout SVN Code - <span style="text-transform:capitalize;">$1_BUILD_SVN_CODE_RESULT</span></a><br/>
Result2: <a href="http://172.99.99.99:8080/job/Rebuild%20Debug/lastBuild/consoleFull">Rebuild Code Debug - <span style="text-transform:capitalize;">$2_REBUILD_DEBUG_RESULT</span></a><br/>
Result3: <a href="http://172.99.99.99:8080/job/Rebuild%20Release/lastBuild/consoleFull">Rebuild Code Release - <span style="text-transform:capitalize;">$3_REBUILD_RELEASE_RESULT</span></a><br/>
Result4: <a href="http://172.99.99.99:8080/job/automation%20for%20development/violations/#stylecop">Coding Style</a><br/>
Result5: <a href="http://172.99.99.99:8080/job/automation%20for%20development/violations/#fxcop">Static Code Analysis</a><br/>
Result6: <a href="http://172.99.99.99:8080/job/automation%20for%20development/$BUILD_ID/dryResult/">Duplicated Code</a><br/>
Result7: <a href="http://172.99.99.99:8080/job/automation%20for%20development/HTML_Report/">Complex Methods</a><br/>
<br/>
<br/>
Check <a href="http://172.99.99.99:8080/job/automation%20for%20development/">this</a> to view the detial results. 
<br/>
<br/>
Best regards,<br/>
James Peng</spen>
~~~

#### 不穩定時 ####

Subject

~~~html
[Unstable] Daily build $BUILD_TIMESTAMP
~~~


Content ： Unstable 字體橘色

~~~html
<spen style="font:Calibri;">Dear all,<br/>
<br/>
Daily Build Result : <br/>
<span style="color:#FF6600;font-size:40px;"><b>Unstable</b></span><br/>
[$BUILD_TIMESTAMP]<br/>
<br/>
Report Login Account: user<br/>
Login Password: user<br/>
<br/>
Result1: <a href="http://172.99.99.99:8080/job/Build%20SVN%20Code/lastBuild/consoleFull">Checkout SVN Code - <span style="text-transform:capitalize;">$1_BUILD_SVN_CODE_RESULT</span></a><br/>
Result2: <a href="http://172.99.99.99:8080/job/Rebuild%20Debug/lastBuild/consoleFull">Rebuild Code Debug - <span style="text-transform:capitalize;">$2_REBUILD_DEBUG_RESULT</span></a><br/>
Result3: <a href="http://172.99.99.99:8080/job/Rebuild%20Release/lastBuild/consoleFull">Rebuild Code Release - <span style="text-transform:capitalize;">$3_REBUILD_RELEASE_RESULT</span></a><br/>
Result4: <a href="http://172.99.99.99:8080/job/automation%20for%20development/violations/#stylecop">Coding Style</a><br/>
Result5: <a href="http://172.99.99.99:8080/job/automation%20for%20development/violations/#fxcop">Static Code Analysis</a><br/>
Result6: <a href="http://172.99.99.99:8080/job/automation%20for%20development/$BUILD_ID/dryResult/">Duplicated Code</a><br/>
Result7: <a href="http://172.99.99.99:8080/job/automation%20for%20development/HTML_Report/">Complex Methods</a><br/>
<br/>
<br/>
Check <a href="http://172.99.99.99:8080/job/automation%20for%20development/">this</a> to view the detial results. 
<br/>
<br/>
Best regards,<br/>
James Peng</spen>
~~~

#### 成功時 ####


Subject

~~~html
[Success] Daily build $BUILD_TIMESTAMP
~~~


Content ： Success 字體綠色

~~~html
<spen style="font:Calibri;">Dear all,<br/>
<br/>
Daily Build Result : <br/>
<span style="color:#006600;font-size:40px;"><b>Success</b></span><br/>
[$BUILD_TIMESTAMP]<br/>
<br/>
Report Login Account: user<br/>
Login Password: user<br/>
<br/>
Result1: <a href="http://172.99.99.99:8080/job/Build%20SVN%20Code/lastBuild/consoleFull">Checkout SVN Code - <span style="text-transform:capitalize;">$1_BUILD_SVN_CODE_RESULT</span></a><br/>
Result2: <a href="http://172.99.99.99:8080/job/Rebuild%20Debug/lastBuild/consoleFull">Rebuild Code Debug - <span style="text-transform:capitalize;">$2_REBUILD_DEBUG_RESULT</span></a><br/>
Result3: <a href="http://172.99.99.99:8080/job/Rebuild%20Release/lastBuild/consoleFull">Rebuild Code Release - <span style="text-transform:capitalize;">$3_REBUILD_RELEASE_RESULT</span></a><br/>
Result4: <a href="http://172.99.99.99:8080/job/automation%20for%20development/violations/#stylecop">Coding Style</a><br/>
Result5: <a href="http://172.99.99.99:8080/job/automation%20for%20development/violations/#fxcop">Static Code Analysis</a><br/>
Result6: <a href="http://172.99.99.99:8080/job/automation%20for%20development/$BUILD_ID/dryResult/">Duplicated Code</a><br/>
Result7: <a href="http://172.99.99.99:8080/job/automation%20for%20development/HTML_Report/">Complex Methods</a><br/>
<br/>
<br/>
Check <a href="http://172.99.99.99:8080/job/automation%20for%20development/">this</a> to view the detial results. 
<br/>
<br/>
Best regards,<br/>
James Peng</spen>
~~~




## 成果： ##

![](..\images\2016-03-28-CI_Jenkins\owCszEW.png)

----------

## 參考： ##

- http://ithelp.ithome.com.tw/question/10109773
- http://www.gibar.co/2014/10/jenkins-role-strategy-plugin.html
- https://dotblogs.com.tw/supershowwei/2015/10/14/153562
- https://cheeruplewis.wordpress.com/2016/03/11/ci-jenkins-%E5%85%AD-pmd_cpd/
