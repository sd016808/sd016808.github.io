---
layout: post
title: 'ASP.NET Web API 實作 Line Bot 使用 Azure App Service '
author: 'James Peng'
tags: ['ASP.NET']
---

## 事前準備 ##

- LINE帳戶
	- [Line Business Center](https://business.line.me/) 免費註冊
	- [LINE BOT API Trial](https://developers.line.me/type-of-accounts/bot-api-trial) 
- Microsoft Azure帳號
	- 請參考[免費方案](https://azure.microsoft.com/zh-tw/free/) 
- Visual Studio 2015
	- [社群免費版](https://www.visualstudio.com/zh-tw/products/visual-studio-community-vs.aspx) 


----------

## 先建立一個 Microsoft Azure Web 應用程式 ##

怎麼註冊我就不提了

[進入到自己的 Azure 首頁](https://portal.azure.com/?whr=live.com)

新增一個 Web 應用程式

![](..\images\2016-05-14-Aspnet_LineBot\QPvRil0.png)

![](..\images\2016-05-14-Aspnet_LineBot\XWCbP0G.png)

----------

## 建立一個 ASP.NET Web API 專案 ##

![](..\images\2016-05-14-Aspnet_LineBot\CSR20k2.png)

選 Empty 空專案，然後勾選 Web API，使用 雲端中的主機

![](..\images\2016-05-14-Aspnet_LineBot\wiipZv3.png)

選擇剛剛建立的 Azure Web 應用程式

![](..\images\2016-05-14-Aspnet_LineBot\nrxoy1Y.png)

按下 建立

----------

## 接收處理訊息的 ASP.NET Web API ##

在 Controllers 裡 加入 一個 控制器

![](..\images\2016-05-14-Aspnet_LineBot\dqYCQ01.png)

選擇 Web API1 控制器 - 空白，按下新增

![](..\images\2016-05-14-Aspnet_LineBot\rNaUTmI.png)

輸入名稱 CallbackController

![](..\images\2016-05-14-Aspnet_LineBot\n42SsvN.png)


程式碼如下：


~~~csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;
using System.Web.Http;
using Newtonsoft.Json;


namespace LineBotApp.Controllers
{

  public class CallbackController : ApiController
  {
      public async Task<HttpResponseMessage> Post()
      {
        var contentString = await Request.Content.ReadAsStringAsync();
 
        dynamic contentObj = JsonConvert.DeserializeObject(contentString);
        var result = contentObj.result[0];

        var client = new HttpClient();
        try
        {
          client.DefaultRequestHeaders
            .Add("X-Line-ChannelID", "輸入 Channel ID");
          client.DefaultRequestHeaders
            .Add("X-Line-ChannelSecret", "輸入 Channel Secret");
          client.DefaultRequestHeaders
            .Add("X-Line-Trusted-User-With-ACL", "輸入 MID");
                
               
          var res = await client.PostAsJsonAsync("https://trialbot-api.line.me/v1/events",
              new {
                to = new[] { result.content.from },
                toChannel = "1383378250",
                eventType = "138311608800106203",
                content = new {
                  contentType = 1,
                  toType = 1,
                  text = $"安安「{result.content.text}」你好"
                }
              });
          
          System.Diagnostics.Debug.WriteLine(await res.Content.ReadAsStringAsync());
          return new HttpResponseMessage(System.Net.HttpStatusCode.OK);
       }
       catch (Exception e)
       {
         System.Diagnostics.Debug.WriteLine(e);
         return new HttpResponseMessage(System.Net.HttpStatusCode.InternalServerError);
       }
    }
  }
   
}
~~~

編譯看看 是否成功～

----------

## 佈署 到 Azure ##

對著專案 按右鍵 選發行

![](..\images\2016-05-14-Aspnet_LineBot\FRZawko.png)

按 發行

![](..\images\2016-05-14-Aspnet_LineBot\fiWGe6L.png)

成功～ 請複製網址

![](..\images\2016-05-14-Aspnet_LineBot\9oZEndQ.png)

例如：

	https://lxnebxoappxxxxxxxxxxxxx.azurewebsites.net

----------

## 設定 LINE API 的 Callback ##

連到 [LINE Developpers ](https://developers.line.me/)

先點 Channels

在按 Edit 編輯

![](..\images\2016-05-14-Aspnet_LineBot\3XGtMAT.png)

把剛剛複製的網址，貼在這

![](..\images\2016-05-14-Aspnet_LineBot\6uc3f6Z.png)

注意：

- LINE 只吃 https
- 網址一定要帶上PORT， HTTPS的PORT是 443
- 「/api/callback」 是我們自己寫的 CallbackController


最後網址會長這樣：

	https://lxnebxoappxxxxxxxxxxxxx.azurewebsites.net:443/api/callback


----------

## 設定 LINE API 的 白名單 ##

記錄下 Azure 的 輸出 IP 位置

![](..\images\2016-05-14-Aspnet_LineBot\69rZNHw.png)

把 IP 輸入到 LINE 的 Server IP Whitelist 

![](..\images\2016-05-14-Aspnet_LineBot\xTYRBPu.png)

LINE 才能和 Azure 溝通 

----------

## 成果 ##

![](..\images\2016-05-14-Aspnet_LineBot\UNE7YbI.jpg)

蠻有趣的

----------

## 怎麼 debug ##

![](..\images\2016-05-14-Aspnet_LineBot\YvnjnNW.png)

剩下就 設斷點 和往常一樣

----------


## 參考 ##

- http://pierre3.hatenablog.com/entry/2016/04/13/234505
