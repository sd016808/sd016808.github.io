---
layout: post
title: 'C# 與 EF6 於 SQLite 用介面方式實作 Repository Pattern'
author: 'James Peng'
tags: ['Entity Framework']
---


## Repository Pattern ##

這個Pattern概念非常簡單，Repository其實有儲存庫的意思，所以這個Pattern的意思是，把實際的DAL層透過所謂的Repository封裝之後，從外面的角度來說是和Repository 溝通來取得資料，至於Repository的資料來源是那裡，就不管了。

![](..\images\2016-05-29-CSharp_EF6_SQLite_IRepository\jNPP7yN.png)

圖片來源： [Implementing the Repository and Unit of Work Patterns in an ASP.NET MVC Application (9 of 10)](http://www.asp.net/mvc/tutorials/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)


----------


## 定義 Repository 的 interface ##

網路上的作法比較多是使用 interface，例如寫一個 IRepository<T>


## IRepositoryBase.cs ##

~~~csharp
using System;
using System.Data.Entity;
using System.Linq;
using System.Linq.Expressions;

namespace Model.Models
{
    public interface IRepositoryBase
    {

    }

    public interface IRepository<T> : IRepositoryBase where T : class
    {
        DbContext dbContext { get; set; }

        /// <summary> 
        /// 檢查是否存在任何物件 
        /// </summary> 
        /// <typeparam name="T"></typeparam> 
        /// <returns></returns> 
        bool Any();

        /// <summary> 
        /// 檢查是否存在任何符合條件的物件 
        /// </summary> 
        /// <typeparam name="T"></typeparam> 
        /// <param name="predicate"></param> 
        /// <returns></returns> 
        bool Any(Expression<Func<T, bool>> predicate);

        /// <summary> 
        /// 建立 T 
        /// </summary> 
        /// <param name="entity"></param> 
        void Create(T entity);

        /// <summary> 
        /// 刪除 T 
        /// </summary> 
        /// <param name="entity"></param> 
        void Delete(T entity);

        /// <summary> 
        /// 根據條件尋找 T 
        /// </summary> 
        /// <param name="predicate"></param> 
        /// <returns></returns> 
        IQueryable<T> Where(Expression<Func<T, bool>> predicate);

        /// <summary> 
        /// 回傳第一筆 
        /// </summary> 
        /// <returns></returns> 
        T FirstOrDefault();

        /// <summary> 
        /// 回傳符合條件的第一筆 
        /// </summary> 
        /// <param name="predicate"></param> 
        /// <returns></returns> 
        T FirstOrDefault(Expression<Func<T, bool>> predicate);

        /// <summary> 
        /// 取得全部的 T 
        /// </summary> 
        /// <returns></returns> 
        IQueryable<T> GetAll();

        /// <summary> 
        /// 根據 ID 尋找 T 
        /// </summary> 
        /// <param name="id"></param> 
        /// <returns></returns> 
        T GetByID(params object[] keyValues);

        /// <summary> 
        /// 更新 T 
        /// </summary> 
        /// <param name="entity"></param> 
        void Update(T entity);
    }
}
~~~

這個 IRepository.cs 永遠只有一個人實作就是 Repository.cs

----------

## Repository.cs ##

Repository.cs 裡寫一個 抽象類別 abstract class Repository<T> 實作 IRepository<T>


~~~csharp
using System;
using System.Data.Entity;
using System.Linq;
using System.Linq.Expressions;

namespace Model.Models
{
    public abstract class Repository<T> : IRepository<T> where T : class
    {
        protected IDbSet<T> dbSet { get; set; }
        public DbContext dbContext { get; set; }

        public Repository(DbContext context)
        {
            dbContext = context;
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
        /// 建立 T 
        /// </summary> 
        /// <param name="entity"></param> 
        public virtual void Create(T entity)
        {
            dbSet.Add(entity);
            dbContext.SaveChanges();
        }

        /// <summary> 
        /// 刪除 T 
        /// </summary> 
        /// <param name="entity"></param> 
        public virtual void Delete(T entity)
        {
            if (dbContext.Entry(entity).State == EntityState.Detached)
            {
                dbSet.Attach(entity);
            }
            dbSet.Remove(entity);
            dbContext.SaveChanges();
        }

        /// <summary> 
        /// 根據條件尋找 T 
        /// </summary> 
        /// <param name="predicate"></param> 
        /// <returns></returns> 
        public virtual IQueryable<T> Where(Expression<Func<T, bool>> predicate)
        {
            return dbSet.Where(predicate);
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
        /// 取得全部的 T 
        /// </summary> 
        /// <returns></returns> 
        public virtual IQueryable<T> GetAll()
        {
            return dbSet;
        }

        /// <summary> 
        /// 根據 ID 尋找 T 
        /// </summary> 
        /// <param name="id"></param> 
        /// <returns></returns> 
        public virtual T GetByID(params object[] keyValues)
        {
            return dbSet.Find(keyValues);
        }

        /// <summary> 
        /// 更新 T 
        /// </summary> 
        /// <param name="entity"></param> 
        public virtual void Update(T entity)
        {
            dbSet.Attach(entity);
            dbContext.Entry(entity).State = EntityState.Modified;
            dbContext.SaveChanges();
        }
    }
}
~~~


----------

## BrandRepository.cs ##

然後 一個 DB 裡面的 Table，要繼承一個 Repository

例如：我有一個 Table 叫 Brand，就多寫一個 BrandRepository.cs 繼承 

除了實作 Repository<Brand> 外， 還預留一個 IBrandRepository 擴充用

~~~csharp
using System.Data.Entity;

namespace Model.Models
{
    public partial class BrandRepository : Repository<Brand>, IBrandRepository
    {
        public BrandRepository(DbContext _context)
            : base(_context)
        {
        }
    }
}
~~~

## IBrandRepository.cs ##

~~~csharp
namespace Model.Models
{
    public interface IBrandRepository : IRepository<Brand>
    {
    }
}
~~~

----------

## 參考 ##

- http://ithelp.ithome.com.tw/articles/10157484
- http://www.asp.net/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
