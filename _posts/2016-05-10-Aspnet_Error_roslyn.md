---
layout: post
title: 'ASP.NET 部屬 AppHarbor 錯誤訊息 bin\roslyn\csc.exe'
author: 'James Peng'
tags: ['ASP.NET']
---



##  起因  ##

最近想用一下 雲端 Web API，但是我窮爆了 根本沒有 Azure 的帳戶，

我們我找到了一個免費類似服務叫 AppHarbor ，也類似 Heroku 的服務。

當然，免費的最貴 馬上吃屎 遇到問題


![](..\images\2016-05-10-Aspnet_Error_roslyn\9ukMP4Q.png)


~~~csharp
Could not find a part of the path 'D:\Users\xxx\app\_PublishedWebsites\xxx\bin\roslyn\csc.exe'.

Description: An unhandled exception occurred during the execution of the current web request. Please review the stack trace for more information about the error and where it originated in the code. 

Exception Details: System.IO.DirectoryNotFoundException: Could not find a part of the path 'D:\Users\xxx\app\_PublishedWebsites\xxx\bin\roslyn\csc.exe'.

Source Error: 

An unhandled exception was generated during the execution of the current web request. Information regarding the origin and location of the exception can be identified using the exception stack trace below.
~~~



----------

<body bgcolor="white">

            <span><h1>Server Error in '/' Application.<hr width="100%" size="1" color="silver"></h1>

            <h2> <i>Could not find a part of the path 'D:\Users\xxx\app\_PublishedWebsites\LineBotJames\bin\roslyn\csc.exe'.</i> </h2></span>

            <font face="Arial, Helvetica, Geneva, SunSans-Regular, sans-serif ">

            <b> Description: </b>An unhandled exception occurred during the execution of the current web request. Please review the stack trace for more information about the error and where it originated in the code.

            <br><br>

            <b> Exception Details: </b>System.IO.DirectoryNotFoundException: Could not find a part of the path 'D:\Users\apphb2a82aac1f15969\app\_PublishedWebsites\LineBotJames\bin\roslyn\csc.exe'.<br><br>

            <b>Source Error:</b> <br><br>

            <table width="100%" bgcolor="#ffffcc">
               <tbody><tr>
                  <td>
                      <code>

An unhandled exception was generated during the execution of the current web request. Information regarding the origin and location of the exception can be identified using the exception stack trace below.</code>

                  </td>
               </tr>
            </tbody></table>

            <br>

            <b>Stack Trace:</b> <br><br>

            <table width="100%" bgcolor="#ffffcc">
               <tbody><tr>
                  <td>
                      <code><pre>
[DirectoryNotFoundException: Could not find a part of the path 'D:\Users\xxx\app\_PublishedWebsites\LineBotJames\bin\roslyn\csc.exe'.]
   System.IO.__Error.WinIOError(Int32 errorCode, String maybeFullPath) +353
   System.IO.FileStream.Init(String path, FileMode mode, FileAccess access, Int32 rights, Boolean useRights, FileShare share, Int32 bufferSize, FileOptions options, SECURITY_ATTRIBUTES secAttrs, String msgPath, Boolean bFromProxy, Boolean useLongPath, Boolean checkHost) +1326
   System.IO.FileStream..ctor(String path, FileMode mode, FileAccess access, FileShare share) +65
   Microsoft.CodeDom.Providers.DotNetCompilerPlatform.Compiler.get_CompilerName() +91
   Microsoft.CodeDom.Providers.DotNetCompilerPlatform.Compiler.FromFileBatch(CompilerParameters options, String[] fileNames) +656
   Microsoft.CodeDom.Providers.DotNetCompilerPlatform.Compiler.CompileAssemblyFromFileBatch(CompilerParameters options, String[] fileNames) +186
   System.CodeDom.Compiler.CodeDomProvider.CompileAssemblyFromFile(CompilerParameters options, String[] fileNames) +24
   System.Web.Compilation.AssemblyBuilder.Compile() +948
   System.Web.Compilation.BuildProvidersCompiler.PerformBuild() +10082369
   System.Web.Compilation.ApplicationBuildProvider.GetGlobalAsaxBuildResult(Boolean isPrecompiledApp) +9977420
   System.Web.Compilation.BuildManager.CompileGlobalAsax() +44
   System.Web.Compilation.BuildManager.EnsureTopLevelFilesCompiled() +260

[HttpException (0x80004005): Could not find a part of the path 'D:\Users\apphb2a82aac1f15969\app\_PublishedWebsites\LineBotJames\bin\roslyn\csc.exe'.]
   System.Web.Compilation.BuildManager.ReportTopLevelCompilationException() +62
   System.Web.Compilation.BuildManager.EnsureTopLevelFilesCompiled() +435
   System.Web.Compilation.BuildManager.CallAppInitializeMethod() +33
   System.Web.Hosting.HostingEnvironment.Initialize(ApplicationManager appManager, IApplicationHost appHost, IConfigMapPathFactory configMapPathFactory, HostingEnvironmentParameters hostingParameters, PolicyLevel policyLevel, Exception appDomainCreationException) +545

[HttpException (0x80004005): Could not find a part of the path 'D:\Users\apphb2a82aac1f15969\app\_PublishedWebsites\LineBotJames\bin\roslyn\csc.exe'.]
   System.Web.HttpRuntime.FirstRequestInit(HttpContext context) +9946024
   System.Web.HttpRuntime.EnsureFirstRequestInit(HttpContext context) +90
   System.Web.HttpRuntime.ProcessRequestNotificationPrivate(IIS7WorkerRequest wr, HttpContext context) +261
</pre></code>

                  </td>
               </tr>
            </tbody></table>

            <br>

            <hr width="100%" size="1" color="silver">

            <b>Version Information:</b>&nbsp;Microsoft .NET Framework Version:4.0.30319; ASP.NET Version:4.6.1069.1

            </font>

    

<!-- 
[DirectoryNotFoundException]: Could not find a part of the path &#39;D:\Users\apphb2a82aac1f15969\app\_PublishedWebsites\LineBotJames\bin\roslyn\csc.exe&#39;.
   at System.IO.__Error.WinIOError(Int32 errorCode, String maybeFullPath)
   at System.IO.FileStream.Init(String path, FileMode mode, FileAccess access, Int32 rights, Boolean useRights, FileShare share, Int32 bufferSize, FileOptions options, SECURITY_ATTRIBUTES secAttrs, String msgPath, Boolean bFromProxy, Boolean useLongPath, Boolean checkHost)
   at System.IO.FileStream..ctor(String path, FileMode mode, FileAccess access, FileShare share)
   at Microsoft.CodeDom.Providers.DotNetCompilerPlatform.Compiler.get_CompilerName()
   at Microsoft.CodeDom.Providers.DotNetCompilerPlatform.Compiler.FromFileBatch(CompilerParameters options, String[] fileNames)
   at Microsoft.CodeDom.Providers.DotNetCompilerPlatform.Compiler.CompileAssemblyFromFileBatch(CompilerParameters options, String[] fileNames)
   at System.CodeDom.Compiler.CodeDomProvider.CompileAssemblyFromFile(CompilerParameters options, String[] fileNames)
   at System.Web.Compilation.AssemblyBuilder.Compile()
   at System.Web.Compilation.BuildProvidersCompiler.PerformBuild()
   at System.Web.Compilation.ApplicationBuildProvider.GetGlobalAsaxBuildResult(Boolean isPrecompiledApp)
   at System.Web.Compilation.BuildManager.CompileGlobalAsax()
   at System.Web.Compilation.BuildManager.EnsureTopLevelFilesCompiled()
[HttpException]: Could not find a part of the path &#39;D:\Users\apphb2a82aac1f15969\app\_PublishedWebsites\LineBotJames\bin\roslyn\csc.exe&#39;.
   at System.Web.Compilation.BuildManager.ReportTopLevelCompilationException()
   at System.Web.Compilation.BuildManager.EnsureTopLevelFilesCompiled()
   at System.Web.Compilation.BuildManager.CallAppInitializeMethod()
   at System.Web.Hosting.HostingEnvironment.Initialize(ApplicationManager appManager, IApplicationHost appHost, IConfigMapPathFactory configMapPathFactory, HostingEnvironmentParameters hostingParameters, PolicyLevel policyLevel, Exception appDomainCreationException)
[HttpException]: Could not find a part of the path &#39;D:\Users\apphb2a82aac1f15969\app\_PublishedWebsites\LineBotJames\bin\roslyn\csc.exe&#39;.
   at System.Web.HttpRuntime.FirstRequestInit(HttpContext context)
   at System.Web.HttpRuntime.EnsureFirstRequestInit(HttpContext context)
   at System.Web.HttpRuntime.ProcessRequestNotificationPrivate(IIS7WorkerRequest wr, HttpContext context)
--><!-- 
This error page might contain sensitive information because ASP.NET is configured to show verbose error messages using &lt;customErrors mode="Off"/&gt;. Consider using &lt;customErrors mode="On"/&gt; or &lt;customErrors mode="RemoteOnly"/&gt; in production environments.--></body>

----------


## 解決辦法 ##

在你的專案檔 xxxxx.csproj 裡

</Target>之下

</Project>之上  

插入：

~~~html
  <PropertyGroup>
    <PostBuildEvent>
      if not exist "$(WebProjectOutputDir)\bin\Roslyn" md "$(WebProjectOutputDir)\bin\Roslyn"
      start /MIN xcopy /s /y /R "$(OutDir)roslyn\*.*" "$(WebProjectOutputDir)\bin\Roslyn"
    </PostBuildEvent>
  </PropertyGroup>
~~~

----------

參考：

- https://support.appharbor.com/discussions/problems/78633-cant-build-aspnet-mvc-project-generated-from-vstudio-2015-enterprise
- https://github.com/runesoerensen/roslyn-sample
- https://appharbor.com/
