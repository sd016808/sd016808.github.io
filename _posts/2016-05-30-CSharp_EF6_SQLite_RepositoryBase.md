---
layout: post
title: 'C# 與 EF6 於 SQLite 用繼承方式實作 Repository Pattern'
author: 'James Peng'
tags: ['Entity Framework']
---


## Repository Pattern ##

這個Pattern概念非常簡單，Repository其實有儲存庫的意思，所以這個Pattern的意思是，把實際的DAL層透過所謂的Repository封裝之後，從外面的角度來說是和Repository 溝通來取得資料，至於Repository的資料來源是那裡，就不管了。

![](..\images\2016-05-30-CSharp_EF6_SQLite_RepositoryBase\jNPP7yN.png)

圖片來源： [Implementing the Repository and Unit of Work Patterns in an ASP.NET MVC Application (9 of 10)](http://www.asp.net/mvc/tutorials/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)


----------


## 定義 Repository 的 Base class ##

承上篇的作法是使用 interface，例如寫一個 IRepositoryBase.cs

但是 結果只有一個 Repository.cs 實作他，

根本沒人會用到 這 interface 感覺很像多寫的，甚至會被拿去誤用 

這時候 把 interface 和 實作他的 Repository class 直接一起寫成一個 abstract 抽象 base class

因為設為 abstract 所以只能被 繼承，

然後再由其他的子類別去override內部方法，才能 new 出實體



## RepositoryBase.cs ##

~~~csharp
using System;
using System.Data.Entity;
using System.Linq;
using System.Linq.Expressions;

namespace EntityController
{
    public abstract class RepositoryBase<T> where T : class
    {
        protected IDbSet<T> dbSet { get; set; }
        protected DbContext dbContext { get; set; }

        /// <summary>
        /// 建構EF一個Entity的Repository，需傳入此Entity的Context。
        /// </summary>
        /// <param name="inContext">Entity所在的Context</param>
        public RepositoryBase(DbContext inContext)
        {
            dbContext = inContext;
            dbSet = dbContext.Set<T>();
        }

        /// <summary> 
        /// 檢查是否存在任何物件 
        /// </summary> 
        /// <typeparam name="T"></typeparam> 
        /// <returns></returns> 
        public virtual bool Any()
        {
            return dbSet.Any();
        }

        /// <summary> 
        /// 檢查是否存在任何符合條件的物件 
        /// </summary> 
        /// <typeparam name="T"></typeparam> 
        /// <param name="predicate"></param> 
        /// <returns></returns> 
        public virtual bool Any(Expression<Func<T, bool>> predicate)
        {
            return dbSet.Any(predicate);
        }

        /// <summary>
        /// 新增一筆資料到資料庫。
        /// </summary>
        /// <param name="entity">要新增到資料的庫的Entity</param>
        public virtual void Add(T entity)
        {
            dbSet.Add(entity);
        }

        /// <summary>
        /// 取得第一筆符合條件的內容。如果符合條件有多筆，也只取得第一筆。
        /// </summary>
        /// <param name="predicate">要取得的Where條件。</param>
        /// <returns>取得第一筆符合條件的內容。</returns>
        public virtual T Read(Expression<Func<T, bool>> predicate)
        {
            return dbSet.Where(predicate).FirstOrDefault();
        }

        /// <summary>
        /// 取得Entity全部筆數的IQueryable。
        /// </summary>
        /// <returns>Entity全部筆數的IQueryable。</returns>
        public virtual IQueryable<T> GetAll()
        {
            return dbSet.AsQueryable();
        }

        public virtual IQueryable<T> GetAll(Expression<Func<T, bool>> predicate)
        {
            return dbSet.Where(predicate).AsQueryable();
        }

        /// <summary> 
        /// 回傳第一筆 
        /// </summary> 
        /// <returns></returns> 
        public virtual T FirstOrDefault()
        {
            return dbSet.FirstOrDefault();
        }

        /// <summary> 
        /// 回傳符合條件的第一筆 
        /// </summary> 
        /// <param name="predicate"></param> 
        /// <returns></returns> 
        public virtual T FirstOrDefault(Expression<Func<T, bool>> predicate)
        {
            return dbSet.FirstOrDefault(predicate);
        }

        /// <summary>
        /// 更新一筆Entity內容。
        /// </summary>
        /// <param name="entity">要更新的內容</param>
        public virtual void Update(T entity)
        {
            dbContext.Entry<T>(entity).State = EntityState.Modified;
        }

        /// <summary>
        /// 更新一筆Entity的內容。只更新有指定的Property。
        /// </summary>
        /// <param name="entity">要更新的內容。</param>
        /// <param name="updateProperties">需要更新的欄位。</param>
        public virtual void Update(T entity, Expression<Func<T, object>>[] updateProperties)
        {
            dbContext.Configuration.ValidateOnSaveEnabled = false;

            dbContext.Entry<T>(entity).State = EntityState.Unchanged;

            if (updateProperties != null)
            {
                foreach (var property in updateProperties)
                {
                    dbContext.Entry<T>(entity).Property(property).IsModified = true;
                }
            }
        }

        /// <summary>
        /// 刪除一筆資料內容。
        /// </summary>
        /// <param name="entity">要被刪除的Entity。</param>
        public virtual void Delete(T entity)
        {
            dbContext.Entry<T>(entity).State = EntityState.Deleted;
        }

        public virtual void Delete(Expression<Func<T, bool>> predicate)
        {
            var entities = dbSet.Where(predicate).AsQueryable();

            foreach (var entity in entities)
            {
                dbContext.Entry<T>(entity).State = EntityState.Deleted;
            }   
        }

        public virtual void Delete(IQueryable<T> entities)
        {
            foreach (var entity in entities)
            {
                dbContext.Entry<T>(entity).State = EntityState.Deleted;
            }            
        }

        /// <summary>
        /// 儲存異動。
        /// </summary>
        public virtual int SaveChanges()
        {
            int rtn = dbContext.SaveChanges();

            // 因為Update 單一model需要先關掉validation，因此重新打開
            if (dbContext.Configuration.ValidateOnSaveEnabled == false)
            {
                dbContext.Configuration.ValidateOnSaveEnabled = true;
            }

            return rtn;
        }
    }
}

~~~


----------


附註：

有的人會在

~~~csharp
            dbContext.Entry<T>(entity).State = EntityState.Modified;
~~~

之前加上

~~~csharp
            dbSet.Attach(entity);
            dbContext.Entry<T>(entity).State = EntityState.Modified;
~~~

事實證明，我們也不用多寫Attach，只要變更State的時候，他就會自動幫我們Attach去上

參考： http://blog.sanc.idv.tw/2014/08/entity-framework-attach.html

----------

## ModelEsiRepository.cs ##

然後 一個 DB 裡面的 Table，要對應一個 Repository

例如：我有一個 Table 叫 ModelEsi，就多寫一個 ModelEsiRepository.cs 繼承 RepositoryBase<T>

就沒了，省下很多 用不到的 interface


~~~csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;
using System.Linq.Expressions;
using System.Text;

namespace EntityController
{    
    public class ModelEsiRepository<T> : RepositoryBase<T> where T : class
    {
        public ModelEsiRepository(DbContext inContext) : base(inContext)
        {

        }
    }
}
~~~


----------

## RepositoryFactory.cs ##

然後再寫一個 Repository Factory 用來產生不同 Database 不同 table 的 RepositoryBase 實體

~~~csharp
using System;
using System.Collections.Generic;
using System.Configuration;
using System.Data.Entity.Core.EntityClient;
using System.Data.SqlClient;
using System.Diagnostics;
using System.IO;
using System.Linq;
using System.Reflection;
using System.Text;

namespace EntityController
{
    public class RepositoryFactory
    {
        private const string ENTITY_NAME = "EsiEntities";
        static private string connectionString = string.Empty;

        public enum DbName
        {
            ESI = 0,
        }

        public static RepositoryBase<T> Create<T>(string dbPath, DbName dbName = DbName.ESI, string configPath = null, bool foreignKey = true) where T : class
        {
            SwitchDbPath(dbPath, configPath, foreignKey);
            GetConnectionString(ref connectionString, configPath);

            switch (dbName)
            {
                case DbName.ESI:
                    {
                        EsiEntities db = new EsiEntities(connectionString);
                        return new ModelEsiRepository<T>(db);
                    }
                default:
                    {
                        EsiEntities db = new EsiEntities(connectionString);
                        return new ModelEsiRepository<T>(db);
                    }                    
            }
        }

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
    }
}

~~~


----------

## select 用法 ##

~~~csharp
                var objectType = RepositoryFactory.Create<Table_ObjectType>("TestDb");

                //Reads all
                var query = objectType.GetAll();
                foreach (var tmp in query)
                {
                    var test = tmp.Name;
                }

                //Read where id==4
                var query2 = objectType.Read(x => x.id == 4);
                try
                {
                    var test2 = query2.Name;
                }
                catch(Exception e)
                {

                }

                //FirstOrDefault
                var queryFirstOrDefault = objectType.FirstOrDefault();
                var test1 = queryFirstOrDefault.Name;

                //FirstOrDefault where id==3
                var queryFirstOrDefault2 = objectType.FirstOrDefault(x => x.id == 3);
                var test12 = queryFirstOrDefault2.Name;
~~~

----------


## delete 用法 ##

用法一

~~~csharp
                var objectType = RepositoryFactory.Create<Table_ObjectType>("TestDb");
                var item = objectType.Reads(x => x.Name == "James.Peng");
                objectType.Delete(item);
                objectType.SaveChanges();
~~~


用法二

~~~csharp
        public void TestObject_Delete()
        {
            var rrr = RepositoryFactory.Create<Table_ObjectType>("TestDb");
            rrr.Delete(x => x.id == 1);
            try
            {
                int rtn = rrr.SaveChanges();
            }
            catch (Exception ex)
            {
                Debug.WriteLine(ex.Message);
            }
        }
~~~


----------

## update 用法 ##

~~~csharp
                var objectType = RepositoryFactory.Create<Table_ObjectType>("TestDb");
                var item2 = objectType.GetAll(x => x.Name == "James.Peng");
                foreach (var i in item2)
                {
                    i.Name += "2";
                    objectType.Update(i);
                }
                objectType.SaveChanges(); 
~~~

----------

## insert 用法 ##

~~~csharp
                var objectType = RepositoryFactory.Create<Table_ObjectType>("TestDb");
                for (int i = 0; i < 100; i++)
                {
                    Table_ObjectType inserItem = new Table_ObjectType { Index = "0", Name = "James.Peng", Type = "James.Peng", BitSize = "James.Peng" };
                    objectType.Add(inserItem);
                }

                objectType.SaveChanges();
~~~

----------

## 參考 ##

- http://ithelp.ithome.com.tw/articles/10157484
- http://www.asp.net/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
- http://blog.sanc.idv.tw/2014/08/entity-framework-attach.html
- https://msdn.microsoft.com/en-us/data/jj592676.aspx
