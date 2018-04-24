---
layout: post
title: '跨手機App之PhoneGap入門'
author: 'James Peng'
tags: ['Android']
---

#####  

##### ![[image%255B4%255D.png]](http://lh4.ggpht.com/-1FnnE7wXycQ/TkD7hPPDWII/AAAAAAAAK3I/n_j4fBIu8Y4/s1600/image%25255B4%25255D.png)

#####  

PhoneGap是一個跨平台行動開發解決方案，跨越行動網頁與原生軟體的隔閡，透過很少的修改便移植到不同的平台，如iphone、android、等。

跨平台手機app介紹可以參考：[給APP開發者：你可以有更聰明的選擇](http://www.inside.com.tw/2011/03/14/smarter-choice-app-developer)

* * * * *

##### 1. 需求

-   Eclipse 3.4+

不使用 Eclipse 可以參考 使用
[Terminal](http://wiki.phonegap.com/w/page/30864168/phonegap-android-terminal-quickstart)
的版本 .

* * * * *

##### 2. 安裝 SDK + PhoneGap

![](http://www.phonegap.com/assets/sdk/eclipse.png)下載並安裝 [Eclipse
Classic](http://www.eclipse.org/downloads/)

![](http://www.phonegap.com/assets/os/android.png)下載並安裝 [Android
SDK](http://developer.android.com/sdk/index.html)

![](http://www.phonegap.com/assets/os/android.png)下載並安裝 [ADT
Plugin](http://developer.android.com/sdk/eclipse-adt.html#installing)

![](http://www.phonegap.com/assets/sdk/phonegap.png)[下載](https://github.com/phonegap/phonegap/zipball/1.0.0)
最新版的 PhoneGap 並解壓  

* * * * *

##### 3. 開新專案

-   啟動Eclipse, **[Flie] -\> [New]** –\> 新增一個**[Android Project]**

![](http://www.phonegap.com/assets/guide/New%20Android%20Project.jpeg)

 

-   在專案的根目錄, 建立兩個新資料夾:
    -   /libs
    -   /assets/www

 

-   複製 phonegap.js 到 /assets/www
-   複製 phonegap.jar 到 /libs
-   複製 xml 資料夾 到 /res
-   修改 src 目錄裡的部分程式碼
    -   把class裡的 extend 繼承從 **Activity** 改成 **DroidGap**
    -   取代最後一行的 **setContentView()**
        改成**super.loadUrl("file:///android\_asset/www/index.html");**
    -   增加 **import com.phonegap.\*;**
    -   移除 **import android.app.Activity;**

    ![javaSrc](http://www.phonegap.com/assets/guide/javaSrc.jpg)

 

-   這邊您可能會遇到許多錯誤提示, Eclipse 找不到 phonegap-1.0.0.jar.
    這種情況下,
    右鍵點選你剛複製進去的**libs/phonegap.jar**，選擇**[Build Path」-\>
    [Add to Bulid Path]** 錯誤訊息就會消失了！, 你可能需要按(F5)
    重新整理 project .

 

-   滑鼠右鍵點 AndroidManifest.xml 並選 **Open With \> Text Editor**
-   貼上下列程式碼,在 **android:versionName=”1.0″\>** 下面一行:
    (請參考下圖)

> \<supports-screens  
> android:largeScreens="true"  
> android:normalScreens="true"  
> android:smallScreens="true"  
> android:resizeable="true"  
> android:anyDensity="true"  
> /\>  
> \<uses-permission android:name="android.permission.CAMERA" /\>  
> \<uses-permission android:name="android.permission.VIBRATE" /\>  
> \<uses-permission
> android:name="android.permission.ACCESS\_COARSE\_LOCATION" /\>  
> \<uses-permission
> android:name="android.permission.ACCESS\_FINE\_LOCATION" /\>  
> \<uses-permission
> android:name="android.permission.ACCESS\_LOCATION\_EXTRA\_COMMANDS"
> /\>  
> \<uses-permission android:name="android.permission.READ\_PHONE\_STATE"
> /\>  
> \<uses-permission android:name="android.permission.INTERNET" /\>  
> \<uses-permission android:name="android.permission.RECEIVE\_SMS" /\>  
> \<uses-permission android:name="android.permission.RECORD\_AUDIO"
> /\>  
> \<uses-permission
> android:name="android.permission.MODIFY\_AUDIO\_SETTINGS" /\>  
> \<uses-permission android:name="android.permission.READ\_CONTACTS"
> /\>  
> \<uses-permission android:name="android.permission.WRITE\_CONTACTS"
> /\>  
> \<uses-permission
> android:name="android.permission.WRITE\_EXTERNAL\_STORAGE" /\>  
> \<uses-permission
> android:name="android.permission.ACCESS\_NETWORK\_STATE" /\>
> \<uses-permission android:name="android.permission.GET\_ACCOUNTS" /\>

-   增加 `android:configChanges="orientation|keyboardHidden"` 到
    \<activity\>標籤裡android:label="@string/app\_name" 之後（如下）

> \<activity android:name="com.phonegap.DroidGap"
> android:label="@string/app\_name"
> android:configChanges="orientation|keyboardHidden"\> \<intent-filter\>
> \</intent-filter\> \</activity\>

* * * * *

##### 4. Hello World

 

-   建立新檔案 **index.html** 到 **/assets/www** 資料夾.
    貼上下面程式碼：

> \<!DOCTYPE HTML\>  
> \<html\>  
>   \<head\>  
>     \<meta name="viewport" content="width=320; user-scalable=no" /\>  
>     \<meta http-equiv="Content-type" content="text/html;
> charset=utf-8"\>  
>     \<title\>PhoneGap\</title\>  
>       \<link rel="stylesheet" href="master.css" type="text/css"
> media="screen" title="no title" charset="utf-8"\>  
>       \<script type="text/javascript" charset="utf-8"
> src="phonegap-1.0.0.js"\>\</script\>  
>       \<script type="text/javascript" charset="utf-8"
> src="main.js"\>\</script\> 
>
>   \</head\>  
>   \<body onload="init();" id="stage" class="theme"\>  
>     \<h1\>Welcome to PhoneGap!\</h1\>  
>     \<h2\>this file is located at assets/www/index.html\</h2\>  
>     \<div id="info"\>  
>         \<h4\>Platform: \<span id="platform"\> &nbsp;\</span\>,  
> Version: \<span id="version"\>&nbsp;\</span\>\</h4\>  
>         \<h4\>UUID: \<span id="uuid"\> &nbsp;\</span\>,   Name: \<span
> id="name"\>&nbsp;\</span\>\</h4\>  
>         \<h4\>Width: \<span id="width"\> &nbsp;\</span\>,   Height:
> \<span id="height"\>&nbsp;  
>                    \</span\>, Color Depth: \<span
> id="colorDepth"\>\</span\>\</h4\>  
>      \</div\>  
>     \<dl id="accel-data"\>  
>       \<dt\>X:\</dt\>\<dd id="x"\>&nbsp;\</dd\>  
>       \<dt\>Y:\</dt\>\<dd id="y"\>&nbsp;\</dd\>  
>       \<dt\>Z:\</dt\>\<dd id="z"\>&nbsp;\</dd\>  
>     \</dl\>  
>     \<a href="\#" class="btn large" onclick="toggleAccel();"\>Toggle
> Accelerometer\</a\>  
>     \<a href="\#" class="btn large" onclick="getLocation();"\>Get
> Location\</a\>  
>     \<a href="tel://411" class="btn large"\>Call 411\</a\>  
>     \<a href="\#" class="btn large" onclick="beep();"\>Beep\</a\>  
>     \<a href="\#" class="btn large"
> onclick="vibrate();"\>Vibrate\</a\>  
>     \<a href="\#" class="btn large" onclick="show\_pic();"\>Get a
> Picture\</a\>  
>     \<a href="\#" class="btn large" onclick="get\_contacts();"\>Get
> Phone's Contacts\</a\>  
>     \<a href="\#" class="btn large" onclick="check\_network();"\>Check
> Network\</a\>  
>     \<div id="viewport" class="viewport" style="display:
> none;"\>        
>       \<img style="width:60px;height:60px" id="test\_img" src="" /\>  
>     \</div\>  
>   \</body\>  
> \</html\>  

 

-   建立新檔案 **main.js** 到 **/assets/www** 資料夾. 貼上下面程式碼：

> var deviceInfo = function() {  
>     document.getElementById("platform").innerHTML = device.platform;  
>     document.getElementById("version").innerHTML = device.version;  
>     document.getElementById("uuid").innerHTML = device.uuid;  
>     document.getElementById("name").innerHTML = device.name;  
>     document.getElementById("width").innerHTML = screen.width;  
>     document.getElementById("height").innerHTML = screen.height;  
>     document.getElementById("colorDepth").innerHTML =
> screen.colorDepth;  
> };
>
> var getLocation = function() {  
>     var suc = function(p) {  
>         alert(p.coords.latitude + " " + p.coords.longitude);  
>     };  
>     var locFail = function() {  
>     };  
>     navigator.geolocation.getCurrentPosition(suc, locFail);  
> };
>
> var beep = function() {  
>     navigator.notification.beep(2);  
> };
>
> var vibrate = function() {  
>     navigator.notification.vibrate(0);  
> };
>
> function roundNumber(num) {  
>     var dec = 3;  
>     var result = Math.round(num \* Math.pow(10, dec)) / Math.pow(10,
> dec);  
>     return result;  
> }
>
> var accelerationWatch = null;
>
> function updateAcceleration(a) {  
>     document.getElementById('x').innerHTML = roundNumber(a.x);  
>     document.getElementById('y').innerHTML = roundNumber(a.y);  
>     document.getElementById('z').innerHTML = roundNumber(a.z);  
> }
>
> var toggleAccel = function() {  
>     if (accelerationWatch !== null) {  
>         navigator.accelerometer.clearWatch(accelerationWatch);  
>         updateAcceleration({  
>             x : "",  
>             y : "",  
>             z : ""  
>         });  
>         accelerationWatch = null;  
>     } else {  
>         var options = {};  
>         options.frequency = 1000;  
>         accelerationWatch =
> navigator.accelerometer.watchAcceleration(  
>                 updateAcceleration, function(ex) {  
>                     alert("accel fail (" + ex.name + ": " + ex.message
> + ")");  
>                 }, options);  
>     }  
> };
>
> var preventBehavior = function(e) {  
>     e.preventDefault();  
> };
>
> function dump\_pic(data) {  
>     var viewport = document.getElementById('viewport');  
>     console.log(data);  
>     viewport.style.display = "";  
>     viewport.style.position = "absolute";  
>     viewport.style.top = "10px";  
>     viewport.style.left = "10px";  
>     document.getElementById("test\_img").src =
> "data:image/jpeg;base64," + data;  
> }
>
> function fail(msg) {  
>     alert(msg);  
> }
>
> function show\_pic() {  
>     navigator.camera.getPicture(dump\_pic, fail, {  
>         quality : 50  
>     });  
> }
>
> function close() {  
>     var viewport = document.getElementById('viewport');  
>     viewport.style.position = "relative";  
>     viewport.style.display = "none";  
> }
>
> // This is just to do this.  
> function readFile() {  
>     navigator.file.read('/sdcard/phonegap.txt', fail, fail);  
> }
>
> function writeFile() {  
>     navigator.file.write('foo.txt', "This is a test of writing to a
> file",  
>             fail, fail);  
> }
>
> function contacts\_success(contacts) {  
>     alert(contacts.length  
>             + ' contacts returned.'  
>             + (contacts[2] && contacts[2].name ? (' Third contact is '
> + contacts[2].name.formatted)  
>                     : ''));  
> }
>
> function get\_contacts() {  
>     var obj = new ContactFindOptions();  
>     obj.filter = "";  
>     obj.multiple = true;  
>     obj.limit = 5;  
>     navigator.service.contacts.find(  
>             [ "displayName", "name" ], contacts\_success,  
>             fail, obj);  
> }
>
> var networkReachableCallback = function(reachability) {  
>     // There is no consistency on the format of reachability  
>     var networkState = reachability.code || reachability;
>
>     var currentState = {};  
>     currentState[NetworkStatus.NOT\_REACHABLE] = 'No network
> connection';  
>     currentState[NetworkStatus.REACHABLE\_VIA\_CARRIER\_DATA\_NETWORK]
> = 'Carrier data connection';  
>     currentState[NetworkStatus.REACHABLE\_VIA\_WIFI\_NETWORK] = 'WiFi
> connection';
>
>     confirm("Connection type:\\n" + currentState[networkState]);  
> };
>
> function check\_network() {  
>    
> navigator.network.isReachable("www.mobiledevelopersolutions.com",  
>             networkReachableCallback, {});  
> }
>
> function init() {  
>     // the next line makes it impossible to see Contacts on the HTC
> Evo since it  
>     // doesn't have a scroll button  
>     // document.addEventListener("touchmove", preventBehavior,
> false);  
>     document.addEventListener("deviceready", deviceInfo, true);  
> }  

 

-   建立新檔案 **master.css** 到 **/assets/www** 資料夾.
    貼上下面程式碼：

> body {  
>   background:\#222 none repeat scroll 0 0;  
>   color:\#666;  
>   font-family:Helvetica;  
>   font-size:72%;  
>   line-height:1.5em;  
>   margin:0;  
>   border-top:1px solid \#393939;  
> }
>
> \#info{  
>   background:\#ffa;  
>   border: 1px solid \#ffd324;  
>   -webkit-border-radius: 5px;  
>   border-radius: 5px;  
>   clear:both;  
>   margin:15px 6px 0;  
>   width:295px;  
>   padding:4px 0px 2px 10px;  
> }
>
> \#info \> h4{  
>   font-size:.95em;  
>   margin:5px 0;  
> }  
>       
> \#stage.theme{  
>   padding-top:3px;  
> }
>
> /\* Definition List \*/  
> \#stage.theme \> dl{  
>     padding-top:10px;  
>     clear:both;  
>     margin:0;  
>     list-style-type:none;  
>     padding-left:10px;  
>     overflow:auto;  
> }
>
> \#stage.theme \> dl \> dt{  
>     font-weight:bold;  
>     float:left;  
>     margin-left:5px;  
> }
>
> \#stage.theme \> dl \> dd{  
>     width:45px;  
>     float:left;  
>     color:\#a87;  
>     font-weight:bold;  
> }
>
> /\* Content Styling \*/  
> \#stage.theme \> h1, \#stage.theme \> h2, \#stage.theme \> p{  
>   margin:1em 0 .5em 13px;  
> }
>
> \#stage.theme \> h1{  
>   color:\#eee;  
>   font-size:1.6em;  
>   text-align:center;  
>   margin:0;  
>   margin-top:15px;  
>   padding:0;  
> }
>
> \#stage.theme \> h2{  
>     clear:both;  
>   margin:0;  
>   padding:3px;  
>   font-size:1em;  
>   text-align:center;  
> }
>
> /\* Stage Buttons \*/  
> \#stage.theme a.btn{  
>     border: 1px solid \#555;  
>     -webkit-border-radius: 5px;  
>     border-radius: 5px;  
>     text-align:center;  
>     display:block;  
>     float:left;  
>     background:\#444;  
>     width:150px;  
>     color:\#9ab;  
>     font-size:1.1em;  
>     text-decoration:none;  
>     padding:1.2em 0;  
>     margin:3px 0px 3px 5px;  
> }  
> \#stage.theme a.btn.large{  
>     width:308px;  
>     padding:1.2em 0;  
> }
>
>  

 

執行如下：

[![image](http://lh4.ggpht.com/-DlDvOd39t-o/TkD7iIwLbcI/AAAAAAAAK3M/9SmqsmhiRhU/image_thumb%25255B7%25255D.png?imgmax=800 "image")](http://lh4.ggpht.com/-1FnnE7wXycQ/TkD7hPPDWII/AAAAAAAAK3I/n_j4fBIu8Y4/s1600-h/image%25255B4%25255D.png)

 

* * * * *

參考：

-   <http://www.phonegap.com/start#android>
-   <http://www.inside.com.tw/2011/01/29/hello-inside-phonegap>
-   <http://www.inside.com.tw/2010/08/15/phonegap-eliminates-the-gap-between-mobile-web-and-native-apps>

