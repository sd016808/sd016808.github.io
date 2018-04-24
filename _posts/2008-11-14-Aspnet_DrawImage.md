---
layout: post
title: 'ASP.NET 給圖片加上浮水印效果'
author: 'James Peng'
tags: ['ASP.NET']
---


~~~csharp
    private void Btn_Upload_Click(object sender, System.EventArgs e) 
        { 
            if(UploadFile.PostedFile.FileName.Trim()!="") 
            { 
                //上傳文件 
                string extension = Path.GetExtension(UploadFile.PostedFile.FileName).ToUpper(); 
                string fileName = DateTime.Now.Year.ToString() + DateTime.Now.Month.ToString() + DateTime.Now.Day.ToString() + DateTime.Now.Hour.ToString() + DateTime.Now.Minute.ToString() + DateTime.Now.Second.ToString(); 
                string path = Server.MapPath(".") + "/UploadFile/" + fileName + extension; 
                UploadFile.PostedFile.SaveAs(path); 
                //加文字水印，注意，這裡的代碼和以下加圖片水印的代碼不能共存 
                System.Drawing.Image image = System.Drawing.Image.FromFile(path); 
                Graphics g = Graphics.FromImage(image); 
                g.DrawImage(image, 0, 0, image.Width, image.Height); 
                Font f = new Font("Verdana", 32); 
                Brush b = new SolidBrush(Color.White); 
                string addText = AddText.Value.Trim(); 
                g.DrawString(addText, f, b, 10, 10); 
                g.Dispose(); 
                //加圖片水印 
                System.Drawing.Image image = System.Drawing.Image.FromFile(path); 
                System.Drawing.Image copyImage = System.Drawing.Image.FromFile( Server.MapPath(".") + "/Alex.gif"); 
                Graphics g = Graphics.FromImage(image); 
                g.DrawImage(copyImage, new Rectangle(image.Width-copyImage.Width, image.Height-copyImage.Height, copyImage.Width, copyImage.Height), 0, 0, copyImage.Width, copyImage.Height, GraphicsUnit.Pixel); 
                g.Dispose(); 
                //保存加水印過後的圖片,刪除原始圖片 
                string newPath = Server.MapPath(".") + "/UploadFile/" + fileName + "_new" + extension; 
                image.Save(newPath); 
                image.Dispose(); 
                if(File.Exists(path)) 
                { 
                    File.Delete(path); 
                } 
                Response.Redirect(newPath); 
            } 
        }
~~~


----------


來源：


- http://www.cnblogs.com/index/archive/2004/10/20/54498.html
