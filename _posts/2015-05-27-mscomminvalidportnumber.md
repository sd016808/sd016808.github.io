---
layout: post
title: 'MSCOMM Invalid port number'
author: 'James Peng'
tags: ['Visual C++']
---

High Port numbers with MSComm  
  
 MSCOMM Handling port number more than 16.  
  
 MSCOMM支持大於16個限制。  
  
 I noticed a few posts about the limitation to port 1-16 of MSComm. As  
 I completed my generic ASCOM DSLR driver, I wanted to fix this  
 limitation.  
  
 There is a hack that can be done to the mscomm file to increase the  
 port number to 1-256:  
  
 Get an HEX editor  
 open C:\\windows\\system32\\mscomm32.ocx (make a backup copy too)  
 Find the string "66 3D 10 00" (there is only one)  
 Change the string to "66 3D FF 00"  
 Save the file.  
  
 I just tested it with my driver and my \$3 USB to rs232. It would  
 normally go on port 14 but I changed to port 255 and it works great.  
  
  
 [download it](https://www.dropbox.com/s/yw96mxur14rc193/MSCOMM32.7z?dl=0)
   
  

