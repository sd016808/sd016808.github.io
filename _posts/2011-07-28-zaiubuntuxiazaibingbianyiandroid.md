---
layout: post
title: '在ubuntu下載並編譯Android'
author: 'James Peng'
tags: ['Android','Linux']
---

 

在ubuntu下載Android：

1.安裝git

> sudo apt-get install git

過程：

[![image](http://lh6.ggpht.com/-YzP7lFRKSxE/TjETx7jV-cI/AAAAAAAAJq8/G5traH2WluE/image_thumb%25255B4%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-jF-41RnGXHU/TjETxKXiTKI/AAAAAAAAJq4/xA6VVYeLXbM/s1600-h/image%25255B6%25255D.png)

2.安裝curl

> sudo apt-get install curl

過程：

[![image](http://lh6.ggpht.com/-zuV7yARYmtA/TjETzL1wSCI/AAAAAAAAJrE/ejDHOtdcTZ0/image_thumb%25255B7%25255D.png?imgmax=800 "image")](http://lh3.ggpht.com/-FW6jw3MJn58/TjETyp5jiHI/AAAAAAAAJrA/4SeHcRWDpqk/s1600-h/image%25255B11%25255D.png)

 

3.安裝google 程式碼版本控制程式 repo 在 home 目錄

> cd \~  
> mkdir bin  
> \# 下載 repo ，加上執行權限。  
> curl http://android.git.kernel.org/repo \>\~/bin/repo  
> chmod a+x \~/bin/repo  
> \# 將 repo 加入路徑  
> echo "export PATH=\${PATH}:\~/bin/" \>\> \~/.bashrc  
> . \~/.bashrc

過程：

[![image](http://lh6.ggpht.com/-nWQNBsiSO1w/TjET0OCU9YI/AAAAAAAAJrM/pIYnNVYkhEY/image_thumb%25255B10%25255D.png?imgmax=800 "image")](http://lh3.ggpht.com/-bjuECSvVP7U/TjETzhHz9TI/AAAAAAAAJrI/2XEBurObDHQ/s1600-h/image%25255B16%25255D.png)

4.開始下載 Android 2.1 奶油麵包 eclair

> mkdir mydroid  
> cd mydroid  
> repo init -u git://android.git.kernel.org/platform/manifest.git -b
> eclair  

 

\# 也可以改用這行來抓 Android 2.2 冷凍優格 froyo  
\# repo init -u git://android.git.kernel.org/platform/manifest.git -b
froyo

 

過程：

<table>
<colgroup>
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p>james@james-VirtualBox:~$ mkdir mydroid<br />james@james-VirtualBox:~$ cd mydroid<br />james@james-VirtualBox:~/mydroid$ repo init  -u git://android.git.kernel.org/platform/manifest.git -b eclair<br />gpg: `/home/james/.repoconfig/gnupg/secring.gpg' 鑰匙圈已建立<br />gpg: `/home/james/.repoconfig/gnupg/pubring.gpg' 鑰匙圈已建立<br />gpg: /home/james/.repoconfig/gnupg/trustdb.gpg: 建立了信任資料庫<br />gpg: 金鑰 920F5C65: 公鑰 &quot;Repo Maintainer &lt;repo@android.kernel.org&gt;&quot; 已匯入<br />gpg: 處理總量: 1<br />gpg:               已匯入: 1</p>
<p>Getting repo ...<br />   from git://android.git.kernel.org/tools/repo.git<br />remote: Counting objects: 1309, done.<br />remote: Compressing objects: 100% (575/575), done.<br />remote: Total 1309 (delta 843), reused 1140 (delta 711)<br />Receiving objects: 100% (1309/1309), 358.04 KiB | 205 KiB/s, done.<br />Resolving deltas: 100% (843/843), done.<br />From git://android.git.kernel.org/tools/repo<br />* [new branch]      maint      -&gt; origin/maint<br />* [new branch]      master     -&gt; origin/master<br />* [new branch]      stable     -&gt; origin/stable<br />* [new tag]         v1.7.5     -&gt; v1.7.5<br />From git://android.git.kernel.org/tools/repo<br />* [new tag]         v1.0       -&gt; v1.0<br />* [new tag]         v1.0.1     -&gt; v1.0.1<br />* [new tag]         v1.0.2     -&gt; v1.0.2<br />* [new tag]         v1.0.3     -&gt; v1.0.3<br />* [new tag]         v1.0.4     -&gt; v1.0.4<br />* [new tag]         v1.0.5     -&gt; v1.0.5<br />* [new tag]         v1.0.6     -&gt; v1.0.6<br />* [new tag]         v1.0.7     -&gt; v1.0.7<br />* [new tag]         v1.0.8     -&gt; v1.0.8<br />* [new tag]         v1.0.9     -&gt; v1.0.9<br />* [new tag]         v1.1       -&gt; v1.1<br />* [new tag]         v1.2       -&gt; v1.2<br />* [new tag]         v1.3       -&gt; v1.3<br />* [new tag]         v1.3.1     -&gt; v1.3.1<br />* [new tag]         v1.3.2     -&gt; v1.3.2<br />* [new tag]         v1.4       -&gt; v1.4<br />* [new tag]         v1.4.1     -&gt; v1.4.1<br />* [new tag]         v1.4.2     -&gt; v1.4.2<br />* [new tag]         v1.4.3     -&gt; v1.4.3<br />* [new tag]         v1.4.4     -&gt; v1.4.4<br />* [new tag]         v1.5       -&gt; v1.5<br />* [new tag]         v1.5.1     -&gt; v1.5.1<br />* [new tag]         v1.6       -&gt; v1.6<br />* [new tag]         v1.6.1     -&gt; v1.6.1<br />* [new tag]         v1.6.10    -&gt; v1.6.10<br />* [new tag]         v1.6.10.1  -&gt; v1.6.10.1<br />* [new tag]         v1.6.10.2  -&gt; v1.6.10.2<br />* [new tag]         v1.6.2     -&gt; v1.6.2<br />* [new tag]         v1.6.3     -&gt; v1.6.3<br />* [new tag]         v1.6.4     -&gt; v1.6.4<br />* [new tag]         v1.6.5     -&gt; v1.6.5<br />* [new tag]         v1.6.6     -&gt; v1.6.6<br />* [new tag]         v1.6.7     -&gt; v1.6.7<br />* [new tag]         v1.6.7.1   -&gt; v1.6.7.1<br />* [new tag]         v1.6.7.2   -&gt; v1.6.7.2<br />* [new tag]         v1.6.7.3   -&gt; v1.6.7.3<br />* [new tag]         v1.6.7.4   -&gt; v1.6.7.4<br />* [new tag]         v1.6.7.5   -&gt; v1.6.7.5<br />* [new tag]         v1.6.8     -&gt; v1.6.8<br />* [new tag]         v1.6.8.1   -&gt; v1.6.8.1<br />* [new tag]         v1.6.8.10  -&gt; v1.6.8.10<br />* [new tag]         v1.6.8.11  -&gt; v1.6.8.11<br />* [new tag]         v1.6.8.2   -&gt; v1.6.8.2<br />* [new tag]         v1.6.8.3   -&gt; v1.6.8.3<br />* [new tag]         v1.6.8.4   -&gt; v1.6.8.4<br />* [new tag]         v1.6.8.5   -&gt; v1.6.8.5<br />* [new tag]         v1.6.8.6   -&gt; v1.6.8.6<br />* [new tag]         v1.6.8.7   -&gt; v1.6.8.7<br />* [new tag]         v1.6.8.8   -&gt; v1.6.8.8<br />* [new tag]         v1.6.8.9   -&gt; v1.6.8.9<br />* [new tag]         v1.6.9     -&gt; v1.6.9<br />* [new tag]         v1.6.9.1   -&gt; v1.6.9.1<br />* [new tag]         v1.6.9.2   -&gt; v1.6.9.2<br />* [new tag]         v1.6.9.3   -&gt; v1.6.9.3<br />* [new tag]         v1.6.9.4   -&gt; v1.6.9.4<br />* [new tag]         v1.6.9.5   -&gt; v1.6.9.5<br />* [new tag]         v1.6.9.6   -&gt; v1.6.9.6<br />* [new tag]         v1.6.9.7   -&gt; v1.6.9.7<br />* [new tag]         v1.6.9.8   -&gt; v1.6.9.8<br />* [new tag]         v1.7       -&gt; v1.7<br />* [new tag]         v1.7.1     -&gt; v1.7.1<br />* [new tag]         v1.7.2     -&gt; v1.7.2<br />* [new tag]         v1.7.3     -&gt; v1.7.3<br />* [new tag]         v1.7.3.1   -&gt; v1.7.3.1<br />* [new tag]         v1.7.4     -&gt; v1.7.4<br />* [new tag]         v1.7.4.1   -&gt; v1.7.4.1<br />* [new tag]         v1.7.4.2   -&gt; v1.7.4.2<br />* [new tag]         v1.7.4.3   -&gt; v1.7.4.3<br />Getting manifest ...<br />   from git://android.git.kernel.org/platform/manifest.git<br />remote: Counting objects: 854, done.<br />remote: Compressing objects: 100% (356/356), done.<br />remote: Total 854 (delta 357), reused 807 (delta 337)<br />Receiving objects: 100% (854/854), 244.79 KiB | 196 KiB/s, done.<br />Resolving deltas: 100% (357/357), done.<br />From git://android.git.kernel.org/platform/manifest<br />* [new branch]      android-1.5 -&gt; origin/android-1.5<br />* [new branch]      android-1.5r2 -&gt; origin/android-1.5r2<br />* [new branch]      android-1.5r3 -&gt; origin/android-1.5r3<br />* [new branch]      android-1.5r4 -&gt; origin/android-1.5r4<br />* [new branch]      android-1.6_r1 -&gt; origin/android-1.6_r1<br />* [new branch]      android-1.6_r1.1 -&gt; origin/android-1.6_r1.1<br />* [new branch]      android-1.6_r1.2 -&gt; origin/android-1.6_r1.2<br />* [new branch]      android-1.6_r1.3 -&gt; origin/android-1.6_r1.3<br />* [new branch]      android-1.6_r1.4 -&gt; origin/android-1.6_r1.4<br />* [new branch]      android-1.6_r1.5 -&gt; origin/android-1.6_r1.5<br />* [new branch]      android-1.6_r2 -&gt; origin/android-1.6_r2<br />* [new branch]      android-2.0.1_r1 -&gt; origin/android-2.0.1_r1<br />* [new branch]      android-2.0_r1 -&gt; origin/android-2.0_r1<br />* [new branch]      android-2.1_r1 -&gt; origin/android-2.1_r1<br />* [new branch]      android-2.1_r2 -&gt; origin/android-2.1_r2<br />* [new branch]      android-2.1_r2.1p -&gt; origin/android-2.1_r2.1p<br />* [new branch]      android-2.1_r2.1p2 -&gt; origin/android-2.1_r2.1p2<br />* [new branch]      android-2.1_r2.1s -&gt; origin/android-2.1_r2.1s<br />* [new branch]      android-2.2.1_r1 -&gt; origin/android-2.2.1_r1<br />* [new branch]      android-2.2.1_r2 -&gt; origin/android-2.2.1_r2<br />* [new branch]      android-2.2.2_r1 -&gt; origin/android-2.2.2_r1<br />* [new branch]      android-2.2_r1 -&gt; origin/android-2.2_r1<br />* [new branch]      android-2.2_r1.1 -&gt; origin/android-2.2_r1.1<br />* [new branch]      android-2.2_r1.2 -&gt; origin/android-2.2_r1.2<br />* [new branch]      android-2.2_r1.3 -&gt; origin/android-2.2_r1.3<br />* [new branch]      android-2.3.1_r1 -&gt; origin/android-2.3.1_r1<br />* [new branch]      android-2.3.2_r1 -&gt; origin/android-2.3.2_r1<br />* [new branch]      android-2.3.3_r1 -&gt; origin/android-2.3.3_r1<br />* [new branch]      android-2.3.3_r1.1 -&gt; origin/android-2.3.3_r1.1<br />* [new branch]      android-2.3.4_r0.9 -&gt; origin/android-2.3.4_r0.9<br />* [new branch]      android-2.3.4_r1 -&gt; origin/android-2.3.4_r1<br />* [new branch]      android-2.3.5_r1 -&gt; origin/android-2.3.5_r1<br />* [new branch]      android-2.3_r1 -&gt; origin/android-2.3_r1<br />* [new branch]      android-adt-0.9.8 -&gt; origin/android-adt-0.9.8<br />* [new branch]      android-adt-0.9.9 -&gt; origin/android-adt-0.9.9<br />* [new branch]      android-cts-2.1_r2 -&gt; origin/android-cts-2.1_r2<br />* [new branch]      android-cts-2.1_r3 -&gt; origin/android-cts-2.1_r3<br />* [new branch]      android-cts-2.1_r4 -&gt; origin/android-cts-2.1_r4<br />* [new branch]      android-cts-2.1_r5 -&gt; origin/android-cts-2.1_r5<br />* [new branch]      android-cts-2.2_r1 -&gt; origin/android-cts-2.2_r1<br />* [new branch]      android-cts-2.2_r2 -&gt; origin/android-cts-2.2_r2<br />* [new branch]      android-cts-2.2_r3 -&gt; origin/android-cts-2.2_r3<br />* [new branch]      android-cts-2.2_r4 -&gt; origin/android-cts-2.2_r4<br />* [new branch]      android-cts-2.2_r5 -&gt; origin/android-cts-2.2_r5<br />* [new branch]      android-cts-2.2_r6 -&gt; origin/android-cts-2.2_r6<br />* [new branch]      android-cts-2.3_r1 -&gt; origin/android-cts-2.3_r1<br />* [new branch]      android-cts-2.3_r2 -&gt; origin/android-cts-2.3_r2<br />* [new branch]      android-cts-2.3_r3 -&gt; origin/android-cts-2.3_r3<br />* [new branch]      android-cts-2.3_r4 -&gt; origin/android-cts-2.3_r4<br />* [new branch]      android-sdk-1.5-pre -&gt; origin/android-sdk-1.5-pre<br />* [new branch]      android-sdk-1.5_r1 -&gt; origin/android-sdk-1.5_r1<br />* [new branch]      android-sdk-1.5_r3 -&gt; origin/android-sdk-1.5_r3<br />* [new branch]      android-sdk-1.6-docs_r1 -&gt; origin/android-sdk-1.6-docs_r1<br />* [new branch]      android-sdk-1.6_r1 -&gt; origin/android-sdk-1.6_r1<br />* [new branch]      android-sdk-1.6_r2 -&gt; origin/android-sdk-1.6_r2<br />* [new branch]      android-sdk-2.0.1-docs_r1 -&gt; origin/android-sdk-2.0.1-docs_r1<br />* [new branch]      android-sdk-2.0.1_r1 -&gt; origin/android-sdk-2.0.1_r1<br />* [new branch]      android-sdk-2.0_r1 -&gt; origin/android-sdk-2.0_r1<br />* [new branch]      android-sdk-2.1_r1 -&gt; origin/android-sdk-2.1_r1<br />* [new branch]      android-sdk-2.2_r1 -&gt; origin/android-sdk-2.2_r1<br />* [new branch]      android-sdk-2.2_r2 -&gt; origin/android-sdk-2.2_r2<br />* [new branch]      android-sdk-2.3.4_r1 -&gt; origin/android-sdk-2.3.4_r1<br />* [new branch]      android-sdk-adt_r12 -&gt; origin/android-sdk-adt_r12<br />* [new branch]      android-sdk-tools_r12 -&gt; origin/android-sdk-tools_r12<br />* [new branch]      android-sdk-tools_r2 -&gt; origin/android-sdk-tools_r2<br />* [new branch]      android-sdk-tools_r3 -&gt; origin/android-sdk-tools_r3<br />* [new branch]      android-sdk-tools_r4 -&gt; origin/android-sdk-tools_r4<br />* [new branch]      android-sdk-tools_r5 -&gt; origin/android-sdk-tools_r5<br />* [new branch]      android-sdk-tools_r6 -&gt; origin/android-sdk-tools_r6<br />* [new branch]      android-sdk-tools_r7 -&gt; origin/android-sdk-tools_r7<br />* [new branch]      cdma-import -&gt; origin/cdma-import<br />* [new branch]      cupcake    -&gt; origin/cupcake<br />* [new branch]      cupcake-release -&gt; origin/cupcake-release<br />* [new branch]      donut      -&gt; origin/donut<br />* [new branch]      donut-plus-aosp -&gt; origin/donut-plus-aosp<br />* [new branch]      eclair     -&gt; origin/eclair<br />* [new branch]      froyo      -&gt; origin/froyo<br />* [new branch]      froyo-plus-aosp -&gt; origin/froyo-plus-aosp<br />* [new branch]      gingerbread -&gt; origin/gingerbread<br />* [new branch]      master     -&gt; origin/master<br />* [new branch]      release-1.0 -&gt; origin/release-1.0<br />* [new branch]      tools-adt_r11 -&gt; origin/tools-adt_r11<br />* [new branch]      tools_r10  -&gt; origin/tools_r10<br />* [new branch]      tools_r11  -&gt; origin/tools_r11<br />* [new branch]      tools_r12  -&gt; origin/tools_r12<br />* [new branch]      tools_r7   -&gt; origin/tools_r7<br />* [new branch]      tools_r8   -&gt; origin/tools_r8<br />* [new branch]      tools_r9   -&gt; origin/tools_r9<br />* [new tag]         android-1.5 -&gt; android-1.5<br />* [new tag]         android-1.5r2 -&gt; android-1.5r2<br />* [new tag]         android-1.5r3 -&gt; android-1.5r3<br />* [new tag]         android-1.5r4 -&gt; android-1.5r4<br />* [new tag]         android-1.6_r1 -&gt; android-1.6_r1<br />* [new tag]         android-1.6_r1.1 -&gt; android-1.6_r1.1<br />* [new tag]         android-1.6_r1.2 -&gt; android-1.6_r1.2<br />* [new tag]         android-1.6_r1.3 -&gt; android-1.6_r1.3<br />* [new tag]         android-1.6_r1.4 -&gt; android-1.6_r1.4<br />* [new tag]         android-1.6_r1.5 -&gt; android-1.6_r1.5<br />* [new tag]         android-1.6_r2 -&gt; android-1.6_r2<br />* [new tag]         android-2.0.1_r1 -&gt; android-2.0.1_r1<br />* [new tag]         android-2.0_r1 -&gt; android-2.0_r1<br />* [new tag]         android-2.1_r1 -&gt; android-2.1_r1<br />* [new tag]         android-2.1_r2 -&gt; android-2.1_r2<br />* [new tag]         android-2.1_r2.1p -&gt; android-2.1_r2.1p<br />* [new tag]         android-2.1_r2.1p2 -&gt; android-2.1_r2.1p2<br />* [new tag]         android-2.1_r2.1s -&gt; android-2.1_r2.1s<br />* [new tag]         android-2.2.1_r1 -&gt; android-2.2.1_r1<br />* [new tag]         android-2.2.1_r2 -&gt; android-2.2.1_r2<br />* [new tag]         android-2.2.2_r1 -&gt; android-2.2.2_r1<br />* [new tag]         android-2.2_r1 -&gt; android-2.2_r1<br />* [new tag]         android-2.2_r1.1 -&gt; android-2.2_r1.1<br />* [new tag]         android-2.2_r1.2 -&gt; android-2.2_r1.2<br />* [new tag]         android-2.2_r1.3 -&gt; android-2.2_r1.3<br />* [new tag]         android-2.3.1_r1 -&gt; android-2.3.1_r1<br />* [new tag]         android-2.3.2_r1 -&gt; android-2.3.2_r1<br />* [new tag]         android-2.3.3_r1.1 -&gt; android-2.3.3_r1.1<br />* [new tag]         android-2.3.3_r1a -&gt; android-2.3.3_r1a<br />* [new tag]         android-2.3.4_r0.9 -&gt; android-2.3.4_r0.9<br />* [new tag]         android-2.3.4_r1 -&gt; android-2.3.4_r1<br />* [new tag]         android-2.3.5_r1 -&gt; android-2.3.5_r1<br />* [new tag]         android-2.3_r1 -&gt; android-2.3_r1<br />* [new tag]         android-adt-0.9.8 -&gt; android-adt-0.9.8<br />* [new tag]         android-adt-0.9.9 -&gt; android-adt-0.9.9<br />* [new tag]         android-cts-2.1_r2 -&gt; android-cts-2.1_r2<br />* [new tag]         android-cts-2.1_r3 -&gt; android-cts-2.1_r3<br />* [new tag]         android-cts-2.1_r4 -&gt; android-cts-2.1_r4<br />* [new tag]         android-cts-2.1_r5 -&gt; android-cts-2.1_r5<br />* [new tag]         android-cts-2.2_r1 -&gt; android-cts-2.2_r1<br />* [new tag]         android-cts-2.2_r2 -&gt; android-cts-2.2_r2<br />* [new tag]         android-cts-2.2_r3 -&gt; android-cts-2.2_r3<br />* [new tag]         android-cts-2.2_r4 -&gt; android-cts-2.2_r4<br />* [new tag]         android-cts-2.2_r5 -&gt; android-cts-2.2_r5<br />* [new tag]         android-cts-2.2_r6 -&gt; android-cts-2.2_r6<br />* [new tag]         android-cts-2.3_r1 -&gt; android-cts-2.3_r1<br />* [new tag]         android-cts-2.3_r2 -&gt; android-cts-2.3_r2<br />* [new tag]         android-cts-2.3_r3 -&gt; android-cts-2.3_r3<br />* [new tag]         android-cts-2.3_r4 -&gt; android-cts-2.3_r4<br />* [new tag]         android-sdk-1.5-pre -&gt; android-sdk-1.5-pre<br />* [new tag]         android-sdk-1.5_r1 -&gt; android-sdk-1.5_r1<br />* [new tag]         android-sdk-1.5_r3 -&gt; android-sdk-1.5_r3<br />* [new tag]         android-sdk-1.6-docs_r1 -&gt; android-sdk-1.6-docs_r1<br />* [new tag]         android-sdk-1.6_r1 -&gt; android-sdk-1.6_r1<br />* [new tag]         android-sdk-1.6_r2 -&gt; android-sdk-1.6_r2<br />* [new tag]         android-sdk-2.0.1-docs_r1 -&gt; android-sdk-2.0.1-docs_r1<br />* [new tag]         android-sdk-2.0.1_r1 -&gt; android-sdk-2.0.1_r1<br />* [new tag]         android-sdk-2.0_r1 -&gt; android-sdk-2.0_r1<br />* [new tag]         android-sdk-2.1_r1 -&gt; android-sdk-2.1_r1<br />* [new tag]         android-sdk-2.2_r1 -&gt; android-sdk-2.2_r1<br />* [new tag]         android-sdk-2.2_r2 -&gt; android-sdk-2.2_r2<br />* [new tag]         android-sdk-2.3.4_r1 -&gt; android-sdk-2.3.4_r1<br />* [new tag]         android-sdk-adt_r12 -&gt; android-sdk-adt_r12<br />* [new tag]         android-sdk-tools_r12 -&gt; android-sdk-tools_r12<br />* [new tag]         android-sdk-tools_r2 -&gt; android-sdk-tools_r2<br />* [new tag]         android-sdk-tools_r3 -&gt; android-sdk-tools_r3<br />* [new tag]         android-sdk-tools_r4 -&gt; android-sdk-tools_r4<br />* [new tag]         android-sdk-tools_r5 -&gt; android-sdk-tools_r5<br />* [new tag]         android-sdk-tools_r6 -&gt; android-sdk-tools_r6<br />* [new tag]         android-sdk-tools_r7 -&gt; android-sdk-tools_r7<br />From git://android.git.kernel.org/platform/manifest<br />* [new tag]         android-1.0 -&gt; android-1.0<br />* [new tag]         android-2.3.3_r1 -&gt; android-2.3.3_r1</p>
<p>Your Name  [james]:<br />Your Email [james@james-VirtualBox.(none)]: msn@james.com</p>
<p>Your identity is: james &lt;msn@james.com&gt;<br />is this correct [y/n]? y</p>
<p>Testing colorized output (for 'repo diff', 'repo status'):<br />  black    red      green    yellow   blue     magenta   cyan     white<br />  bold     dim      ul       reverse<br />Enable color display in this user account (y/n)? red</p>
<p>repo initialized in /home/james/mydroid<br />james@james-VirtualBox:~/mydroid$<br /></p></td>
</tr>
</tbody>
</table>

 

5. repo sync 執行就會開始抓程式碼，Android
2.1大概3.XG，大概要好一陣子了XD。

> repo sync  

過程：

[![image](http://lh3.ggpht.com/-n9PyeL1KczQ/TjET1BdxQKI/AAAAAAAAJrU/_QjwdByF3g8/image_thumb%25255B13%25255D.png?imgmax=800 "image")](http://lh5.ggpht.com/-xOvVz3c9Jkc/TjET0lOxGrI/AAAAAAAAJrQ/hlFtDFuHch4/s1600-h/image%25255B21%25255D.png)

[![image](http://lh4.ggpht.com/-doX0i7FNybs/TjET2YbxswI/AAAAAAAAJrc/GTesS_O1rSw/image_thumb%25255B15%25255D.png?imgmax=800 "image")](http://lh6.ggpht.com/-rHvpz5hATeM/TjET15f9GMI/AAAAAAAAJrY/UQyZXLohg6E/s1600-h/image%25255B25%25255D.png)

上面的動作就可以把我們需要的 Android 2.1 抓下來了

* * * * *

在ubuntu編譯Android：

1.我先切回原本的使用者

> su james

2.然後切到剛剛下載的目錄mydroid

> cd mydroid

3.接著設定

> export USE\_CCACHE=1  
> . build/envsetup.sh

4.接著編譯，注意如果你有dual CPUs 核心可以用  -j4 指令

> make -j2  
> make -j2 sdk

於是看到

> Checking build tools versions...  
> \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*  
> You are attempting to build with the incorrect version  
> of java.  
>   
> Your version is: java version "1.6.0\_22".  
> The correct version is: 1.5.  
>   
> Please follow the machine setup instructions at  
> http://source.android.com/download  
> \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*  
> build/core/main.mk:117: \*\*\* stop. Stop.

 

這邊要注意，因為 Android 2.1 沒辦法用 Java 6 來編譯（主要是
javadoc有差），所以要改用 Java 5 ，而 ubuntu 10.04 已經不包含
sun-java5-jdk 套件了，所以我們只好加入 9.04 的套件來源來安裝
sun-java5-jdk。這也是最省事的方法，不然就要自己手動安裝 JDK 1.5
的套件，還要自己設定環境變數 \$JAVA\_HOME 和 \$ANDROID\_JAVA\_HOME。

5.安裝java5方法可以參考：[ubuntu安裝sun-java5-jdk](http://note.james.com/2011/07/ubuntusun-java5-jdk.html)

6.再次編譯

> make -j2  
> make -j2 sdk

[![image](http://lh5.ggpht.com/-J1xUo7PcRp8/TjET32wYzuI/AAAAAAAAJrk/iSf0IqPUWwc/image_thumb%25255B20%25255D.png?imgmax=800 "image")](http://lh5.ggpht.com/-m6C3fzyZLGg/TjET3P3b8dI/AAAAAAAAJrg/wZtPh-EkDg0/s1600-h/image%25255B34%25255D.png)  

接著就等他編譯完成囉！！

* * * * *

參考：

-   <http://jimmynuts.blogspot.com/search/label/ubuntu%2010.04>
-   <http://note.james.com/2011/07/ubuntusun-java5-jdk.html>
-   <http://source.android.com/source/initializing.html>

