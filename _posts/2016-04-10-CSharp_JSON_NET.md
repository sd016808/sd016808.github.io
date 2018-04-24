---
layout: post
title: 'C# 使用 Json.Net 操作 Json'
author: 'James Peng'
tags: ['C# 3rd-Party Library']
---

## 安裝下載 Json.Net ##

官網: [http://json.codeplex.com/](http://json.codeplex.com/)

說明文件: [http://james.newtonking.com/projects/json/help/](http://james.newtonking.com/projects/json/help/)

## 範例1: 透過 JsonConvert 轉換 自訂類別 ##

- 先準備一個類別

~~~csharp

    public class Person
    {
         public int id;
         public string name;
         public DateTime today;
    }
~~~

~~~csharp
            Person p1 = new Person()
            {
                id = 1,
                name = "James",
                today = DateTime.Today
            };

~~~

----------

- 序列化物件

~~~csharp
            string jsonString = Newtonsoft.Json.JsonConvert.SerializeObject(p1);
            MessageBox.Show(jsonString);
~~~

![](..\images\2016-04-10-CSharp_JSON_NET\sizryKT.png)


加上 Newtonsoft.Json.Formatting.Indented  多了換行

~~~csharp
            string jsonString = Newtonsoft.Json.JsonConvert.SerializeObject(p1, Newtonsoft.Json.Formatting.Indented);
            MessageBox.Show(jsonString);
~~~

![](..\images\2016-04-10-CSharp_JSON_NET\yXCUo4u.png)

----------

- 反序列化物件

~~~csharp
Person p2 = JsonConvert.DeserializeObject<Person>(jsonString);
~~~

![](..\images\2016-04-10-CSharp_JSON_NET\WzT1Flw.png)



----------

## 範例2: 透過 JsonConvert 轉換 DataTable類別 ##

從 SQLite 裡 撈出 DataTable 轉成 Json

![](..\images\2016-04-10-CSharp_JSON_NET\jzbh8q4.png)

~~~csharp

            SqlOperator.OpenDb("Test.db");

            SqlOperator.Execute(@"
                                    BEGIN TRANSACTION;
                                    CREATE TABLE GPIB (
                                    [id]  INTEGER PRIMARY KEY,
                                    [idn]  VARCHAR(255),
                                    [type]  VARCHAR(255),
                                    [action]  VARCHAR(255),
                                    [command]  VARCHAR(255)
                                    );

                                    INSERT INTO `GPIB` (idn,type,action,command) VALUES ('KEITHLEY INSTRUMENTS INC.,MODEL 2000','Multimeter','Get Devive Name','*IDN?');
                                    INSERT INTO `GPIB` (idn,type,action,command) VALUES ('KEITHLEY INSTRUMENTS INC.,MODEL 2000','Multimeter','Reset','*RST');
                                    INSERT INTO `GPIB` (idn,type,action,command) VALUES ('KEITHLEY INSTRUMENTS INC.,MODEL 2000','Multimeter','Get Ac or Dc',':FUNCtion?');
                                    INSERT INTO `GPIB` (idn,type,action,command) VALUES ('KEITHLEY INSTRUMENTS INC.,MODEL 2000','Multimeter','Switch To Ac',':FUNCtion ''VOLTage:AC''');
                                    INSERT INTO `GPIB` (idn,type,action,command) VALUES ('KEITHLEY INSTRUMENTS INC.,MODEL 2000','Multimeter','Switch To Dc',':FUNCtion ''VOLTage:DC''');
                                    INSERT INTO `GPIB` (idn,type,action,command) VALUES ('KEITHLEY INSTRUMENTS INC.,MODEL 2000','Multimeter','Read Value',':READ?');
                                    
                                    INSERT INTO `GPIB` (idn,type,action,command) VALUES ('HEWLETT-PACKARD,34401A','Multimeter','Get Devive Name','*IDN?');
                                    INSERT INTO `GPIB` (idn,type,action,command) VALUES ('HEWLETT-PACKARD,34401A','Multimeter','Reset','*RST');                                    
                                    INSERT INTO `GPIB` (idn,type,action,command) VALUES ('HEWLETT-PACKARD,34401A','Multimeter','Get Ac or Dc',':FUNCtion?');
                                    INSERT INTO `GPIB` (idn,type,action,command) VALUES ('HEWLETT-PACKARD,34401A','Multimeter','Switch To Ac',':FUNCtion ''VOLTage:AC''');
                                    INSERT INTO `GPIB` (idn,type,action,command) VALUES ('HEWLETT-PACKARD,34401A','Multimeter','Switch To Dc',':FUNCtion ''VOLTage:DC''');
                                    INSERT INTO `GPIB` (idn,type,action,command) VALUES ('HEWLETT-PACKARD,34401A','Multimeter','Read Value',':READ?');
                                    COMMIT;
            ");
            DataTable dt = SqlOperator.Select("GPIB", "*" , "`id`<4");
            string jsonString = Newtonsoft.Json.JsonConvert.SerializeObject(dt, Newtonsoft.Json.Formatting.Indented);            
            MessageBox.Show(jsonString);
            System.IO.File.WriteAllText(@"GPIB.json", jsonString);
~~~

![](..\images\2016-04-10-CSharp_JSON_NET\lFvBjB1.png)




----------

讀取 Json 轉回 DataTable

修改過後的 Json 

![](..\images\2016-04-10-CSharp_JSON_NET\VyY44Wg.png)

~~~csharp
            string jsonString = System.IO.File.ReadAllText(@"GPIB.json");
            DataTable dt = Newtonsoft.Json.JsonConvert.DeserializeObject<DataTable>(jsonString);
            dataGridView1.DataSource = dt;
~~~

![](..\images\2016-04-10-CSharp_JSON_NET\tLu6M2u.png)



----------

讀取 Json  轉成Newtonsoft.Json.Linq.JArray 也行

~~~csharp
            string jsonString = System.IO.File.ReadAllText(@"GPIB.json");            
            Newtonsoft.Json.Linq.JArray jArry = Newtonsoft.Json.JsonConvert.DeserializeObject<Newtonsoft.Json.Linq.JArray>(jsonString.Trim());
            dataGridView1.DataSource = jArry;            
~~~

![](..\images\2016-04-10-CSharp_JSON_NET\tLu6M2u.png)


----------

## 範例3: 透過 JsonTextWriter 輸出 Jsong ##

~~~csharp

            string jsonString = string.Empty;

            using (System.IO.StringWriter sw = new System.IO.StringWriter())
            {
                using (Newtonsoft.Json.JsonTextWriter writer = new Newtonsoft.Json.JsonTextWriter(sw))
                {
                    writer.Formatting = Newtonsoft.Json.Formatting.Indented;

                    // 開始輸出物件
                    writer.WriteStartObject();


                    // 輸出屬性:id
                    writer.WritePropertyName("id");
                    writer.WriteValue(1);                    

                    // 輸出屬性:name
                    writer.WritePropertyName("name");
                    writer.WriteValue("xian");

                    // 輸出屬性today
                    writer.WritePropertyName("today");
                    writer.WriteValue(DateTime.Today);

                    // 開始輸出陣列
                    writer.WritePropertyName("arraydata");
                    writer.WriteStartArray();
                    writer.WriteValue(1);
                    writer.WriteValue(2);
                    writer.WriteValue(3);

                    // 結束輸出陣列
                    writer.WriteEndArray();

                    // 結束串出物件
                    writer.WriteEndObject();
                }

                jsonString = sw.ToString();
                MessageBox.Show(jsonString);
                System.IO.File.WriteAllText(@"jsonString.json", jsonString);
            }
~~~


![](..\images\2016-04-10-CSharp_JSON_NET\vq2Z15c.png)

----------



## 範例4: 透過 JsonTextReader 讀取 Jsong ##

~~~csharp
            string jsonString = string.Empty;
            jsonString = System.IO.File.ReadAllText(@"jsonString.json");

            string ReadOutput = string.Empty;

                using (System.IO.StringReader sr = new System.IO.StringReader(jsonString))
                {
                    using (Newtonsoft.Json.JsonTextReader reader = new Newtonsoft.Json.JsonTextReader(sr))
                    {
                        while (reader.Read())
                        {
                            if (reader.Value != null)
                            {
                                ReadOutput += string.Format("token:{0}, value:{1} \n", reader.TokenType, reader.Value);
                            }
                            else
                            {
                                ReadOutput += string.Format("token:{0} \n", reader.TokenType);
                            }
                        }
                    }
                }

                MessageBox.Show(ReadOutput);  
~~~

![](..\images\2016-04-10-CSharp_JSON_NET\6Ck3i3N.png)




----------

參考：

- http://blog.developer.idv.tw/2013/06/jsonnet.html
- http://myprogramlog.blogspot.tw/2013/09/jsonnet-datatablejsonjsondatatable.html
- http://blog.darkthread.net/post-2010-06-05-json-net-jobject-example.aspx
- http://blog.darkthread.net/post-2012-06-09-json-net-performance.aspx
- https://dotblogs.com.tw/shadow/2011/11/30/60083

