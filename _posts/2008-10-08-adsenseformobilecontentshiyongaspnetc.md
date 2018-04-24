---
layout: post
title: 'AdSense for Mobile Content使用ASP.Net C#'
author: 'James Peng'
tags: ['ASP.NET']
---

mobile版沒有支援C\#\~Asp.net!!!

[![image](http://lh5.ggpht.com/pompom.new/SOxmFgxOCLI/AAAAAAAAFYk/2q0hiOtLkc0/image_thumb%5B5%5D.png?imgmax=800 "image")](http://lh5.ggpht.com/pompom.new/SOxmEuomTNI/AAAAAAAAFYg/t4IKbJDloUY/s1600-h/image%5B4%5D.png) 

解決方案：

下載：AdSenseHelper.cs

```
/********************************************************************** * AdSenseHelper Class * Version: 1.0 * Release date: 19/09/2007 * Copyright (c) 2007 by Alberto Falossi *  * Web & Blog: http://www.albertofalossi.com *  * This component is 100% free to use, also in commercial applications. * This software is provided "AS IS," without a warranty of any kind. ***********************************************************************************/using System;using System.Text;using System.Collections.Generic;using System.IO;using System.Net;using System.Web;/// <summary>/// Helper class to build AdSense banners./// </summary>public static class AdSenseHelper{    /// <summary>    /// Retrieves the markup for the AdSense banner.    /// </summary>    /// <param name="parameters">Advertisement parameters.</param>    /// <returns>The raw markup to insert into the page.</returns>    public static string GetAdMarkup(Dictionary<string, string> parameters)    {        // build the URL        string url = BuildAdSenseQueryUrl(parameters);        // get and return the response markup        return GetUrl(url);    }    /// <summary>    /// Builds a query URL for the AdSense Server, given the parameters for an advertisement.    /// </summary>    /// <param name="parameters">Advertisement parameters.</param>    /// <returns>The URL to send to the AD Server.</returns>    public static string BuildAdSenseQueryUrl(Dictionary<string, string> parameters)    {        HttpRequest request = HttpContext.Current.Request;        const string URL_ADSERVER = "http://pagead2.googlesyndication.com/pagead/ads?&";        // prepare some parameters        long googleDt = (long)DateTime.Now.Subtract(new DateTime(1970, 1, 1)).TotalMilliseconds;        string googleScheme = request.IsSecureConnection ? "https://" : "http://";        string googleHost = googleScheme + request.ServerVariables["HTTP_HOST"];        StringBuilder url = new StringBuilder(URL_ADSERVER);        // set "automatic" parameters        AddDefaultParameter(parameters, "ad_type", "text");        AddDefaultParameter(parameters, "dt", googleDt.ToString());        AddDefaultParameter(parameters, "host", HttpUtility.UrlEncode(googleHost));        AddDefaultParameter(parameters, "ip", request.UserHostAddress);        AddDefaultParameter(parameters, "ref", request.UrlReferrer == null ? "" : HttpUtility.UrlEncode(request.UrlReferrer.ToString()));        AddDefaultParameter(parameters, "url", HttpUtility.UrlEncode(request.RawUrl));        AddDefaultParameter(parameters, "useragent", HttpUtility.UrlEncode(request.UserAgent));        // add the parameters to the url        foreach (System.Collections.Generic.KeyValuePair<string, string> pair in parameters)        {            string parameterName = pair.Key;            string parameterValue = pair.Value;            // encode the color values            if (parameterName.Contains("color"))                parameterValue = GetGoogleColor(parameterValue, googleDt);            // add the parameter            url.AppendFormat("{0}={1}&", parameterName, parameterValue);        }        return url.ToString();    }    /// <summary>    /// Add a parameter to the parameters dictionary only if the parameter isn't already present.    /// </summary>    /// <param name="parameters">Parameters dictionary.</param>    /// <param name="parameterName">Parameter name.</param>    /// <param name="parameterDefaultValue">Parameter value.</param>    private static void AddDefaultParameter(Dictionary<string, string> parameters, string parameterName, string parameterDefaultValue)    {        // set the value only if the key is missing        if (!parameters.ContainsKey(parameterName))            parameters[parameterName] = parameterDefaultValue;    }    /// <summary>    /// Get a color according to Google algorithm    /// </summary>    /// <param name="value"></param>    /// <param name="random"></param>    /// <returns></returns>    private static string GetGoogleColor(string value, long random)    {        string[] colorArray = value.Split(',');        return colorArray[(int)(random % colorArray.Length)];    }    /// <summary>    /// Send a Web request and returns the server response markup.    /// </summary>    /// <param name="url">Request URL.</param>    /// <returns>A string containing the server response markup. Empty string if an error occurred.</returns>    private static string GetUrl(string url)    {        Stream sream = null;        StreamReader reader = null;        try        {            // build the request            System.Net.WebRequest request = WebRequest.Create(url);            // get the response stream            sream = request.GetResponse().GetResponseStream();            // read the stream and copy it to a string; return the string            reader = new StreamReader(sream);            return reader.ReadToEnd();        }        catch        {            // generic error, return an empty string            return "";        }        finally        {            if (reader != null)                reader.Close();            if (sream != null)                sream.Close();        }    }}
```

  
  
  
  

請參考你自己的php版程式碼內的廣告代號：

  
  
  

[![image.axd](http://lh5.ggpht.com/pompom.new/SOxmHEsIaiI/AAAAAAAAFYs/xIXpd4u3S30/image.axd_thumb%5B1%5D.gif?imgmax=800 "image.axd")](http://lh4.ggpht.com/pompom.new/SOxmGVKuPyI/AAAAAAAAFYo/39vrJ7zrRRE/s1600-h/image.axd%5B3%5D.gif)

  
  

然後在你要插入廣告的地方，輸入以下程式碼：  
  
（廣告代號要改成你自己的）

  
  
  
  

``` csharp
 // ad parameters        Dictionary<string, string> parameters = new Dictionary<string, string>();        parameters["channel"] = "6653xxxxx5";        parameters["client"] = "pub-33xxxxx257202040";        parameters["format"] = "mobile_single";        parameters["color_border"] = "FFFFFF";        parameters["color_bg"] = "FFFFFF";        parameters["color_link"] = "0000CC";        parameters["color_text"] = "000000";        parameters["color_url"] = "008000";        // set the markup language according to the current user browser        string language = "xhtml";        parameters["markup"] = language;        parameters["output"] = language;        // retrieve the markup        string markup = AdSenseHelper.GetAdMarkup(parameters);        // fill the Literal with the markup        Response.Write(markup);
```

  
  

 

  
  

搞定：

  
  

[![image](http://lh6.ggpht.com/pompom.new/SOxmIhm3tuI/AAAAAAAAFY0/l0BLBEZhamM/image_thumb%5B27%5D.png?imgmax=800 "image")](http://lh3.ggpht.com/pompom.new/SOxmH2iTuUI/AAAAAAAAFYw/gJCthupyXpc/s1600-h/image%5B21%5D.png)

  
  

參考：<http://www.albertofalossi.com/page/Using-AdSense-for-Mobile-in-ASP-NET.aspx#download>

