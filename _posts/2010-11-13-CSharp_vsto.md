---
layout: post
title: 'C# VSTO 操作 Word 文件 '
author: 'James Peng'
tags: ['C# 3rd-Party Library']
---


## C# VSTO 操作 Word 文件  ##

~~~csharp

     //合併單元格 
   table.Cell(2, 2).Merge(table.Cell(2, 3));
//單元格分離 
    object Rownum = 2; 
    object Columnnum = 2; 
    table.Cell(2, 2).Split(ref Rownum, ref Columnnum);
//單元格對齊方式 
     WApp.Selection.Cells.VerticalAlignment =Microsoft.Office.Interop.Word.WdCellVerticalAlignment.wdCellAlignVerticalCenter;
//插入表行 
     table.Rows.Add(ref missing);
//分頁 object ib = Microsoft.Office.Interop.Word.WdBreakType.wdPageBreak; 
    WApp.Selection.InsertBreak(ref ib);
//換行 
     WApp.Selection.TypeParagraph();
~~~


----------


## word文件設置 ##
~~~csharp

WApp.ActiveDocument.PageSetup.LineNumbering.Active =0;//行編號 
            WApp.ActiveDocument.PageSetup.Orientation =Microsoft.Office.Interop.Word.WdOrientation.wdOrientPortrait;//頁面方向 
            WApp.ActiveDocument.PageSetup.TopMargin =WApp.CentimetersToPoints(float.Parse("2.54"));//上頁邊距 
            WApp.ActiveDocument.PageSetup.BottomMargin = WApp.CentimetersToPoints(float.Parse("2.54"));//下頁邊距 
            WApp.ActiveDocument.PageSetup.LeftMargin = WApp.CentimetersToPoints(float.Parse("3.17"));//左頁邊距 
            WApp.ActiveDocument.PageSetup.RightMargin = WApp.CentimetersToPoints(float.Parse("3.17"));//右頁邊距 
            WApp.ActiveDocument.PageSetup.Gutter = WApp.CentimetersToPoints(float.Parse("0"));//裝訂線位置 
            WApp.ActiveDocument.PageSetup.HeaderDistance = WApp.CentimetersToPoints(float.Parse("1.5"));//頁眉 
            WApp.ActiveDocument.PageSetup.FooterDistance = WApp.CentimetersToPoints(float.Parse("1.75"));//頁腳 
            WApp.ActiveDocument.PageSetup.PageWidth = WApp.CentimetersToPoints(float.Parse("21"));//紙張寬度 
            WApp.ActiveDocument.PageSetup.PageHeight = WApp.CentimetersToPoints(float.Parse("29.7"));//紙張高度 
            WApp.ActiveDocument.PageSetup.FirstPageTray = Microsoft.Office.Interop.Word.WdPaperTray.wdPrinterDefaultBin;//紙張來源 
            WApp.ActiveDocument.PageSetup.OtherPagesTray = Microsoft.Office.Interop.Word.WdPaperTray.wdPrinterDefaultBin;//紙張來源 
            WApp.ActiveDocument.PageSetup.SectionStart = Microsoft.Office.Interop.Word.WdSectionStart.wdSectionNewPage;//節的起始位置：新建頁 
            WApp.ActiveDocument.PageSetup.OddAndEvenPagesHeaderFooter = 0;//頁眉頁腳-奇偶頁不同 
            WApp.ActiveDocument.PageSetup.DifferentFirstPageHeaderFooter = 0;//頁眉頁腳-首頁不同 
            WApp.ActiveDocument.PageSetup.VerticalAlignment = Microsoft.Office.Interop.Word.WdVerticalAlignment.wdAlignVerticalTop;//頁面垂直對齊方式 
            WApp.ActiveDocument.PageSetup.SuppressEndnotes =0;//不隱藏尾注 
            WApp.ActiveDocument.PageSetup.MirrorMargins = 0;//不設置首頁的內外邊距 
            WApp.ActiveDocument.PageSetup.TwoPagesOnOne = false;//不雙面打印 
            WApp.ActiveDocument.PageSetup.BookFoldPrinting =false;//不設置手動雙面正面打印 
            WApp.ActiveDocument.PageSetup.BookFoldRevPrinting =false;//不設置手動雙面背面打印    
            WApp.ActiveDocument.PageSetup.BookFoldPrintingSheets = 1;//打印預設份數 
            WApp.ActiveDocument.PageSetup.GutterPos = Microsoft.Office.Interop.Word.WdGutterStyle.wdGutterPosLeft;//裝訂線位於左側 
            WApp.ActiveDocument.PageSetup.LinesPage = 40;//預設頁行數量 
            WApp.ActiveDocument.PageSetup.LayoutMode = Microsoft.Office.Interop.Word.WdLayoutMode.wdLayoutModeLineGrid;//版式模式為「只指定行網格」
~~~


----------


## 光標移動 ##
~~~csharp

//移動光標 
//光標下移3行 上移3行 
            object unit = Microsoft.Office.Interop.Word.WdUnits.wdLine; 
            object count = 3; 
            WApp.Selection.MoveEnd(ref unit,ref count); 
            WApp.Selection.MoveUp(ref unit, ref count, ref missing); 
//Microsoft.Office.Interop.Word.WdUnits說明 
            //wdCell                  A cell. 
            //wdCharacter             A character. 
            //wdCharacterFormatting   Character formatting. 
            //wdColumn                A column. 
            //wdItem                  The selected item. 
            //wdLine                  A line. //行 
            //wdParagraph             A paragraph. 
            //wdParagraphFormatting   Paragraph formatting. 
            //wdRow                   A row. 
            //wdScreen                The screen dimensions. 
            //wdSection               A section. 
            //wdSentence              A sentence. 
            //wdStory                 A story. 
            //wdTable                 A table. 
            //wdWindow                A window. 
            //wdWord                  A word.
//錄製的vb宏 
            //     ,移動光標至當前行首 
            //    Selection.HomeKey unit:=wdLine 
            //    '移動光標至當前行尾 
            //    Selection.EndKey unit:=wdLine 
            //    '選擇從光標至當前行首的內容 
            //    Selection.HomeKey unit:=wdLine, Extend:=wdExtend 
            //    '選擇從光標至當前行尾的內容 
            //    Selection.EndKey unit:=wdLine, Extend:=wdExtend 
            //    '選擇當前行 
            //    Selection.HomeKey unit:=wdLine 
            //    Selection.EndKey unit:=wdLine, Extend:=wdExtend 
            //    '移動光標至文件開始 
            //    Selection.HomeKey unit:=wdStory 
            //    '移動光標至文件結尾 
            //    Selection.EndKey unit:=wdStory 
            //    '選擇從光標至文件開始的內容 
            //    Selection.HomeKey unit:=wdStory, Extend:=wdExtend 
            //    '選擇從光標至文件結尾的內容 
            //    Selection.EndKey unit:=wdStory, Extend:=wdExtend 
            //    '選擇文件全部內容（從WholeStory可猜出Story應是當前文件的意思） 
            //    Selection.WholeStory 
            //    '移動光標至當前段落的開始 
            //    Selection.MoveUp unit:=wdParagraph 
            //    '移動光標至當前段落的結尾 
            //    Selection.MoveDown unit:=wdParagraph 
            //    '選擇從光標至當前段落開始的內容 
            //    Selection.MoveUp unit:=wdParagraph, Extend:=wdExtend 
            //    '選擇從光標至當前段落結尾的內容 
            //    Selection.MoveDown unit:=wdParagraph, Extend:=wdExtend 
            //    '選擇光標所在段落的內容 
            //    Selection.MoveUp unit:=wdParagraph 
            //    Selection.MoveDown unit:=wdParagraph, Extend:=wdExtend 
            //    '顯示選擇區的開始與結束的位置，注意：文件第1個字符的位置是0 
            //    MsgBox ("第" &amp; Selection.Start &amp; "個字符至第" &amp; Selection.End &amp; "個字符") 
            //    '刪除當前行 
            //    Selection.HomeKey unit:=wdLine 
            //    Selection.EndKey unit:=wdLine, Extend:=wdExtend 
            //    Selection.Delete 
            //    '刪除當前段落 
            //    Selection.MoveUp unit:=wdParagraph 
            //    Selection.MoveDown unit:=wdParagraph, Extend:=wdExtend 
            //    Selection.Delete
//表格的光標移動 
//光標到當前光標所在表格的地單元格 
WApp.Selection.Tables[1].Cell(1, 1).Select(); 
//unit對像定義 
object unith = Microsoft.Office.Interop.Word.WdUnits.wdRow;//表格行方式 
            object extend = Microsoft.Office.Interop.Word.WdMovementType.wdExtend;/**////extend對光標移動區域進行擴展選擇 
            object unitu = Microsoft.Office.Interop.Word.WdUnits.wdLine;//文件行方式,可以看成表格一行.不過和wdRow有區別 
            object unitp = Microsoft.Office.Interop.Word.WdUnits.wdParagraph;//段落方式,對於表格可以選擇到表格行後的換車符,對於跨行合併的行選擇,我能找到的最簡單方式 
            object count=1;//光標移動量 
~~~

----------


## 對於存在合併單元格的選擇操作 ##

下面代碼演示對於存在合併單元格的選擇操作．合併單元格的選擇問題一直是word的bug.部分object對像參照上面代碼
上面這個是表格合併樣式.要如何才能選擇2行標題欄尼.看下面代碼

~~~csharp


//定位到表格第1單元格 
WApp.Selection.Tables[1].Cell(1, 1).Select(); 
//定位到第1個單元格第1個字符前          
WApp.Selection.HomeKey(ref unith, ref missing); 
//擴展到行尾,選擇表第1行            
WApp.Selection.EndKey(ref unith, ref extend); 
//定義表格標題的行數量,titlerow為參數            
object strtitlerow=titlerow-1; 
//移動光標選擇第1行的末尾段落標記            
WApp.Selection.MoveDown(ref unitp, ref count, ref extend); 
//選擇下一行,因為合併的原因,如表格標題最後列是合併,只選擇了2行的部分 
            WApp.Selection.MoveDown(ref unitu, ref strtitlerow, ref extend); 
//擴展到該行的末端,保證合併行能全部選擇到 
            WApp.Selection.EndKey(ref unith, ref extend); 
//複製選擇內容到剪貼板 
            WApp.Selection.Copy(); 
//下面是移動光標到任何位置並粘貼內容.我程序中目的是到表格換頁的時候自動插入下一頁的表頭. 
            WApp.Selection.Tables[1].Cell(System.Convert.ToInt32(strRownum), 1).Select(); 
            WApp.Selection.HomeKey(ref unith, ref missing); 
            WApp.Selection.Paste(); 

~~~


----------


## 段落格式設定  ##
~~~csharp

//段落格式設定 
            WApp.Selection.ParagraphFormat.LeftIndent = WApp.CentimetersToPoints(float.Parse("0"));//左縮進 
            WApp.Selection.ParagraphFormat.RightIndent = WApp.CentimetersToPoints(float.Parse("0"));//右縮進 
            WApp.Selection.ParagraphFormat.SpaceBefore =float.Parse("0");//段前間距 
            WApp.Selection.ParagraphFormat.SpaceBeforeAuto =0;// 
            WApp.Selection.ParagraphFormat.SpaceAfter = float.Parse("0");//段後間距 
            WApp.Selection.ParagraphFormat.SpaceAfterAuto = 0;// 
            WApp.Selection.ParagraphFormat.LineSpacingRule = Microsoft.Office.Interop.Word.WdLineSpacing.wdLineSpaceSingle;//單倍行距 
            WApp.Selection.ParagraphFormat.Alignment = Microsoft.Office.Interop.Word.WdParagraphAlignment.wdAlignParagraphJustify;// 段落2端對齊 
            WApp.Selection.ParagraphFormat.WidowControl = 0;//孤行控制 
            WApp.Selection.ParagraphFormat.KeepWithNext = 0;//與下段同頁 
            WApp.Selection.ParagraphFormat.KeepTogether = 0;//段中不分頁 
            WApp.Selection.ParagraphFormat.PageBreakBefore = 0;//段前分頁 
            WApp.Selection.ParagraphFormat.NoLineNumber = 0;//取消行號 
            WApp.Selection.ParagraphFormat.Hyphenation = 1;//取消段字 
            WApp.Selection.ParagraphFormat.FirstLineIndent = WApp.CentimetersToPoints(float.Parse("0"));//首行縮進 
            WApp.Selection.ParagraphFormat.OutlineLevel = Microsoft.Office.Interop.Word.WdOutlineLevel.wdOutlineLevelBodyText; 
            WApp.Selection.ParagraphFormat.CharacterUnitLeftIndent = float.Parse("0"); 
            WApp.Selection.ParagraphFormat.CharacterUnitRightIndent = float.Parse("0"); 
            WApp.Selection.ParagraphFormat.CharacterUnitFirstLineIndent = float.Parse("0"); 
            WApp.Selection.ParagraphFormat.LineUnitBefore = float.Parse("0"); 
            WApp.Selection.ParagraphFormat.LineUnitAfter = float.Parse("0"); 
            WApp.Selection.ParagraphFormat.AutoAdjustRightIndent = 1; 
            WApp.Selection.ParagraphFormat.DisableLineHeightGrid =0; 
            WApp.Selection.ParagraphFormat.FarEastLineBreakControl =1; 
            WApp.Selection.ParagraphFormat.WordWrap = 1; 
            WApp.Selection.ParagraphFormat.HangingPunctuation = 1; 
            WApp.Selection.ParagraphFormat.HalfWidthPunctuationOnTopOfLine = 0; 
            WApp.Selection.ParagraphFormat.AddSpaceBetweenFarEastAndAlpha = 1; 
            WApp.Selection.ParagraphFormat.AddSpaceBetweenFarEastAndDigit = 1; 
            WApp.Selection.ParagraphFormat.BaseLineAlignment = Microsoft.Office.Interop.Word.WdBaselineAlignment.wdBaselineAlignAuto;

~~~


----------


## 字體格式設定  ##
~~~csharp

//字體格式設定 
            WApp.Selection.Font.NameFarEast = "華文中宋"; 
            WApp.Selection.Font.NameAscii = "Times New Roman"; 
            WApp.Selection.Font.NameOther = "Times New Roman"; 
            WApp.Selection.Font.Name = "細明體"; 
            WApp.Selection.Font.Size = float.Parse("14"); 
            WApp.Selection.Font.Bold = 0; 
            WApp.Selection.Font.Italic = 0; 
            WApp.Selection.Font.Underline = Microsoft.Office.Interop.Word.WdUnderline.wdUnderlineNone; 
            WApp.Selection.Font.UnderlineColor = Microsoft.Office.Interop.Word.WdColor.wdColorAutomatic; 
            WApp.Selection.Font.StrikeThrough =0;//刪除線 
            WApp.Selection.Font.DoubleStrikeThrough = 0;//雙刪除線 
            WApp.Selection.Font.Outline =0;//空心 
            WApp.Selection.Font.Emboss = 0;//陽文 
            WApp.Selection.Font.Shadow = 0;//陰影 
            WApp.Selection.Font.Hidden = 0;//隱藏文字 
            WApp.Selection.Font.SmallCaps = 0;//小型大寫字母 
            WApp.Selection.Font.AllCaps = 0;//全部大寫字母 
            WApp.Selection.Font.Color = Microsoft.Office.Interop.Word.WdColor.wdColorAutomatic; 
            WApp.Selection.Font.Engrave = 0;//陰文 
            WApp.Selection.Font.Superscript = 0;//上標 
            WApp.Selection.Font.Subscript = 0;//下標 
            WApp.Selection.Font.Spacing = float.Parse("0");//字符間距 
            WApp.Selection.Font.Scaling = 100;//字符縮放 
            WApp.Selection.Font.Position = 0;//位置 
            WApp.Selection.Font.Kerning = float.Parse("1");//字體間距調整 
            WApp.Selection.Font.Animation = Microsoft.Office.Interop.Word.WdAnimation.wdAnimationNone;//文字效果 
            WApp.Selection.Font.DisableCharacterSpaceGrid =false; 
            WApp.Selection.Font.EmphasisMark = Microsoft.Office.Interop.Word.WdEmphasisMark.wdEmphasisMarkNone;

~~~


----------


## 獲取光標位置  ##

那裡找到的忘了，感謝提供的老大。放到這裡供大家參考。 
有了這個和上面內容，相信大家對word文件的控制應該到了隨心所欲的地步，爽啊 
獲取的c#語法 

~~~csharp
//get_Information 
Selection.get_Information(WdInformation.wdActiveEndPageNumber) 
//關於行號-頁號-列號-位置 
            //information 屬性 
            //返回有關指定的所選內容或區域的訊息。variant 類型，只讀。 
            //expression.information(type) 
            //expression 必需。該表達式返回一個 range 或 selection 對象。 
            //type long 類型，必需。需要返回的訊息。可取下列 wdinformation 常量之一： 
            //wdactiveendadjustedpagenumber 返回頁碼，在該頁中包含指定的所選內容或區域的活動結尾。如果設置了一個起始頁碼，並對頁碼進行了手工調整，則返回調整過的頁碼。 
            //wdactiveendpagenumber 返回頁碼，在該頁中包含指定的所選內容或區域的活動結尾，頁碼從文件的開頭開始計算而不考慮對頁碼的任何手工調整。 
            //wdactiveendsectionnumber 返回節號，在該節中包含了指定的所選內容或區域的活動結尾。 
            //wdatendofrowmarker 如果指定的所選內容或區域位於表格的行結尾標記處，則本參數返回 true。 
            //wdcapslock 如果大寫字母鎖定模式有效，則本參數返回 true。 
            //wdendofrangecolumnnumber 返回表格列號，在該表格列中包含了指定的所選內容或區域的活動結尾。 
            //wdendofrangerownumber 返回表格行號，在該表格行包含了指定的所選內容或區域的活動結尾。 
            //wdfirstcharactercolumnnumber 返回指定的所選內容或區域中第一個字符的位置。如果所選內容或區域是折疊的，則返回所選內容或區域右側緊接著的字符編號。 
            //wdfirstcharacterlinenumber 返回所選內容中第一個字符的行號。如果 pagination 屬性為 false，或 draft 屬性為 true，則返回 - 1。 
            //wdframeisselected 如果所選內容或區域是一個完整的圖文框文本框，則本參數返回 true。 
            //wdheaderfootertype 返回一個值，該值表明包含了指定的所選內容或區域的頁眉或頁腳的類型，如下表所示。 值 頁眉或頁腳的類型 
            //- 1 無 
            //0 偶數頁頁眉 
            //1 奇數頁頁眉 
            //2 偶數頁頁腳 
            //3 奇數頁頁腳 
            //4 第一個頁眉 
            //5 第一個頁腳 
            //wdhorizontalpositionrelativetopage 返回指定的所選內容或區域的水平位置。該位置是所選內容或區域的左邊與頁面的左邊之間的距離，以磅為單位。如果所選內容或區域不可見，則返回 - 1。 
            //wdhorizontalpositionrelativetotextboundary 返回指定的所選內容或區域相對於周圍最近的正文邊界的左邊的水平位置，以磅為單位。如果所選內容或區域沒有顯示在當前屏幕，則本參數返回 - 1。 
            //wdinclipboard 有關此常量的詳細內容，請參閱 microsoft office 98 macintosh 版的語言參考幫助。 
            //wdincommentpane 如果指定的所選內容或區域位於批注窗格，則返回 true。 
            //wdinendnote 如果指定的所選內容或區域位於頁面視圖的尾注區內，或者位於普通視圖的尾注窗格中，則本參數返回 true。 
            //wdinfootnote 如果指定的所選內容或區域位於頁面視圖的腳注區內，或者位於普通視圖的腳注窗格中，則本參數返回 true。 
            //wdinfootnoteendnotepane 如果指定的所選內容或區域位於頁面視圖的腳注或尾注區內，或者位於普通視圖的腳注或尾注窗格中，則本參數返回 true。詳細內容，請參閱前面的 wdinfootnote 和 wdinendnote 的說明。 
            //wdinheaderfooter 如果指定的所選內容或區域位於頁眉或頁腳窗格中，或者位於頁面視圖的頁眉或頁腳中，則本參數返回 true。 
            //wdinmasterdocument 如果指定的所選內容或區域位於主控文件中，則本參數返回 true。 
            //wdinwordmail 返回一個值，該值表明了所選內容或區域的的位置，如下表所示。值 位置 
            //0 所選內容或區域不在一條電子郵件消息中。 
            //1 所選內容或區域位於正在發送的電子郵件中。 
            //2 所選內容或區域位於正在閱讀的電子郵件中。 
            //wdmaximumnumberofcolumns 返回所選內容或區域中任何行的最大表格列數。 
            //wdmaximumnumberofrows 返回指定的所選內容或區域中表格的最大行數。 
            //wdnumberofpagesindocument 返回與所選內容或區域相關聯的文件的頁數。 
            //wdnumlock 如果 num lock 有效，則本參數返回 true。 
            //wdovertype 如果改寫模式有效，則本參數返回 true。可用 overtype 屬性改變改寫模式的狀態。 
            //wdreferenceoftype 返回一個值，該值表明所選內容相對於腳注、尾注或批注引用的位置，如下表所示。 值 描述 
            //— 1 所選內容或區域包含、但不只限定於腳注、尾注或批注引用中。 
            //0 所選內容或區域不在腳注、尾注或批注引用之前。 
            //1 所選內容或區域位於腳注引用之前。 
            //2 所選內容或區域位於尾注引用之前。 
            //3 所選內容或區域位於批注引用之前。 
            //wdrevisionmarking 如果修訂功能處於活動狀態，則本參數返回 true。 
            //wdselectionmode 返回一個值，該值表明當前的選定模式，如下表所示。 值 選定模式 
            //0 常規選定 
            //1 擴展選定 
            //2 列選定 
            //wdstartofrangecolumnnumber 返回所選內容或區域的起點所在的表格的列號。 
            //wdstartofrangerownumber 返回所選內容或區域的起點所在的表格的行號。 
            //wdverticalpositionrelativetopage 返回所選內容或區域的垂直位置，即所選內容的上邊與頁面的上邊之間的距離，以磅為單位。如果所選內容或區域沒有顯示在屏幕上，則本參數返回 - 1。 
            //wdverticalpositionrelativetotextboundary 返回所選內容或區域相對於周圍最近的正文邊界的上邊的垂直位置，以磅為單位。如果所選內容或區域沒有顯示在屏幕上，則本參數返回 - 1。 
            //wdwithintable 如果所選內容位於一個表格中，則本參數返回 true。 
            //wdzoompercentage 返回由 percentage 屬性設置的當前的放大百分比。

~~~


----------

參考：

- http://hi.baidu.com/nmgdahai/blog/item/24cc4c00df463d1d738b6561.html
