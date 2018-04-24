---
layout: post
title: 'Java Servlet 入門 亂記'
author: 'James Peng'
tags: ['Java']
---



## 取得實體路徑 ##


~~~java
// save uploaded files to a 'data' directory in the web app
fileSavePath = getServletContext( ).getRealPath("/") + "data"; 
br.close();
~~~


----------



## 讀取檔案內容 ##

~~~java
InputStreamReader read = new InputStreamReader (new FileInputStream(file),"UTF-8");
BufferedReader br = new BufferedReader(read);

String tempString = "";
while( (tempString = br.readLine())!=null ){
	Content += tempString + "\n";
}
br.close();
~~~


----------

## 資料庫連線 Oracle ##

~~~java
import java.sql.*;

Connection conn = null;
Statement stmt = null;
ResultSet rs = null;
ResultSetMetaData rsm = null;

try{
   
    //load the database driver
    Class.forName ("oracle.jdbc.driver.OracleDriver");

    //The JDBC URL for this Oracle database
    String url = "jdbc:oracle:thin:@192.168.0.2:1521:ORCL";

    //Create the java.sql.Connection to the database, using the
    //correct username and password
    conn = DriverManager.getConnection(url,"scott", "tiger");

    //Create a statement for executing some SQL
    stmt = conn.createStatement( );

    //Execute the SQL statement
    rs = stmt.executeQuery(sql);
   
    //Get info about the return value in the form of
    //a ResultSetMetaData object
    rsm = rs.getMetaData( );

    int colCount =  rsm.getColumnCount( );
   
    //print column names in table header cells
    for (int i = 1; i <=colCount; ++i){
 	  
 	   out.println("<th>" + rsm.getColumnName(i) + "</th>");

    }
   
    out.println("</tr>");
 
    while( rs.next( )){
 	  
 		out.println("<tr>");
 	  
 	   //print the values for each column
 	   for (int i = 1;  i <=colCount; ++i)
 		   out.println("<td>" + rs.getString(i) + "</td>");
 	  
 		out.println("</tr>");

 	   }

} catch (Exception e){
   
    throw new ServletException(e.getMessage( ));

} finally {
   
    try{
 	  
 	   //this will close any associated ResultSets
 	   if(stmt != null)
 		   stmt.close( );

 	   if (conn != null)
 		   conn.close( );

    } catch (SQLException sqle){ }
   
}//finally
~~~


----------

## SQL- INSERT、UPDATE、DELETE 適用 ##

~~~java
PreparedStatement stmt = connection.prepareStatement("insert into discuss_post values "
 				   + " ( null,?,?,?,null,?,null) ");
//數字 對照第幾個 ?
stmt.setString(1,userID);
stmt.setString(2,title);
stmt.setString(3,getDate());
stmt.setString(4,disID);
stmt.executeUpdate();

stmt.close();
~~~


----------

## Redirect ##

forward方式：

~~~java
request.getRequestDispatcher("/somePage.jsp").forward(request, response);
~~~

redirect方式：

~~~java
response.sendRedirect("/somePage.jsp");
~~~


----------

## 取得 servlet 名稱 ##

~~~java
String name = getServletName();
~~~

※ 此 servlet name 為 web.xml 中設定的 

~~~xml
<servlet-name>
<servlet>
    <servlet-name>indexpage</servlet-name>
    <servlet-class>tw.edu.cyu.csie.d520.yichang.index</servlet-class>
</servlet>
~~~
在 index.java 中 getServletName() 會取得 indexpage


----------

## 取得 request ##

~~~java
String ID = req.getParameter("id");
String Password = req.getParameter("password");
~~~

----------

## Session 處理 ##

~~~java
public void HttpSession.setAttribute(String name, Object value)
public Object HttpSession.getAttribute(String name)
~~~
--
把 sessionID 寫上 URL

~~~java
res.sendRedirect(res.encodeRedirectURL("/servlet/NewServlet"));
~~~

----------

## 取得日期 ##

~~~java
import java.util.Calendar;
import java.text.SimpleDateFormat;

 	   SimpleDateFormat df2 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
 	   Calendar cal = Calendar.getInstance();
 	   return df2.format(cal.getTime());
~~~

顯示：　2007-10-12 00:36:08

----------

## 編碼修正 ##

~~~java
Request.setCharacterEncoding("UTF-8");
Response.setContentType("text/html;charset=UTF-8");
~~~

----------

