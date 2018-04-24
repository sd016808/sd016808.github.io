---
layout: post
title: 'Java Servlet 入門 HelloServlet'
author: 'James Peng'
tags: ['Java']
---

## 測試環境 ##
Java SDK 6 update 2
tomcat-6.0.14


----------

## 執行步驟 ##

1. Servlet 編寫
2. web.xml 編寫
3. 執行確認


----------

## 步驟 1. ##

Servlet 程式編譯後，放置於 [context目錄]\WEB-INF\classes\ 底下

便於測試

1. 直接將 HelloPOMPOM.java 放置於 [context目錄]\WEB-INFO\classes\ 底下
2. 將 Tomcat\lib\servlet-api.jar 也複製過來 (或者設系統環境變數)

###  HelloPOMPOM.java 內容 ###

~~~java
package tw.edu.cyu.d520.yichang;
 
import java.io.*; 
import javax.servlet.*; 
import javax.servlet.http.*; 
 
public class HelloPOMPOM extends HttpServlet { 
    public void doGet(HttpServletRequest req, 
                      HttpServletResponse res) 
                  throws ServletException, IOException { 
        res.setContentType("text/html"); 
        PrintWriter out = res.getWriter(); 
 
        out.println("<html>"); 
        out.println("<head>");
        out.println("<title>Hello!POMPOM!</title>");
        out.println("</head>"); 
        out.println("<body>"); 
        out.println("<h1><b>Hello!POMPOM!</b></h1>"); 
        out.println("Now You See The Servlet , HelloPOMPOM"); 
        out.println("</body>"); 
        out.println("</html>"); 
    } 
}
~~~


----------


###  Servlet 編譯指令  ###

    javac -classpath servlet-api.jar -d . HelloPOMPOM.java


###  簡述  ###

Servlet 程式要求使用 package 做程式管理 ( like .NET namespace )

指令" -d . "為編譯自行照 package 配置目錄

----------


## 步驟 2. ##

web.xml 放置於 [context目錄]\WEB-INF\ 底下

###  web.xml 內容 ###

~~~xml
<?xml version="1.0" encoding="ISO-8859-1"?> 
 
<web-app xmlns="http://java.sun.com/xml/ns/j2ee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd"
	version="2.5"> 
 
    <servlet> 
        <servlet-name>helloPOM</servlet-name> 
        <servlet-class>tw.edu.cyu.d520.yichang.HelloPOMPOM</servlet-class> 
    </servlet>
 
    <servlet-mapping> 
        <servlet-name>helloPOM</servlet-name> 
        <url-pattern>/helloPOMPOM</url-pattern> 
    </servlet-mapping> 
  
</web-app>

~~~


###  注意  ###

    xmlns:xsi="xxx" xsi:yy="xx" 為單行(不換行)
    
    <servlet> 配置 servlet名稱 對映的 class
    
    <servlet-mapping> 配置 servlet名稱 對映的 客戶端顯示路徑/名稱

-------------------------------

## 步驟 3. ##


啟動 Tomcat ，開起瀏覽器連線確認

### 瀏覽器顯示內容  ###

~~~text
Hello!POMPOM!
Now You See The Servlet , HelloPOMPOM
~~~

###  注意  ###

    如自訂外部 [context目錄]
    
    每當重新編譯程式、修改web.xml
    
    Tomcat必須重新啟動，才會載入新的 class 及配置

