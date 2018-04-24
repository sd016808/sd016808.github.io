---
layout: post
title: 'C# 使用 SharpZipLib 壓縮解壓縮檔案'
author: 'James Peng'
tags: ['C# 3rd-Party Library']
---

SharpZipLib是一個GPL License的開放原始碼元件，完全以C#程式開發，提供Zip, GZip, Tar及BZip2等壓縮方式。

SharpZipLib的最新軟體可由下列網址下載:
[http://www.icsharpcode.net/opensource/sharpziplib/](http://www.icsharpcode.net/opensource/sharpziplib/)

解壓縮軟體後，將\SharpZipLib_0860_Bin\net-20\ICSharpCode.SharpZipLib.dll的DLL檔複製到自己專案下的bin目錄

加入參考: ICSharpCode.SharpZipLib.dll

- 
## 引用命名空間 ##

~~~csharp

using ICSharpCode.SharpZipLib;
using ICSharpCode.SharpZipLib.Zip;
using ICSharpCode.SharpZipLib.Checksums;

~~~


----------


## 壓縮 ##

~~~csharp
        #region 壓縮檔案及資料夾
        /// <summary>
        /// 遞迴壓縮資料夾方法
        /// </summary>
        /// <param name="FolderToZip"></param>
        /// <param name="ZOPStream">壓縮檔案輸出流物件</param>
        /// <param name="ParentFolderName"></param>
        private bool ZipFileDictory(string FolderToZip, ZipOutputStream ZOPStream, string ParentFolderName)
        {
            bool res = true;
            string[] folders, filenames;
            ZipEntry entry = null;
            FileStream fs = null;
            Crc32 crc = new Crc32();
            try
            {
                //建立目前資料夾
                entry = new ZipEntry(Path.Combine(ParentFolderName, Path.GetFileName(FolderToZip) + "/"));  //加上 「/」 才會當成是資料夾建立
                ZOPStream.PutNextEntry(entry);
                ZOPStream.Flush();
                //先壓縮檔案，再遞迴壓縮資料夾 
                filenames = Directory.GetFiles(FolderToZip);
                foreach (string file in filenames)
                {
                    //打開壓縮檔案
                    fs = File.OpenRead(file);
                    byte[] buffer = new byte[fs.Length];
                    fs.Read(buffer, 0, buffer.Length);
                    entry = new ZipEntry(Path.Combine(ParentFolderName, Path.GetFileName(FolderToZip) + "/" + Path.GetFileName(file)));
                    entry.DateTime = DateTime.Now;
                    entry.Size = fs.Length;
                    fs.Close();
                    crc.Reset();
                    crc.Update(buffer);
                    entry.Crc = crc.Value;
                    ZOPStream.PutNextEntry(entry);
                    ZOPStream.Write(buffer, 0, buffer.Length);
                }
            }
            catch
            {
                res = false;
            }
            finally
            {
                if (fs != null)
                {
                    fs.Close();
                    fs = null;
                }
                if (entry != null)
                {
                    entry = null;
                }
                GC.Collect();
                GC.Collect(1);
            }
            folders = Directory.GetDirectories(FolderToZip);
            foreach (string folder in folders)
            {
                if (!ZipFileDictory(folder, ZOPStream, Path.Combine(ParentFolderName, Path.GetFileName(FolderToZip))))
                {
                    return false;
                }
            }

            return res;
        }

        /// <summary>
        /// 壓縮目錄
        /// </summary>
        /// <param name="FolderToZip">待壓縮的資料夾</param>
        /// <param name="ZipedFile">壓縮後的檔案名</param>
        /// <returns></returns>
        private bool ZipFileDictory(string FolderToZip, string ZipedFile)
        {
            bool res;
            if (!Directory.Exists(FolderToZip))
            {
                return false;
            }
            ZipOutputStream ZOPStream = new ZipOutputStream(File.Create(ZipedFile));
            ZOPStream.SetLevel(6);
            res = ZipFileDictory(FolderToZip, ZOPStream, "");
            ZOPStream.Finish();
            ZOPStream.Close();
            return res;
        }

        /// <summary>
        /// 壓縮檔案和資料夾
        /// </summary>
        /// <param name="FileToZip">待壓縮的檔案或資料夾</param>
        /// <param name="ZipedFile">壓縮後產生的壓縮檔案名，全路徑格式</param>
        /// <returns></returns>
        public bool Zip(String FileToZip, String ZipedFile)
        {
            if (Directory.Exists(FileToZip))
            {
                return ZipFileDictory(FileToZip, ZipedFile);
            }
            else
            {
                return false;
            }
        }
        #endregion
~~~


----------

## 解壓縮 ##

~~~csharp
       #region 解壓檔案
        /// <summary>
        /// 解壓檔案
        /// </summary>
        /// <param name="FileToUpZip">待解壓的檔案</param>
        /// <param name="ZipedFolder">指定解壓目標目錄</param>
        public void UnZip(string FileToUpZip, string ZipedFolder)
        {
            if (!File.Exists(FileToUpZip))
            {
                return;
            }
            if (!Directory.Exists(ZipedFolder))
            {
                Directory.CreateDirectory(ZipedFolder);
            }
            ZipInputStream ZIPStream = null;
            ZipEntry theEntry = null;
            string fileName;
            FileStream streamWriter = null;
            try
            {
                //產生一個GZipInputStream流，用來打開壓縮檔案
                ZIPStream = new ZipInputStream(File.OpenRead(FileToUpZip));
                while ((theEntry = ZIPStream.GetNextEntry()) != null)
                {
                    if (theEntry.Name != String.Empty)
                    {
                        fileName = Path.Combine(ZipedFolder, theEntry.Name);
                        //判斷檔案路徑是否是資料夾
                        if (fileName.EndsWith("/") || fileName.EndsWith("\\"))
                        {
                            Directory.CreateDirectory(fileName);
                            continue;
                        }
                        //產生一個檔案流，它用來產生解壓檔案
                        streamWriter = File.Create(fileName);
                        int size = 2048;//指定壓縮塊的大小，一般為2048的倍數
                        byte[] data = new byte[2048];//指定緩衝區的大小
                        while (true)
                        {
                            size = ZIPStream.Read(data, 0, data.Length);//讀入一個壓縮塊
                            if (size > 0)
                            {
                                streamWriter.Write(data, 0, size);//寫入解壓檔案代表的檔案流
                            }
                            else
                            {
                                break;//若讀到壓縮檔案尾，則結束 
                            }
                        }
                    }
                }
            }
            finally
            {
                if (streamWriter != null)
                {
                    streamWriter.Close();
                    streamWriter = null;
                }
                if (theEntry != null)
                {
                    theEntry = null;
                }
                if (ZIPStream != null)
                {
                    ZIPStream.Close();
                    ZIPStream = null;
                }
                GC.Collect();
                GC.Collect(1);
            }
        }
        #endregion
~~~



----------


## 複製目錄 ##

~~~csharp
        #region 複製檔案//
        public void CopyFile(string[] list, string strNewPath, ToolStripProgressBar TSPBar)
        {
            try
            {
                TSPBar.Maximum = list.Length;
                string strNewFile = "c:\\" + strNewPath;
                if (!Directory.Exists(strNewFile))
                    Directory.CreateDirectory(strNewFile);
                foreach (object objFile in list)
                {
                    string strFile = objFile.ToString();
                    string Filename = strFile.Substring(strFile.LastIndexOf("\\") + 1, strFile.Length - strFile.LastIndexOf("\\") - 1);
                    File.Copy(strFile, strNewFile + "\\" + Filename, true);
                    TSPBar.Value += 1;
                }
            }
            catch (Exception ex)
            {
                MessageBox.Show(ex.Message);
            }
        }
        #endregion
~~~


----------

## 批次壓縮使用方法 ##

~~~csharp
    private void button2_Click(object sender, EventArgs e)//批次壓縮
        {
            try
            {
                if (txtfiles.Text.Trim() != "")
                {
                    toolStripProgressBar1.Maximum = files.Length;
                    if (files.Length > 1)
                    {
                        if (saveFileDialog1.ShowDialog() == DialogResult.OK)
                        {
                            string strNewPath = DateTime.Now.ToString("yyyyMMddhhmmss");
                            CopyFile(files, strNewPath, toolStripProgressBar1);
                            Zip("c:\\" + strNewPath, saveFileDialog1.FileName);
                            Directory.Delete("c:\\" + strNewPath, true);
                            MessageBox.Show("壓縮檔案成功");
                        }
                    }
                    toolStripProgressBar1.Value = 0;
                }
                else
                {
                    MessageBox.Show("警告：請選擇要進行批次壓縮的檔案！", "警告", MessageBoxButtons.OK, MessageBoxIcon.Error);
                }
            }
            catch { }
        }
~~~


----------

## 批次解壓縮使用方法 ##

~~~csharp
        private void button4_Click(object sender, EventArgs e)
        {
            try
            {
                if (txtfiles2.Text.Trim() != "")
                {
                    toolStripProgressBar1.Maximum = files2.Length;
                    for (int i = 0; i < files2.Length; i++)
                    {
                        toolStripProgressBar1.Value = i;
                        string path = files2[i].ToString();
                        string newpath = path.Remove(path.LastIndexOf("\\") + 1);
                        UnZip(path, newpath);
                    }
                    toolStripProgressBar1.Value = 0;
                    MessageBox.Show("解壓縮成功！");
                }
                else
                {
                    MessageBox.Show("警告：請選擇要進行批次解壓縮的檔案！", "警告", MessageBoxButtons.OK, MessageBoxIcon.Error);
                }
            }
            catch { }

        }
~~~

----------

## 參考： ##

- http://einboch.pixnet.net/blog/post/267876083-c%23%E4%BD%BF%E7%94%A8%E4%BE%86%E5%AE%8C%E6%88%90%E6%AA%94%E6%A1%88%E7%9A%84%E5%A3%93%E7%B8%AE-%E8%A7%A3%E5%A3%93%E7%B8%AE%E5%8A%9F%E8%83%BD
- http://www.icsharpcode.net/opensource/sharpziplib/
