---
layout: post
title: 'Ubuntu Android NDK環境的配置'
author: 'James Peng'
tags: ['Android']
---

 

開發環境：Ubuntu 10.04（安裝好JDK6+Android SDK+Eclipse IDK+ADT）

步驟：

 

1、從http://androidappdocs.appspot.com/sdk/ndk/index.html下載最新版的NDK，現在最新版名字是android-ndk-r4b-linux-x86.zip，下載後使用unzip
android-ndk-r4b-linux-x86.zip解壓，解壓後名字為android-ndk-r4b，接下來設置PATH環境變量：export
PATH=\$PATH:/home/guochongxin/android-ndk-r4b，設置該環境變量是因為等會在android-ndk-r4b目錄下的ndk-build程序要被用到；

2、上面這樣就配置好了NDK的開發環境，接下來就創建一個項目來測試一下，步驟如下：

1）、使用Eclipse創建一個Android項目，名字為「HelloNDKJNI」，Build
Target設置為「Android 2.2」，Application
Name設置為「HelloNdkJni」，Package Name設置為「com.gcx.ndkjni」，Create
Activity設置為「.HelloNdkJni」，Min SDK Version設置為「8」；

2）、接下來創建C語言庫，在Eclipse的Package
Explore裡面的HelloNDKJNI項目下創建目錄「jni」，並在該目錄下創建兩文件「Android.mk」和「hello-ndk-jni.c」，如下圖所示：

[![1\_3e4774e3g928f4f1eaf8e&690](http://lh4.ggpht.com/-MmrihAmdWWw/ToGPxp_gcrI/AAAAAAAALNU/lIwnRFOJUlg/1_3e4774e3g928f4f1eaf8e%252526690_thumb.jpg?imgmax=800 "1_3e4774e3g928f4f1eaf8e&690")](http://lh4.ggpht.com/-5Zw41dMr5Mw/ToGPwboNW-I/AAAAAAAALNQ/QTj1Vy1iiK0/s1600-h/1_3e4774e3g928f4f1eaf8e%252526690%25255B2%25255D.jpg)

Android.mk文件內容如下：

> LOCAL\_PATH := \$(call my-dir)
>
> include \$(CLEAR\_VARS)
>
> LOCAL\_MODULE := hello-ndk-jni
>
> LOCAL\_SRC\_FILES := hello-ndk-jni.c
>
> include \$(BUILD\_SHARED\_LIBRARY)

hello-ndk-jni.c文件內容如下：

> \#include \<string.h\>
>
> \#include \<jni.h\>
>
> jstring Java\_com\_gcx\_ndkjni\_HelloNdkJni\_stringFromNDKJNI(
> JNIEnv\* env,jobject thiz )
>
> {
>
> return (\*env)-\>NewStringUTF(env, "Hello from NDK JNI !");
>
> }

3）、編譯創建的C庫，打開終端，進行步驟1中的設置PATH環境變量操作（如果有進行，則可跳過），進入到創建的HelloNDKJNI項目中的jni目錄，執行命令ndk-build，此時會在項目中生成libs和obj目錄，並在裡面生成相應的文件，運行結果如下圖所示：

[![2\_3e4774e3g928f53d45c2e&690](http://lh4.ggpht.com/-qDR2dkaYAXc/ToGPz35z9KI/AAAAAAAALNc/KMJHCzHBAwo/2_3e4774e3g928f53d45c2e%252526690_thumb.jpg?imgmax=800 "2_3e4774e3g928f53d45c2e&690")](http://lh3.ggpht.com/-LsYeqNlcA3Y/ToGPyhjTLWI/AAAAAAAALNY/rxd7TsT7akg/s1600-h/2_3e4774e3g928f53d45c2e%252526690%25255B2%25255D.jpg)

4）、刷新Eclipse中的Package
Explore中的HelloNDKJNI項目，此時obj和libs目錄也添加進去了，在obj/armeabi分支下也多了libhello-ndk-jni.so文件，hello-ndk-jni這個名是根據2-2)步中的Android.mk文件中的LOCAL\_MODULE決定的，接下來修改src/com.gcx.ndkjni分支下的HelloNdkJni.java文件，最後的文件內容如下：

> **package** com.gcx.ndkjni;
>
> **import** android.app.Activity;
>
> **import** android.os.Bundle;
>
> **import** android.widget.TextView;
>
> **public** **class** HelloNdkJni **extends** Activity {
>
> @Override
>
> **public** **void** onCreate(Bundle savedInstanceState) {
>
> **super**.onCreate(savedInstanceState);
>
> TextView tv =**new** TextView(**this**);
>
> tv.setText(stringFromNDKJNI());
>
> setContentView(tv);
>
> }
>
> **public** **native** String stringFromNDKJNI();
>
> **static**{
>
> System.*loadLibrary*("hello-ndk-jni");
>
> }
>
> }

5）、接下來就可以在模擬器運行程序了，運行前關於模擬器的配置及Run
Configurations可以參考《Ubuntu下安裝Android
SDK開發環境（三）》一文，運行結果如下圖所示：

[![3\_3e4774e3g928f55ff2fc2&690](http://lh5.ggpht.com/-RsjyHQ-4enE/ToGP2Ob7u-I/AAAAAAAALNk/yD1lkGvNZ4I/3_3e4774e3g928f55ff2fc2%252526690_thumb.jpg?imgmax=800 "3_3e4774e3g928f55ff2fc2&690")](http://lh5.ggpht.com/-jeiKJP-371A/ToGP0zaExfI/AAAAAAAALNg/_Ev-qfl9E3M/s1600-h/3_3e4774e3g928f55ff2fc2%252526690%25255B2%25255D.jpg)

更多的NDK例子，可以參考第1步中解壓後目錄下的samples目錄下的項目。

參考網址：http://www.eoeandroid.com/viewthread.php?tid=2406&highlight=ndk

<http://www.javaeye.com/topic/729133>

<http://blog.sina.com.cn/s/blog_3e4774e30100mjug.html>

