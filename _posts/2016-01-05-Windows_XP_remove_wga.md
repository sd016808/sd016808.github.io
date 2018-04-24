---
layout: post
title: 'WindowsXP remove wga'
author: 'James Peng'
tags: ['English']
---


removeWGA.bat

~~~text
@echo off
echo 解除WGA訊息和黑屏......
c:
cd \windows\system32
ren WgaLogon.exe WgaLogon.old
ren WgaLogon.dll WgaLogon.bak
ren WgaTray.exe WgaTray.old
regsvr32 /u /s LegitCheckControl.dll

del wgalogon*.*
del wgatray*.*
del LegitCheckControl.dll

echo 恭喜~已解除完成，請重開機！！
echo. & pause

~~~