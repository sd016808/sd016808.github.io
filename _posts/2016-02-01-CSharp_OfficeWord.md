---
layout: post
title: 'C# 透過 Microsoft.Office.Interop.Word 操作 Docx 文件'
author: 'James Peng'
tags: ['C# 3rd-Party Library']
---

## 參考 ##

![](..\images\2016-02-01-CSharp_OfficeWord\DtpIJPR.png)


----------


## MsOfficeWordAccessHelper.cs ##

~~~csharp
using DocumentFormat.OpenXml;
using DocumentFormat.OpenXml.Packaging;
using DocumentFormat.OpenXml.Wordprocessing;
using Microsoft.Office.Interop.Word;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Reflection;
using System.Text;
using System.Threading;
using System.Threading.Tasks;


namespace DataAccessComponents
{
    public class MsOfficeWordAccessHelper
    {
        private static WordReport report = new WordReport();


        public static void KillWinWordProcess()
        {
            System.Diagnostics.Process[] processes = System.Diagnostics.Process.GetProcessesByName("WINWORD");
            foreach (System.Diagnostics.Process process in processes)
            {
                if (process.MainWindowTitle.IndexOf("Microsoft Word") >=0 )
                {
                    process.Kill();
                }
            }
        }

        public static void KillWinWordTempFile(string path)
        {
            string wordPath = Path.GetDirectoryName(path);
            string[] files = Directory.GetFiles(wordPath);
            foreach (string file in files)
            {
                if (file.IndexOf("~$") >= 0)
                {
                    File.Delete(file);                    
                }
            }
        }


        public static void CopyWord(string templetPath, string savePath)
        {
            KillWinWordProcess();
            KillWinWordTempFile(templetPath);
            Thread.Sleep(10);
            report.Open(templetPath);
            report.SavedAs(savePath);
        }

        public static void InsertValue(string docPath, string tag, string strText)
        {
            
            report.Open(docPath);
            //report.InsertValueByBookmarks(tag, strText);
            report.InsertValueByString(tag, strText);
            report.SavedAs(docPath);
        }

        public static void InsertPicture(string docPath, string tag, string strPath)
        {
                       
            report.Open(docPath);
            System.Drawing.Image image = System.Drawing.Image.FromFile(strPath);
            int width = image.Width;
            int height = image.Height;
            report.InsertPictureByString(tag, strPath, width, height);
            report.SavedAs(docPath);
        }

        public static bool MergerWord(string[] arrCopies, string outDoc)
        {                              

            Object missing = Missing.Value;
            object oOutputDoc = outDoc;
            Microsoft.Office.Interop.Word.Application wordApp = new Microsoft.Office.Interop.Word.Application();
            object objTempDoc = arrCopies[0];
            wordApp.Documents.Open(ref objTempDoc,
                                                ref missing, ref missing, ref missing,
                                                ref missing, ref missing, ref missing,
                                                ref missing, ref missing, ref missing,
                                                ref missing, ref missing, ref missing,
                                                ref missing, ref missing, ref missing);
            wordApp.Application.Selection.EndKey(WdUnits.wdStory);
            
            object oPageBreak = Microsoft.Office.Interop.Word.WdBreakType.wdPageBreak;
            wordApp.Selection.InsertBreak(ref oPageBreak);
            wordApp.Application.Selection.EndKey(WdUnits.wdStory);

            string[] files = arrCopies;//Directory.GetFiles(sourcePath.Text.Trim());

           try
           {

            for (int iIdx = 1; iIdx < files.Length; iIdx++)
            {
                wordApp.Selection.InsertFile(files[iIdx], ref missing, ref missing, ref missing, ref missing);
                if (iIdx != files.Length - 1)
                {
                    wordApp.Selection.InsertBreak(ref oPageBreak);
                }
                wordApp.ActiveDocument.SaveAs(ref oOutputDoc,
                                                ref missing, ref missing, ref missing,
                                                ref missing, ref missing, ref missing,
                                                ref missing, ref missing, ref missing,
                                                ref missing, ref missing, ref missing,
                                                ref missing, ref missing, ref missing);
            }

            return true;
          }
          catch( Exception e)
          {
              e.ToString();
              return false;
          }
          finally
          {
                wordApp.Quit(
                  ref missing,     //SaveChanges
                  ref missing,     //OriginalFormat
                  ref missing      //RoutDocument
                  );
                wordApp = null;
          }

        }


        public static void HtmlToWord(string strOpenFileName, string strSaveFileName)
        {

            string inputFile = strOpenFileName;
            string outputFile = strSaveFileName;

            using (WordprocessingDocument myDoc =
                    WordprocessingDocument.Create(
                        outputFile, WordprocessingDocumentType.Document))
            {
                string altChunkId = "AltChunkId1";
                MainDocumentPart mainPart = myDoc.AddMainDocumentPart();
                mainPart.Document = new DocumentFormat.OpenXml.Wordprocessing.Document();
                mainPart.Document.Body = new Body();                
                var chunk = mainPart.AddAlternativeFormatImportPart(
                    AlternativeFormatImportPartType.Html, altChunkId);
                using (FileStream fileStream =
                        File.Open(inputFile, FileMode.Open))
                {
                    chunk.FeedData(fileStream);
                }
                AltChunk altChunk = new AltChunk() { Id = altChunkId };
                mainPart.Document.Append(altChunk);
                mainPart.Document.Save();
            }

        }


        public static void WordToText(string strOpenFileName, string strSaveFileName)
        {

            object openFileName = strOpenFileName;
            object saveFileName = strSaveFileName;
            object format = Microsoft.Office.Interop.Word.WdSaveFormat.wdFormatUnicodeText;
            object missing = System.Reflection.Missing.Value;

            Microsoft.Office.Interop.Word.Application myWord = new Microsoft.Office.Interop.Word.Application();
            myWord.Visible = false;
            myWord.Documents.Open(ref openFileName,
                            ref missing, ref missing, ref missing, ref missing, ref missing,
                            ref missing, ref missing, ref missing, ref missing, ref missing,
                            ref missing, ref missing, ref missing, ref missing, ref missing
                        );
            myWord.ActiveDocument.SaveAs(ref saveFileName, ref format,
                            ref missing, ref missing, ref missing, ref missing, ref missing,
                            ref missing, ref missing, ref missing, ref missing, ref missing,
                            ref missing, ref missing, ref missing, ref missing
                        );
            myWord.Quit(ref missing, ref missing, ref missing);
        }


        public static void WordToHtml(string strOpenFileName, string strSaveFileName)
        {

            object openFileName = strOpenFileName;     
            object saveFileName = strSaveFileName;    
            object format = Microsoft.Office.Interop.Word.WdSaveFormat.wdFormatHTML;    
            object missing = System.Reflection.Missing.Value;

            Microsoft.Office.Interop.Word.Application myWord = new Microsoft.Office.Interop.Word.Application();
            myWord.Visible = false;
            myWord.Documents.Open(ref openFileName,
                            ref missing, ref missing, ref missing, ref missing, ref missing,
                            ref missing, ref missing, ref missing, ref missing, ref missing,
                            ref missing, ref missing, ref missing, ref missing, ref missing
                        );
            myWord.ActiveDocument.SaveAs(ref saveFileName, ref format,
                            ref missing, ref missing, ref missing, ref missing, ref missing,
                            ref missing, ref missing, ref missing, ref missing, ref missing,
                            ref missing, ref missing, ref missing, ref missing
                        );
            myWord.Quit(ref missing, ref missing, ref missing);
        }



        public static void ExcelToHtml(string strOpenFileName, string strSaveFileName)
        {

            string copyPath = strOpenFileName;     
            string savePath = strSaveFileName;    
            object format = Microsoft.Office.Interop.Excel.XlFileFormat.xlHtml;  
            object missing = System.Reflection.Missing.Value;

            Microsoft.Office.Interop.Excel.Application myExcel = new Microsoft.Office.Interop.Excel.Application();
            myExcel.Visible = false;
            myExcel.Workbooks.Open(copyPath,
                        missing, missing, missing, missing, missing,
                        missing, missing, missing, missing, missing,
                        missing, missing, missing, missing
                    );
            myExcel.ActiveWorkbook.SaveAs(savePath, format,
                        null, null, false, false,
                        Microsoft.Office.Interop.Excel.XlSaveAsAccessMode.xlNoChange,
                        null, null, null, null, null
                    );
            myExcel.Quit();
        }


        /// <summary>
        /// Word文件轉PDF文件
        /// </summary>
        /// <param name="source">Word文件位置</param>
        /// <param name="target">PDF文件位置</param>
        /// <returns></returns>
        public static bool WordToPdf(string source, string target)
        {
            try
            {
                object missing = Missing.Value;
                var WordApp = new Microsoft.Office.Interop.Word.Application();
                Microsoft.Office.Interop.Word.Document doc = WordApp.Documents.Open(source);
                doc.SaveAs(target, Microsoft.Office.Interop.Word.WdSaveFormat.wdFormatPDF);
                WordApp.Visible = false;
                //WordApp.Quit();
                
                // Close the Word document, but leave the Word application open.
                // doc has to be cast to type _Document so that it will find the
                // correct Close method.                
                object saveChanges = WdSaveOptions.wdDoNotSaveChanges;
                ((_Document)doc).Close(ref saveChanges, ref missing, ref missing);                

                
                WordApp.Quit(ref missing, ref missing, ref missing);
                return true;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
            return false;
        }

        /// <summary>
        /// Excel文件轉PDF文件
        /// </summary>
        /// <param name="source">Excel文件位置</param>
        /// <param name="target">PDF文件位置</param>
        /// <returns></returns>
        public static bool ExcelToPdf(string source, string target)
        {
            try
            {
                var ExcelApp = new Microsoft.Office.Interop.Excel.Application();
                Microsoft.Office.Interop.Excel.Workbook book = ExcelApp.Workbooks.Open(source);
                Microsoft.Office.Interop.Excel.XlFileFormat xlFormatPDF = (Microsoft.Office.Interop.Excel.XlFileFormat)57;
                book.SaveAs(target, xlFormatPDF);
                ExcelApp.Visible = false;
                ExcelApp.Quit();
                return true;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
            return false;
        }

        /// <summary>
        /// PowerPoint文件轉PDF文件
        /// </summary>
        /// <param name="source">PowerPoint文件位置</param>
        /// <param name="target">PDF文件位置</param>
        /// <returns></returns>
        public static bool PowerPointToPdf(string source, string target)
        {
            try
            {
                var PowerPointApp = new Microsoft.Office.Interop.PowerPoint.Application();
                Microsoft.Office.Interop.PowerPoint.Presentation presentation = PowerPointApp.Presentations.Open(source, Microsoft.Office.Core.MsoTriState.msoFalse, Microsoft.Office.Core.MsoTriState.msoFalse, Microsoft.Office.Core.MsoTriState.msoFalse);
                presentation.SaveAs(target, Microsoft.Office.Interop.PowerPoint.PpSaveAsFileType.ppSaveAsPDF);
                PowerPointApp.Quit();
                System.Runtime.InteropServices.Marshal.FinalReleaseComObject(PowerPointApp);
                return true;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
            return false;
        }

    }
}

~~~


----------

## WordReport.cs ##

~~~csharp
using System;
using System.Collections.Generic;
using System.Text;
using Microsoft.Office.Interop.Word ;
 
namespace DataAccessComponents
{

    class WordReport
    {
        private Microsoft.Office.Interop.Word.Application wordApp = null;
        private Microsoft.Office.Interop.Word.Document wordDoc = null;
        public Microsoft.Office.Interop.Word.Application application
        {
            get
            {
                return wordApp;
            }
            set
            {
                wordApp = value;
            }
        }
        public Microsoft.Office.Interop.Word.Document document
        {
            get
            {
                return wordDoc;
            }
            set
            {
                wordDoc = value;
            }
        }
 
        //通過模板創建新文檔
        public void Open(string filepath)
        {
            killwinwordprocess();
            wordApp = new ApplicationClass();
            wordApp.DisplayAlerts = WdAlertLevel.wdAlertsNone;
            wordApp.Visible = false;
            object missing = System.Reflection.Missing.Value;
            object templatename = filepath;
            wordDoc = wordApp.Documents.Open(ref templatename, ref missing,
                ref missing, ref missing, ref missing, ref missing, ref missing,
                ref missing, ref missing, ref missing, ref missing, ref missing,
                ref missing, ref missing, ref missing, ref missing);
        }
 
        //保存新文件
        public void SavedAs(string filepath)
        {
            object filename = filepath;
            //object format = WdSaveFormat.wdFormatDocument;//保存格式
            object miss = System.Reflection.Missing.Value;
            wordDoc.SaveAs(ref filename, ref miss, ref miss,
                ref miss, ref miss, ref miss, ref miss,
                ref miss, ref miss, ref miss, ref miss,
                ref miss, ref miss, ref miss, ref miss,
                ref miss);

            //關閉wordDoc，wordApp對像
            object savechanges = WdSaveOptions.wdSaveChanges;
            object originalformat = WdOriginalFormat.wdOriginalDocumentFormat;
            object routedocument = false;
            wordDoc.Close(ref savechanges, ref originalformat, ref routedocument);
            wordApp.Quit(ref savechanges, ref originalformat, ref routedocument);
        }
 
        //在書籤處插入值
        public bool InsertValueByBookmarks(string bookmark, string value)
        {
            object bkobj = bookmark;
            if (wordApp.ActiveDocument.Bookmarks.Exists(bookmark))
            {
                wordApp.ActiveDocument.Bookmarks.get_Item(ref bkobj).Select();
                wordApp.Selection.TypeText(value);
                return true;
            }
            return false;
        }

        public bool InsertValueByString(string tag, string value)
        {
            try
            {
                object missValue = System.Reflection.Missing.Value;
                object objReplaceOption = WdReplace.wdReplaceAll;
                object oStory=WdUnits.wdStory;
                object oMove=WdMovementType.wdMove;

                wordApp.Selection.HomeKey(ref oStory, ref oMove);
                Find find = wordApp.Selection.Find;
                find.ClearFormatting();
                find.Replacement.ClearFormatting();

                find.Text = tag;
                find.Replacement.Text = value;

                return find.Execute(
                ref missValue,
                ref missValue,
                ref missValue,
                ref missValue,
                ref missValue,
                ref missValue,
                ref missValue,
                ref missValue,
                ref missValue,
                ref missValue,
                ref objReplaceOption,
                ref missValue,
                ref missValue,
                ref missValue,
                ref missValue
                );
            }
            catch (Exception ex)
            {
                //throw new Exception("error:" + ex.Message);
                return false;
            }        

        }

        public bool InsertPictureByString(string tag, string picturepath, float width, float hight)
        {                                     
            try
            {
                object linktofile = false;       //圖片是否為外部鏈接
                object savewithdocument = true;  //圖片是否隨文檔一起保存
                object missValue = System.Reflection.Missing.Value;
                object objReplaceOption = WdReplace.wdReplaceAll;
                object oStory = WdUnits.wdStory;
                object oMove = WdMovementType.wdMove;

                wordApp.Selection.HomeKey(ref oStory, ref oMove);
                Find find = wordApp.Selection.Find;
                find.ClearFormatting();
                find.Replacement.ClearFormatting();

                find.Text = tag;
                find.Replacement.Text = "";

                Object end = wordDoc.Characters.Count;
                Range tmpRange1 = wordDoc.Range(0, ref end);
                Find fnd = tmpRange1.Find;
                fnd.ClearFormatting();
                fnd.Text = tag;
                fnd.Execute(ref missValue, ref missValue, ref missValue, ref missValue, ref missValue, ref missValue, ref missValue, ref missValue,
                            ref missValue, ref missValue, ref missValue, ref missValue, ref missValue, ref missValue, ref missValue);

                //tmpRange1.InlineShapes.AddPicture(picturepath, Type.Missing, Type.Missing, Type.Missing);

                tmpRange1.InlineShapes.AddPicture(picturepath, ref linktofile, ref savewithdocument, Type.Missing);
                tmpRange1.Application.ActiveDocument.InlineShapes[1].Width = width;   //設置圖片寬度
                tmpRange1.Application.ActiveDocument.InlineShapes[1].Height = hight;  //設置圖片高度               

                return find.Execute(
                ref missValue,
                ref missValue,
                ref missValue,
                ref missValue,
                ref missValue,
                ref missValue,
                ref missValue,
                ref missValue,
                ref missValue,
                ref missValue,
                ref objReplaceOption,
                ref missValue,
                ref missValue,
                ref missValue,
                ref missValue
                );
            }
            catch (Exception ex)
            {
                //throw new Exception("error:" + ex.Message);
                return false;
            }    
        }
 
        //插入表格,bookmark書籤
        public Table inserttable(string bookmark, int rows, int columns, float width)
        {
            object miss = System.Reflection.Missing.Value;
            object ostart = bookmark;
            Range range = wordDoc.Bookmarks.get_Item(ref ostart).Range;//表格插入位置
            Table newtable = wordDoc.Tables.Add(range, rows, columns, ref miss, ref miss);
            //設置表的格式
            newtable.Borders.Enable = 1;  //允許有邊框，默認沒有邊框(為0時報錯，1為實線邊框，2、3為虛線邊框，以後的數字沒試過)
            newtable.Borders.OutsideLineWidth = WdLineWidth.wdLineWidth050pt;//邊框寬度
            if (width != 0)
            {
                newtable.PreferredWidth = width;//表格寬度
            }
            newtable.AllowPageBreaks = false;
            return newtable;
        }
 
        //合併單元格 表名,開始行號,開始列號,結束行號,結束列號
        public void mergecell(Microsoft.Office.Interop.Word .Table table, int row1, int column1, int row2, int column2)
        {
            table.Cell(row1, column1).Merge(table.Cell(row2, column2));
        }
 
        //設置表格內容對齊方式 align水平方向，vertical垂直方向(左對齊，居中對齊，右對齊分別對應align和vertical的值為-1,0,1)
        public void setparagraph_table(Microsoft.Office.Interop.Word .Table table, int align, int vertical)
        {
            switch (align)
            {
                case -1: table.Range.ParagraphFormat.Alignment = WdParagraphAlignment.wdAlignParagraphLeft; break;//左對齊
                case 0: table.Range.ParagraphFormat.Alignment = WdParagraphAlignment.wdAlignParagraphCenter; break;//水平居中
                case 1: table.Range.ParagraphFormat.Alignment = WdParagraphAlignment.wdAlignParagraphRight; break;//右對齊
            }
            switch (vertical)
            {
                case -1: table.Range.Cells.VerticalAlignment = WdCellVerticalAlignment.wdCellAlignVerticalTop; break;//頂端對齊
                case 0: table.Range.Cells.VerticalAlignment = WdCellVerticalAlignment.wdCellAlignVerticalCenter; break;//垂直居中
                case 1: table.Range.Cells.VerticalAlignment = WdCellVerticalAlignment.wdCellAlignVerticalBottom; break;//底端對齊
            }
        }
 
        //設置表格字體
        public void setfont_table(Microsoft.Office.Interop.Word .Table table, string fontname, double size)
        {
            if (size != 0)
            {
                table.Range.Font.Size = Convert.ToSingle(size);
            }
            if (fontname != "")
            {
                table.Range.Font.Name = fontname;
            }
        }
 
        //是否使用邊框,n表格的序號,use是或否
        public void useborder(int n,bool use)
        {
            if (use)
            {
                wordDoc.Content.Tables[n].Borders.Enable = 1;  //允許有邊框，默認沒有邊框(為0時報錯，1為實線邊框，2、3為虛線邊框，以後的數字沒試過)
            }
            else
            {
                wordDoc.Content.Tables[n].Borders.Enable = 2;  //允許有邊框，默認沒有邊框(為0時報錯，1為實線邊框，2、3為虛線邊框，以後的數字沒試過)
            }
        }
 
        //給表格插入一行,n表格的序號從1開始記
        public void addrow(int n)
        {
            object miss = System.Reflection.Missing.Value;
            wordDoc.Content.Tables[n].Rows.Add(ref miss);
        }
 
        //給表格添加一行
        public void addrow(Microsoft.Office.Interop.Word .Table table)
        {
            object miss = System.Reflection.Missing.Value;
            table.Rows.Add(ref miss);
        }
 
        //給表格插入rows行,n為表格的序號
        public void addrow(int n, int rows)
        {
            object miss = System.Reflection.Missing.Value;
            Microsoft.Office.Interop.Word .Table table = wordDoc.Content.Tables[n];
            for (int i = 0; i < rows; i++)
            {
                table.Rows.Add(ref miss);
            }
        }
 
        //給表格中單元格插入元素，table所在表格，row行號，column列號，value插入的元素
        public void insertcell(Microsoft.Office.Interop.Word .Table table, int row, int column, string value)
        {
            table.Cell(row, column).Range.Text = value;
        }
 
        //給表格中單元格插入元素，n表格的序號從1開始記，row行號，column列號，value插入的元素
        public void insertcell(int n, int row, int column, string value)
        {
            wordDoc.Content.Tables[n].Cell(row, column).Range.Text = value;
        }
 
        //給表格插入一行數據，n為表格的序號，row行號，columns列數，values插入的值
        public void insertcell(int n, int row, int columns, string[] values)
        {
            Microsoft.Office.Interop.Word .Table table = wordDoc.Content.Tables[n];
            for (int i = 0; i < columns; i++)
            {
                table.Cell(row, i + 1).Range.Text = values[i];
            }
        }
 
        //插入圖片
        public void InsertPictureByBookmarks(string bookmark, string picturepath, float width, float hight)
        {
            if (wordApp.ActiveDocument.Bookmarks.Exists(bookmark))
            {
                object miss = System.Reflection.Missing.Value;
                object ostart = bookmark;
                object linktofile = false;       //圖片是否為外部鏈接
                object savewithdocument = true;  //圖片是否隨文檔一起保存
                object range = wordDoc.Bookmarks.get_Item(ref ostart).Range;//圖片插入位置

                wordDoc.InlineShapes.AddPicture(picturepath, ref linktofile, ref savewithdocument, ref range);
                wordDoc.Application.ActiveDocument.InlineShapes[1].Width = width;   //設置圖片寬度
                wordDoc.Application.ActiveDocument.InlineShapes[1].Height = hight;  //設置圖片高度               
            }           
        }
 
        //插入一段文字,text為文字內容
        public void inserttext(string bookmark, string text)
        {
            object ostart = bookmark;
            object range = wordDoc.Bookmarks.get_Item(ref ostart).Range;
            Paragraph wp = wordDoc.Content.Paragraphs.Add(ref range);
            wp.Format.SpaceBefore = 6;
            wp.Range.Text = text;
            wp.Format.SpaceAfter = 24;
            wp.Range.InsertParagraphAfter();
            wordDoc.Paragraphs.Last.Range.Text = "\n";
        }
         
        public void killwinwordprocess()
        {
            System.Diagnostics.Process[] processes = System.Diagnostics.Process.GetProcessesByName("WINWORD");
            foreach (System.Diagnostics.Process process in processes)
            {
                if (process.MainWindowTitle.IndexOf("Microsoft Word") >= 0)
                {
                    process.Kill();
                }
            }
        }
    }
}
~~~


----------

參考:

- http://www.cnblogs.com/gc2013/p/3978514.html
- http://www.makaidong.com/%E5%8D%9A%E5%AE%A2%E5%9B%AD%E6%B1%87/10262.shtml
- http://www.cnblogs.com/hnsongbiao/p/4805347.html

