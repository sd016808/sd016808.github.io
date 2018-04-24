---
layout: post
title: 'C# 使用 EF6 於 SQLite 動態切換 DB 檔案來源'
author: 'James Peng'
tags: ['Entity Framework']
---


## 切換 db 路徑 ##

- dbPath：sqlite 檔案路徑
- configPath：可以指定特定 App.config，不帶路徑的話就是使用預設的 App.config，會更新 ConnectionString 到此 config
- foreignKey：是否使用外鍵

~~~csharp
        private static void SwitchDbPath(string dbPath, string configPath = null, bool foreignKey = true)
        {
            string providerConnectionString = @"Data Source={@dbPath};foreign keys={@foreignKey};";
            providerConnectionString = providerConnectionString.Replace("{@dbPath}", dbPath);
            providerConnectionString = providerConnectionString.Replace("{@foreignKey}", foreignKey.ToString());

            string connectionString = string.Empty;
            Configuration config = GetConnectionString(ref connectionString, configPath);
            EntityConnectionStringBuilder efb = new EntityConnectionStringBuilder(connectionString);
            efb.ProviderConnectionString = providerConnectionString;

            config.ConnectionStrings.ConnectionStrings[ENTITY_NAME].ConnectionString = efb.ConnectionString;
            config.Save(ConfigurationSaveMode.Modified, true);
            ConfigurationManager.RefreshSection("connectionStrings");
        }
~~~

----------


## 取得 ConnectionString ##

- configPath：可以指定特定 App.config，不帶路徑的話就是使用預設的 App.config 

~~~csharp
private static Configuration GetConnectionString(ref string connectionString, string configPath = null)
        {
            Configuration config;
            if (configPath == null)
            {
                config = ConfigurationManager.OpenExeConfiguration(ConfigurationUserLevel.None);
            }
            else
            {
                var exeConfigurationFileMap = new ExeConfigurationFileMap();
                exeConfigurationFileMap.ExeConfigFilename = configPath;
                config = ConfigurationManager.OpenMappedExeConfiguration(exeConfigurationFileMap, ConfigurationUserLevel.None);
            }

            connectionString = config.ConnectionStrings.ConnectionStrings[ENTITY_NAME].ConnectionString;
            return config;
        }
~~~

這樣就可以了
