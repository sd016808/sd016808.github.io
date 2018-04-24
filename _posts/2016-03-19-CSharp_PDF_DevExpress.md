---
layout: post
title: 'C# 透過 DevExpress 操作 WORD 文件'
author: 'James Peng'
tags: ['DevExpress']
---


## 引用命名空間 ##

~~~csharp
using DevExpress.Pdf;
using DevExpress.XtraRichEdit;
using DevExpress.XtraRichEdit.API.Native;
~~~


----------


## 宣告 ##

~~~csharp
        private static RichEditDocumentServer srv = new RichEditDocumentServer();
        private static Document doc = srv.Document;
~~~


----------


## 讀取 DOCX ##

~~~csharp
        public static void LoadDocx(string docPath)
        {
            doc.LoadDocument(docPath, DocumentFormat.OpenXml);
        }
~~~


----------

## 插入字串 ##

~~~csharp
        public static void InsertValue(string tag, string strText)
        {          
            doc.ReplaceAll(tag, strText, SearchOptions.CaseSensitive);            
        }
~~~

----------
## 刪除表格某ROW ##

~~~csharp


        public static void DeleteTableRow(int iTableIndex, int iRowIndex)
        {
            doc.Delete(doc.Tables[iTableIndex].Rows[iRowIndex].Range); //Clear Contents
            doc.Tables[iTableIndex].Rows[iRowIndex].Delete();          //Delete Row
        }


        public static void DeleteTableRow(string tag)
        {
            DocumentRange[] range = doc.FindAll(tag, SearchOptions.CaseSensitive);



            foreach (var row in doc.Tables[0].Rows)
            {
                if (row.Range.Start == range[0].Start)
                {
                    row.Delete();
                }
            }           
        }

        public static void DeleteTableRow(string tag , int iDeleteRowsCount)
        {
            DocumentRange[] range = doc.FindAll(tag, SearchOptions.CaseSensitive);

            int i = 0;
            foreach (var row in doc.Tables[0].Rows)
            {
                if (row.Range.Start == range[0].Start)
                {
                    int iRowIndex = i;
                    int iDoTimes = iDeleteRowsCount;
                    int iCount = 0;
                    while (iCount < iDoTimes)
                    {
                        doc.Tables[0].Rows[iRowIndex].Delete();
                        iCount++;
                    }
                    return;
                }
                i++;
            }
        }

~~~

----------

## 插入圖片 ##

~~~csharp
 public static void InsertPicture(string tag, Image myImage)
        {
            DocumentRange[] range = doc.FindAll(tag, SearchOptions.CaseSensitive);
            if (range.Length > 0)
            {
                DocumentPosition pos = range[0].Start;
                //, 660, 263);
                
                doc.Images.Insert(pos, myImage);
                doc.Images[0].ScaleX /= 2;
                doc.Images[0].ScaleY /= 2;
              
                doc.ReplaceAll(tag, "", SearchOptions.CaseSensitive);
            }
        }
~~~

----------

## 合併 ##

~~~csharp

        public static bool Merger(string[] arrCopies, string outDoc)
        {
            try
            {
                using (PdfDocumentProcessor documentProcessor = new PdfDocumentProcessor())
                {
                    for (int i = 0; i < arrCopies.Length; i++)
                    {
                        if (i == 0) documentProcessor.LoadDocument(arrCopies[i]);
                        if (i > 0) documentProcessor.AppendDocument(arrCopies[i]);
                    }                                            
                    documentProcessor.SaveDocument(outDoc);
                    documentProcessor.CloseDocument();
                }
                return true;
            }
            catch (IOException)
            {
                return false;
            }         
        }
~~~

----------

## 存檔成 DOCX ##

~~~csharp
        public static void SaveAsDocx(string docPath)
        {            
            srv.SaveDocument(docPath, DocumentFormat.OpenXml);            
        }
~~~


----------


## 存檔成 PDF ##

~~~csharp
        public static bool SaveAsPdf( string target)
        {
            KillPdfProcess();
            System.Threading.Thread.Sleep(100);
            try {
                using (var fs = new FileStream(target, FileMode.Create, FileAccess.ReadWrite))
                {
                    srv.ExportToPdf(fs); //, exportOptions);    
                    fs.Close();
                }
                return true;
            }
            catch
            {
                KillPdfProcess();
                System.Threading.Thread.Sleep(100);
                return false;
            }                    
        }
~~~



