---
layout: post
title: 'WinForm C# 使用 WPF 控件於 Winform (AvalonEdit)'
author: 'James Peng'
tags: ['WinForm']
---

If you want to be able to set the hosted content at design time the control needs to be part of your solution. One way to achieve that is to create a custom WPF user control which contains the AvalonEdit component you want to use. I.e

Create a WPF User Control library project and create a user control containing the AvalonEdit component.
Add the User control project to your Winforms solution.
Now you should be able to select your new user control as the hosted content.

Or you could add the AvalonEdit control directly in code like this:

----------

first install the AvalonEdit Control library

    Install-Package AvalonEdit -Version 5.0.4


----------


Form1()
~~~csharp

public Form1()
{
  InitializeComponent();

  ElementHost host= new ElementHost();
  host.Size = new Size(200, 100);
  host.Location = new Point(100,100);

  var edit = GetTextEditor();
  host.Child = edit;

  this.Controls.Add(host);
}
~~~


----------



GetTextEditor()
~~~csharp
private ICSharpCode.AvalonEdit.TextEditor GetTextEditor()
{
 			ICSharpCode.AvalonEdit.TextEditor textEditor = new ICSharpCode.AvalonEdit.TextEditor();
            string CustomHighlightingFile = "CustomHighlighting.xshd";
            if (File.Exists(CustomHighlightingFile))
            {
                Stream xshd_stream = File.OpenRead(CustomHighlightingFile);
                XmlTextReader xshd_reader = new XmlTextReader(xshd_stream);
                // Apply the new syntax highlighting definition.
                textEditor.SyntaxHighlighting = ICSharpCode.AvalonEdit.Highlighting.Xshd.HighlightingLoader.Load(xshd_reader, ICSharpCode.AvalonEdit.Highlighting.HighlightingManager.Instance);
                xshd_reader.Close();
                xshd_stream.Close();
            }
            //Host the WPF AvalonEdiot control in a Winform ElementHost control
            ElementHost host = new ElementHost();
            host.Dock = DockStyle.Fill;
            host.Child = textEditor;

            //Background
            var converter = new System.Windows.Media.BrushConverter();
            textEditor.Background = (System.Windows.Media.Brush)converter.ConvertFromString("#1D1D1D");
            textEditor.Foreground = (System.Windows.Media.Brush)converter.ConvertFromString("#FFFFFF");
            textEditor.FontSize = 16;
            textEditor.FontFamily = new System.Windows.Media.FontFamily("Consolas");
            textEditor.ShowLineNumbers = true;
            textEditor.HorizontalScrollBarVisibility = System.Windows.Controls.ScrollBarVisibility.Visible;
            textEditor.VerticalScrollBarVisibility = System.Windows.Controls.ScrollBarVisibility.Visible;
			return textEditor;
}
~~~



----------


CustomHighlighting.xshd
~~~xml
<?xml version="1.0"?>
<SyntaxDefinition name="Custom Highlighting" xmlns="http://icsharpcode.net/sharpdevelop/syntaxdefinition/2008">
	<Color name="Comment" foreground="Green" />
	<Color name="String" foreground="Blue" />
	
	<!-- This is the main ruleset. -->
	<RuleSet>
		<Span color="Comment" begin="//" />
		<Span color="Comment" multiline="true" begin="/\*" end="\*/" />
		
		<Span color="String">
			<Begin>"</Begin>
			<End>"</End>
			<RuleSet>
				<!-- nested span for escape sequences -->
				<Span begin="\\" end="." />
			</RuleSet>
		</Span>
		
		<Keywords fontWeight="bold" foreground="Blue">
			<Word>if</Word>
			<Word>else</Word>
			<!-- ... -->
		</Keywords>
		
		<Keywords fontWeight="bold" fontStyle="italic" foreground="Red">
			<Word>AvalonEdit</Word>
		</Keywords>
		
		<!-- Digits -->
		<Rule foreground="DarkBlue">
            \b0[xX][0-9a-fA-F]+  # hex number
        |    \b
            (    \d+(\.[0-9]+)?   #number with optional floating point
            |    \.[0-9]+         #or just starting with floating point
            )
            ([eE][+-]?[0-9]+)? # optional exponent
        </Rule>
	</RuleSet>
</SyntaxDefinition>
~~~



## 參考： ##

- https://stackoverflow.com/questions/14170165/how-can-i-add-this-wpf-control-into-my-winform
- http://avalonedit.net
