---
layout: post
title: 'Google Map API 初探'
author: 'James Peng'
tags: ['Google']
---



## 申請一組 Google Maps API Key ##

(1) 所決定的 URL 填入，並按 "Generate API Key" 取得 Key。

![](..\images\2007-10-09-Google_Map_API\J89nAni.jpg)


----------


(2) 會出現三個方塊，分別是：

1. 你取得的 Key
2. 你指定的 URL
3. 一個範例

如下圖：

![](..\images\2007-10-09-Google_Map_API\131qMIM.jpg)

範例如下：

~~~html

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
  "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
  <head>
    <meta http-equiv="content-type" content="text/html; charset=utf-8"/>
    <title>Google Maps JavaScript API Example</title>
    <script src="http://maps.google.com/maps?file=api&v=2&key=SAMPLEKEY123OXJ6inh_POMZgEz456Pyj0pLBST8lk_et49T0789zZOKYIqg"
      type="text/javascript"></script>
    <script type="text/javascript">
    //<![CDATA[
    function load() {
      if (GBrowserIsCompatible()) {
        var map = new GMap2(document.getElementById("map"));
        map.setCenter(new GLatLng(37.4419, -122.1419), 13);
      }
    }
    //]]>
    </script>
  </head>
  <body onload="load()" onunload="GUnload()">
    <div id="map" style="width: 500px; height: 300px"></div>
  </body>
</html>


~~~


說明：


這部分就是用你取得的 Key 去載入 Google Maps API

~~~html
    <script src="http://maps.google.com/maps?file=api&v=2&key=SAMPLEKEY123OXJ6inh_POMZgEz456Pyj0pLBST8lk_et49T0789zZOKYIqg"
      type="text/javascript"></script>
~~~


----------

一個 div 區塊用來擺置地圖，style="width: 300px; height:300px" 則是用來指定地圖區塊大小。

~~~html
<div id="map" style="width: 500px; height: 300px"></div>
~~~


----------

宣告一個 GMap 物件。

~~~javascript
var map = new GMap2(document.getElementById("map"));
~~~


----------

將地圖的中心點固定在經度 37.4419 和緯度 -122.1419，而 Zoom Level 在這範例中設成 13 (1 為最大，數字越大 Zoom Level 越小)

~~~javascript
map.setCenter(new GLatLng(37.4419, -122.1419), 13);
~~~


----------



## 如何取得 經緯度 ##

利用 Google Maps 的大地圖找到你要的地點，將他固定在中間

然後按下網頁上的 Link to this page，這時候 Google Maps 就會顯示出這頁的 URL，URL 上就有該點的經緯度了


![](..\images\2007-10-09-Google_Map_API\O0lqRDi.jpg)

取得ll=24.947124,121.229646這就是經緯度

取得經緯度

http://maps.huge.info/trace.htm

----------

## 加入控制項 ##


預設沒有 Google Maps 的控制項，沒有這個你沒辦法放大縮小地圖，也沒辦法切換衛星空照圖。
如下範例你就可以在你的地圖中加入這兩個控制項：

~~~javascript
map.addControl(new GSmallMapControl());
map.addControl(new GMapTypeControl());
~~~


Google Maps API 內建四種控制項：

- GLargeMapControl : 適合給大型地圖的控制項。
- GSmallMapControl : 適合給小型地圖的控制項。
- GSmallZoomControl : 只有 Zoom Level 的調整，沒有地圖移動控制。
- GMapTypeControl : 顯示地圖型態切換的控制項。


----------

##  折線 ##


GPolyline類的構造器把一個點的數組作為參數，根據給定點的順序創建連接這些點的一系列線段。您還可以指定這些線的顏色，寬度和透明度。顏色應該使用16進制數字表現，如#ff0000而不要用red。GPolyline類不能理解顏色名。

下面的代碼片斷創建兩點之間10個像素寬的紅色折線：

~~~javascript
var polyline = new GPolyline([
  new GLatLng(37.4419, -122.1419),
  new GLatLng(37.4519, -122.1519)
], "#ff0000", 10);
map.addOverlay(polyline);
~~~

在Internet Explorer下，Google地圖使用VML來畫折線(更多信息請看XHTML和VML)。在所有其它的瀏覽器，我們從Google服務器請求線的圖片覆蓋在地圖上，在地圖縮放和拖動的時候刷新圖片。


----------

## Other resources ##

Here are some additional resources. Note that these sites are not owned or supported by Google.

- Google Mapki: http://www.mapki.com/
- Google Maps API Tutorial: http://www.econym.demon.co.uk/googlemaps/
- Esa's Google Maps API Examples: http://koti.mbnet.fi/ojalesa/exam/index.html
- USNaviguide's Maps API Examples: http://maps.huge.info/examples.htm
- Mark McClure's Encoded Polyline Examples: http://facstaff.unca.edu/mcmcclur/GoogleMaps/EncodePolyline/
- Marcelo's Maps API Experiments: http://maps.forum.nu/
- Bill Chadwick's Maps API Demos: http://www.bdcc.co.uk/Gmaps/BdccGmapBits.htm

