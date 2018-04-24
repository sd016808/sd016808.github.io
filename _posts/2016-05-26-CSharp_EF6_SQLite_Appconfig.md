---
layout: post
title: 'C# 使用 EF6 於 SQLite 修改資料庫連線字串 app.config'
author: 'James Peng'
tags: ['Entity Framework']
---

# 資料庫連接 #

![](..\images\2016-05-26-CSharp_EF6_SQLite_Appconfig\oTje59C.png)

連接字串 會透過 精靈產生，然後寫在 app.config 裡，如下：<connectionStrings>

## app.config ##

~~~xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
    <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
  </configSections>
  <system.data>
    <DbProviderFactories>
      <remove invariant="System.Data.SQLite.EF6" />
      <add name="SQLite Data Provider (Entity Framework 6)" invariant="System.Data.SQLite.EF6" description=".NET Framework Data Provider for SQLite (Entity Framework 6)" type="System.Data.SQLite.EF6.SQLiteProviderFactory, System.Data.SQLite.EF6" />
    </DbProviderFactories>
  </system.data>
  <entityFramework>
    <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
      <parameters>
        <parameter value="v12.0" />
      </parameters>
    </defaultConnectionFactory>
    <providers>
      <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
      <provider invariantName="System.Data.SQLite.EF6" type="System.Data.SQLite.EF6.SQLiteProviderServices, System.Data.SQLite.EF6" />
    </providers>
  </entityFramework>
  <connectionStrings>
    <add name="EsiEntities" connectionString="metadata=res://*/EntityDataModel.ModelEsi.csdl|res://*/EntityDataModel.ModelEsi.ssdl|res://*/EntityDataModel.ModelEsi.msl;provider=System.Data.SQLite.EF6;provider connection string=&quot;data source=D:\ECAT\Project\EIPBuilder\bin\Release\Database\ECAT\Esi.db&quot;" providerName="System.Data.EntityClient" />
  </connectionStrings>
</configuration>
~~~

這裡可以發現， data source= 絕對路徑寫死，

那不就代表 我到時候 release 出去 問題一堆，

所以 連到 sqlite資料庫 的連線字串 ，必須要可以修改

----------


## 引用 ##

![](..\images\2016-05-26-CSharp_EF6_SQLite_Appconfig\SOPGmMH.png)

~~~csharp
using System.Configuration;
~~~


----------



~~~csharp
            ////Find the app.config path
            var exeConfigurationFileMap = new ExeConfigurationFileMap();
            exeConfigurationFileMap.ExeConfigFilename = @"D:\ECAT\UnitTest\EIPBuilderTests\app.config";
            var config = ConfigurationManager.OpenMappedExeConfiguration(exeConfigurationFileMap, ConfigurationUserLevel.None);

            string databaseName = "EsiEntities";
            //string connectionString = @"metadata=res://*/EntityDataModel.ModelEsi.csdl|res://*/EntityDataModel.ModelEsi.ssdl|res://*/EntityDataModel.ModelEsi.msl;provider=System.Data.SQLite.EF6;provider connection string=""data source=D:\ECAT\Project\EIPBuilder\bin\Release\Database\ECAT\Esi.db;foreign keys=true;""";
            string connectionString = @"metadata=res://*/EntityDataModel.ModelEsi.csdl|res://*/EntityDataModel.ModelEsi.ssdl|res://*/EntityDataModel.ModelEsi.msl;provider=System.Data.SQLite.EF6;provider connection string=""data source={@db};foreign keys=true;""";
            connectionString = connectionString.Replace("{@db}", @"D:\ECAT\Project\EIPBuilder\bin\Release\Database\ECAT\Esi.db");

            config.ConnectionStrings.ConnectionStrings[databaseName].ConnectionString = connectionString;
            config.Save(ConfigurationSaveMode.Modified, true);
            ConfigurationManager.RefreshSection("connectionStrings");
~~~



----------

## 參考： ##

- https://msdn.microsoft.com/en-us/library/bb738533(v=vs.100).aspx
- http://a-jau.blogspot.tw/2013/08/centity-frame-work-connection-string.html
- http://blog.csdn.net/wusuopubupt/article/details/8817826
- https://msdn.microsoft.com/zh-tw/library/system.data.sqlclient.sqlconnectionstringbuilder(v=vs.110).aspx
- http://blog.miniasp.com/post/2015/11/23/How-Class-library-read-config-from-webconfig-or-appconfig-file.aspx
- http://stackoverflow.com/questions/7674318/cascade-on-delete-not-cascading-with-ef
- http://stackoverflow.com/questions/4254371/enabling-foreign-key-constraints-in-sqlite
