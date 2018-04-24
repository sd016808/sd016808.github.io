---
layout: post
title: 'C# 透過Proxy 從URL下載圖片'
author: 'James Peng'
tags: ['C#']
---

## C# 透過Proxy 從URL下載圖片 ##

~~~csharp
        public static Image GetImageFromUrl(string urlImagePath)
        {
            Uri uri = new Uri(urlImagePath);
            WebRequest webRequest = WebRequest.Create(uri);

            WebProxy proxyObj = new WebProxy("http://proxy.company.corp:8080");
            proxyObj.Credentials = CredentialCache.DefaultCredentials;
            webRequest.Proxy = proxyObj;

            using (HttpWebResponse rsp = (HttpWebResponse)webRequest.GetResponse())
            {
                using (Stream stream = rsp.GetResponseStream())
                {
                    Image res = Image.FromStream(stream);
                    return res;
                }    
            }                        
        }
~~~


----------

## C# 透過Proxy 從URL下載圖片到 指定路徑 ##

~~~csharp
        public static void GetImageFromUrl(string url, string path)
        {
            HttpWebRequest request = (HttpWebRequest)WebRequest.Create(url);
 
            request.ServicePoint.Expect100Continue = false;
            request.Method = "GET";
            request.KeepAlive = true;

            request.ContentType = "image/png";
            
            WebProxy proxyObj = new WebProxy("http://proxy.company.corp:8080");
            proxyObj.Credentials = CredentialCache.DefaultCredentials;
            request.Proxy = proxyObj;


            // Send the request to the Internet resource and wait for the response.    
            using (HttpWebResponse rsp = (HttpWebResponse)request.GetResponse())
            {
                System.IO.Stream stream = null;
                try
                {
                    using (stream = rsp.GetResponseStream())
                    {
                        Image.FromStream(stream).Save(path);
                    }                    
                }
                finally
                {
                    if (stream != null) stream.Close();
                    if (rsp != null) rsp.Close();
                }
            }
        }
~~~



