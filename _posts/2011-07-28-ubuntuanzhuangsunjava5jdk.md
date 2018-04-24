---
layout: post
title: 'ubuntu安裝sun-java5-jdk'
author: 'James Peng'
tags: ['Java']
---

Ubuntu
9.10以上版本中，Ubuntu就已經去除了sun-java5-jdk的支援，但是為了進行Android
的開發，又必須安裝sun-java5-jdk。補充：ubuntu 10.10以上預設java6-jdk

> root@james-VirtualBox:/home/james/mydroid\# java –version

[![image](http://lh5.ggpht.com/-i9a1LhtrBpA/TjEQJ2XUNxI/AAAAAAAAJqc/OPIru501xkk/image_thumb%25255B5%25255D.png?imgmax=800 "image")](http://lh6.ggpht.com/-moo_XBXwasM/TjEQJYuTB5I/AAAAAAAAJqY/Burcwzh6Ki0/s1600-h/image%25255B9%25255D.png)  
java version "1.6.0\_22"  
OpenJDK Runtime Environment (IcedTea6 1.10.2)
(6b22-1.10.2-0ubuntu1\~11.04.1)  
OpenJDK Client VM (build 20.0-b11, mixed mode, sharing)  
root@james-VirtualBox:/home/james/mydroid\#  

 

但android 2.2只能用java5-jdk，故要安裝。

那麼Ubuntu 10.10及以後，如何來進行sun-java5-jdk的安裝呢？

在ubuntu 10.10下，直接：

* * * * *

> root@james-VirtualBox:/home/james/mydroid\# sudo apt-get install
> sun-java5-jdk

正在讀取套件清單... 完成  
正在重建相依關係           
正在讀取狀態資料... 完成  
E: 找不到套件 sun-java5-jdk

* * * * *

**解決方法之一：**是修改apt來源。

**解決方法之二：**是自行到oracle網站下載jdk的bin文件再手動安裝。（詳情請google）

* * * * *

這裡提供方法一：是修改apt來源

1.修改ubuntu的apt軟體來源

方法一：用指令新增

> apt-add-repository "deb http://old-releases.ubuntu.com/ubuntu/ jaunty
> multiverse"  
> apt-add-repository "deb http://old-releases.ubuntu.com/ubuntu/
> jaunty-updates multiverse"

 

方法二：用vim新增

安裝vim編輯器：

> root@james-VirtualBox:/home/james/mydroid\# apt-get install vim

[![image](http://lh5.ggpht.com/-KJlakEm6_pE/TjEQLlF9zXI/AAAAAAAAJqk/WGDvYhLvJ-w/image_thumb%25255B11%25255D.png?imgmax=800 "image")](http://lh3.ggpht.com/-aqHwZUuMrBo/TjEQKr0vO_I/AAAAAAAAJqg/0xD8AV-vcT0/s1600-h/image%25255B21%25255D.png)

>   
> 正在讀取套件清單... 完成  
> 正在重建相依關係  
> 正在讀取狀態資料... 完成  
> 下列的額外套件將被安裝：  
> vim-runtime  
> 建議套件：  
> ctags vim-doc vim-scripts  
> 下列【新】套件將會被安裝：  
> vim vim-runtime  
> 升級 0 個，新安裝 2 個，移除 0 個，有 195 個未被升級。  
> 需要下載 6,827 kB 的套件檔。  
> 此操作完成之後，會多佔用 28.2 MB 的磁碟空間。  
> 是否繼續進行 [Y/n]？y  
> 下載:1 http://tw.archive.ubuntu.com/ubuntu/ natty/main vim-runtime all
> 2:7.3.035+hg\~8fdc12103333-1ubuntu7 [5,974 kB]  
> 下載:2 http://tw.archive.ubuntu.com/ubuntu/ natty/main vim i386
> 2:7.3.035+hg\~8fdc12103333-1ubuntu7 [853 kB]  
> 取得 6,827 kB 用了 5s (1,272 kB/s)  
> 選取了原先未被選取的套件 vim-runtime。  
> （正在讀取資料庫 ... 133826 files and directories currently
> installed.)  
> 正在解開 vim-runtime （從
> .../vim-runtime\_2%3a7.3.035+hg\~8fdc12103333-1ubuntu7\_all.deb）...  
> Adding 'diversion of /usr/share/vim/vim73/doc/help.txt to
> /usr/share/vim/vim73/doc/help.txt.vim-tiny by vim-runtime'  
> Adding 'diversion of /usr/share/vim/vim73/doc/tags to
> /usr/share/vim/vim73/doc/tags.vim-tiny by vim-runtime'  
> 選取了原先未被選取的套件 vim。  
> 正在解開 vim （從
> .../vim\_2%3a7.3.035+hg\~8fdc12103333-1ubuntu7\_i386.deb）...  
> 正在進行 man-db 的觸發程式 ...  
> 正在設定 vim-runtime (2:7.3.035+hg\~8fdc12103333-1ubuntu7) ...  
> Processing /usr/share/vim/addons/doc  
> 正在設定 vim (2:7.3.035+hg\~8fdc12103333-1ubuntu7) ...  
> update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/vim
> (vim) in auto mode.  
> update-alternatives: using /usr/bin/vim.basic to provide
> /usr/bin/vimdiff (vimdiff) in auto mode.  
> update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/rvim
> (rvim) in auto mode.  
> update-alternatives: using /usr/bin/vim.basic to provide
> /usr/bin/rview (rview) in auto mode.  
> update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/vi
> (vi) in auto mode.  
> update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/view
> (view) in auto mode.  
> update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/ex
> (ex) in auto mode.  
> root@james-VirtualBox:/home/james/mydroid\#
>
> 安裝完成vim

* * * * *

> root@james-VirtualBox:/home/james/mydroid\#  vim
> /etc/apt/sources.list

進入 vim 後，按 i 進入 i-mode，就可以編寫您的文件

在結尾增加4行：

> deb http://old-releases.ubuntu.com/ubuntu/ jaunty multiverse  
> deb-src http://old-releases.ubuntu.com/ubuntu/ jaunty multiverse  
> deb http://old-releases.ubuntu.com/ubuntu/ jaunty-updates multiverse  
> deb-src http://old-releases.ubuntu.com/ubuntu/ jaunty-updates
> multiverse

如果您寫好您的文件，就可以按 Esc 回到 c-mode，  
然後 :w 就會存檔（注意，是冒號命令），但還不會離開 vim，  
要離開可按 :q，就可以了！  
也可以合起來用，:wq，就樣就會存檔後離開。  
然後執行（更新sources.list）

> \$ sudo apt-get update

[![image](http://lh5.ggpht.com/-EweRSxJ0X5E/TjEQNXs6JyI/AAAAAAAAJqs/yXa8TKRQhiA/image_thumb%25255B8%25255D.png?imgmax=800 "image")](http://lh5.ggpht.com/-qCPL-XxoCBY/TjEQMpJp-UI/AAAAAAAAJqo/_ZhNZudIdAc/s1600-h/image%25255B14%25255D.png)

完成！

* * * * *

3.下載安裝sun-java5-jdk

sudo apt-get install sun-java5-jdk

安裝過程會有一個圖形界面，如果你按不了“確定”，請用TAB
鍵切過去就可以了。  
安裝完看一下版本

> root@james-VirtualBox:/home/james/mydroid\# java -version  
> java version "1.6.0\_22"  
> OpenJDK Runtime Environment (IcedTea6 1.10.2)
> (6b22-1.10.2-0ubuntu1\~11.04.1)  
> OpenJDK Client VM (build 20.0-b11, mixed mode, sharing)

安裝好sun-java5-jdk後，預設還是1.6，此時系統也有2種java
jdk，可選擇使用哪個java jdk。

> sudo update-alternatives --config java

接著就會給你選擇，選1.5的代號數字

> There are 2 choices for the alternative java (providing
> /usr/bin/java).
>
>   Selection    Path                                      優先級 
> Status  
> ------------------------------------------------------------  
> \* 0            /usr/lib/jvm/java-6-openjdk/jre/bin/java   1061     
> auto mode  
>   1            /usr/lib/jvm/java-1.5.0-sun/jre/bin/java   53       
> manual mode  
>   2            /usr/lib/jvm/java-6-openjdk/jre/bin/java   1061     
> manual mode
>
> Press enter to keep the current choice[\*], or type selection number:
> 1  

再次看版本

root@james-VirtualBox:/home/james/mydroid\# java -version  
java version "1.5.0\_19"  
Java(TM) 2 Runtime Environment, Standard Edition (build 1.5.0\_19-b02)  
Java HotSpot(TM) Client VM (build 1.5.0\_19-b02, mixed mode, sharing)  

[![image](http://lh3.ggpht.com/-Wc3Kku-KIlE/TjEQOhe3o8I/AAAAAAAAJq0/EUMQaItYDMw/image_thumb%25255B10%25255D.png?imgmax=800 "image")](http://lh5.ggpht.com/-Xb0-iRBXMdg/TjEQOAdfVpI/AAAAAAAAJqw/K41TGxfiYR8/s1600-h/image%25255B18%25255D.png)  

安裝完後，可以再把sourc es.list改回去（並\$ sudo apt-get update）。

 

參考：

-   <http://superuser.com/questions/291277/cant-install-build-essential-on-ubuntu-jaunty-9-04>
-   <http://www.cublog.cn/u/23444/showart_2417806.html>
-   <http://hi.baidu.com/suntolee/blog/item/328871078bd9e9f237d12268.html>
-   <http://jimmynuts.blogspot.com/search/label/ubuntu%2010.04>

