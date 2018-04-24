---
layout: post
title: 'Visual Studio 錯誤訊息 No EditorOptionDefinition Export Found Error'
author: 'James Peng'
tags: ['Visual Studio']
---

![](..\images\2016-06-13-VisualStudio_No_EditorOptionDefinition_Export_Found_Error\nyLYTkv.png)

![](..\images\2016-06-13-VisualStudio_No_EditorOptionDefinition_Export_Found_Error\OdchQug.png)

![](..\images\2016-06-13-VisualStudio_No_EditorOptionDefinition_Export_Found_Error\QsLYv2c.png)

> No EditorOptionDefinition Export Found for the given option name: Adomments/HighlightCurrentLine/Enable 參數名稱: optionId


----------


Follow the steps:

1. Close Visual Studio
2. Open the folder: %LocalAppData%\Microsoft\VisualStudio\12.0\
3. Delete the ComponentModelCache folder
4. Restart Visual Studio.

----------

參考:

- http://stackoverflow.com/questions/23893497/no-editoroptiondefinition-export-found-error
