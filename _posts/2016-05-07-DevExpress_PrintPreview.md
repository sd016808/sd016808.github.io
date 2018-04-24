---
layout: post
title: 'C# DevExpress 實作 Print Preview 功能'
author: 'James Peng'
tags: ['DevExpress']
---

##  C# DevExpress 實作 Print Preview 功能  ##

先看圖

![](..\images\2016-05-07-CSharp_PrintPreview\Nyfft6u.png)


![](..\images\2016-05-07-CSharp_PrintPreview\00oL7Y4.png)

![](..\images\2016-05-07-CSharp_PrintPreview\EBHtt6C.png)


----------


## 給定指定pdf ##


~~~csharp
        public PrintPreviewForm()
        {
            InitializeComponent();             
            pdfViewer1.LoadDocument(ProjectInfo.ProjectPath + @"\Report.pdf");
        }
~~~


![](..\images\2016-05-07-CSharp_PrintPreview\fklimJl.png)

----------

## 關閉 Document ##

當點選 Row 時 ，會選取 Checkbox

~~~csharp
        private void PrintPreviewForm_FormClosing(object sender, System.Windows.Forms.FormClosingEventArgs e)
        {
            pdfViewer1.CloseDocument();
        }
~~~



----------

參考：

- https://documentation.devexpress.com/#WindowsForms/CustomDocument3227
