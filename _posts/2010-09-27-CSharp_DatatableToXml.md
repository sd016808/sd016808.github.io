---
layout: post
title: 'C# DataTable 轉換成 XML'
author: 'James Peng'
tags: ['C#','ASP.NET']
---

## using ##

~~~csharp
using System;
using System.Data;
using System.IO;
using System.Xml;
using System.Text;
~~~

----------


## 將DataTable對象轉換成XML字符串 ##

~~~csharp
  public static string CDataToXml(DataTable dt)
        {
            if (dt != null)
            {
                MemoryStream ms = null;
                XmlTextWriter XmlWt = null;
                try
                {
                    ms = new MemoryStream();
                    //根據ms實例化XmlWt
                    XmlWt = new XmlTextWriter(ms, Encoding.Unicode);
                    //獲取ds中的數據
                    dt.WriteXml(XmlWt);
                    int count = (int)ms.Length;
                    byte[] temp = new byte[count];
                    ms.Seek(0, SeekOrigin.Begin);
                    ms.Read(temp, 0, count);
                    //返回Unicode編碼的文本
                    UnicodeEncoding ucode = new UnicodeEncoding();
                    string returnValue = ucode.GetString(temp).Trim();
                    return returnValue;
                }
                catch (System.Exception ex)
                {
                    throw ex;
                }
                finally
                {
                    //釋放資源
                    if (XmlWt != null)
                    {
                        XmlWt.Close();
                        ms.Close();
                        ms.Dispose();
                    }
                }
            }
            else
            {
                return "";
            }
        }
~~~

----------






## 將DataSet對像數據保存為XML文件 ##

~~~csharp
        public static bool CDataToXmlFile(DataTable dt, string xmlFilePath)
        {
            if ((dt != null) &amp;&amp; (!string.IsNullOrEmpty(xmlFilePath)))
            {
                string path = HttpContext.Current.Server.MapPath(xmlFilePath);
                MemoryStream ms = null;
                XmlTextWriter XmlWt = null;
                try
                {
                    ms = new MemoryStream();
                    //根據ms實例化XmlWt
                    XmlWt = new XmlTextWriter(ms, Encoding.Unicode);
                    //獲取ds中的數據
                    dt.WriteXml(XmlWt);
                    int count = (int)ms.Length;
                    byte[] temp = new byte[count];
                    ms.Seek(0, SeekOrigin.Begin);
                    ms.Read(temp, 0, count);
                    //返回Unicode編碼的文本
                    UnicodeEncoding ucode = new UnicodeEncoding();
                    //寫文件
                    StreamWriter sw = new StreamWriter(path);
                    sw.WriteLine("&lt;?xml version="1.0" encoding="utf-8"?&gt;");
                    sw.WriteLine(ucode.GetString(temp).Trim());
                    sw.Close();
                    return true;
                }
                catch (System.Exception ex)
                {
                    throw ex;
                }
                finally
                {
                    //釋放資源
                    if (XmlWt != null)
                    {
                        XmlWt.Close();
                        ms.Close();
                        ms.Dispose();
                    }
                }
            }
            else
            {
                return false;
            }
        }
~~~


----------




## 將Xml內容字符串轉換成DataSet對象 ##

~~~csharp
 public static DataSet CXmlToDataSet(string xmlStr)
        {
            if (!string.IsNullOrEmpty(xmlStr))
            {
                StringReader StrStream = null;
                XmlTextReader Xmlrdr = null;
                try
                {
                    DataSet ds = new DataSet();
                    //讀取字符串中的信息
                    StrStream = new StringReader(xmlStr);
                    //獲取StrStream中的數據
                    Xmlrdr = new XmlTextReader(StrStream);
                    //ds獲取Xmlrdr中的數據
                    ds.ReadXml(Xmlrdr);
                    return ds;
                }
                catch (Exception e)
                {
                    throw e;
                }
                finally
                {
                    //釋放資源
                    if (Xmlrdr != null)
                    {
                        Xmlrdr.Close();
                        StrStream.Close();
                        StrStream.Dispose();
                    }
                }
            }
            else
            {
                return null;
            }
        }
~~~
----------



## 讀取Xml文件信息,並轉換成DataSet對象 ##

~~~csharp

        public static DataSet CXmlFileToDataSet(string xmlFilePath)
        {
            if (!string.IsNullOrEmpty(xmlFilePath))
            {
                string path = HttpContext.Current.Server.MapPath(xmlFilePath);
                StringReader StrStream = null;
                XmlTextReader Xmlrdr = null;
                try
                {
                    XmlDocument xmldoc = new XmlDocument();
                    //根據地址加載Xml文件
                    xmldoc.Load(path);
                    DataSet ds = new DataSet();
                    //讀取文件中的字符流
                    StrStream = new StringReader(xmldoc.InnerXml);
                    //獲取StrStream中的數據
                    Xmlrdr = new XmlTextReader(StrStream);
                    //ds獲取Xmlrdr中的數據
                    ds.ReadXml(Xmlrdr);
                    return ds;
                }
                catch (Exception e)
                {
                    throw e;
                }
                finally
                {
                    //釋放資源
                    if (Xmlrdr != null)
                    {
                        Xmlrdr.Close();
                        StrStream.Close();
                        StrStream.Dispose();
                    }
                }
            }
            else
            {
                return null;
            }
        }
~~~


----------




## Convert DataTable To XML ##

~~~csharp
private string ConvertDataTableToXML(DataTable xmlDS)
{
     MemoryStream stream = null;
     XmlTextWriter writer = null;
     try
     {
         stream = new MemoryStream();
         writer = new XmlTextWriter(stream, Encoding.Default);
         xmlDS.WriteXml(writer);
         int count = (int)stream.Length;
         byte[] arr = new byte[count];
         stream.Seek(0, SeekOrigin.Begin);
         stream.Read(arr, 0, count);
         UTF8Encoding utf = new UTF8Encoding();
         return utf.GetString(arr).Trim();
     }
     catch
     {
         return String.Empty;
     }
     finally
     {
         if (writer != null) writer.Close();
     }
}
~~~


----------




## Convert XML To DataSet ##

~~~csharp
private DataSet ConvertXMLToDataSet(string xmlData)
{
   StringReader stream = null;
   XmlTextReader reader = null;
   try
   {
     DataSet xmlDS = new DataSet();
     stream = new StringReader(xmlData);
     reader = new XmlTextReader(stream);
     xmlDS.ReadXml(reader);
     return xmlDS;
   }
   catch (Exception ex)
   {
     string strTest = ex.Message;
     return null;
   }
   finally
   {
     if (reader != null)
     reader.Close();
   }
}
~~~


----------


參考：

- http://minuo.blog.hexun.com/35959878_d.html
- http://www.cnblogs.com/sunrise/archive/2009/12/01/1614534.html
