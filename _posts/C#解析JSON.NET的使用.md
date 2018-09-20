---
layout:     post
title:      快速搭建个人博客
subtitle:   手把手教你在半小时内搭建自己的个人博客(如果不踩坑的话🙈🙊🙉)
date:       2017-02-06
author:     BY
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
tags:
    - Blog
---
#【C#】解析C#中JSON.NET的使用
##1. JSON.NET的简介
*JSON.NET对于.NET来说是一个非常高效的JSON框架<br>
*使用JSON.NET可以很方便的实现.NET对象和JSON对象之间的转化.<br/>
*Linq to JSON可以手动读写JSON对象.<br/>
*高性能.<br/>
*可以使用JSON格式的数据读写XML文件<br/>
*支持.NET2、.NET3.5、.NET4、.NET4.5、Silverlight、Windows Phone 、Windows 8 Store<br/>
_在使用JSON.NET之前应该先引入JSON相应的EXE/DLL模块，比如Newtonsoft.Json文件：_

##2. Serializing and Deserializing JSON(序列化和反序列化JSON)
_使用JSONSerializer可以非常方便的实现.NET对象与Json数据之间的转化，JSONSerializer会把.NET对象的属性名转化为Json数据中的Key，把对象的属性值转化为Json数据中的value。_
###2.1  JsonConvert

//Convert to Json 
Product product = new Product();
 product.Name = "Apple";
 product.ExpiryDate = new DateTime(2008, 12, 28);
 product.Price = 3.99M;
 product.Sizes = new string[] { "Small", "Medium", "Large" };
 
 string output = JsonConvert.SerializeObject(product);
//{
//  "Name": "Apple",
//  "ExpiryDate": "2008-12-28T00:00:00",
//  "Price": 3.99,
//  "Sizes": [
//    "Small",
//    "Medium",
//    "Large"
//  ]
//}

//Convert to Object
Product deserializedProduct = JsonConvert.DeserializeObject<Product>(output);

###2.2 JsonSerializer
_JsonSerializer可以直接通过流的方式来操作JSON数据。_

_将对象转化为JSON格式的字符串，然后存储到本地：_
Product product = new Product();
product.ExpiryDate = new DateTime(2008, 12, 28);

JsonSerializer serializer = new JsonSerializer();
serializer.Converters.Add(new JavaScriptDateTimeConverter());//指定转化日期的格式
serializer.NullValueHandling = NullValueHandling.Ignore;//忽略空值

using (StreamWriter sw = new StreamWriter(@"d:\json.txt"))
using (JsonWriter writer = new JsonTextWriter(sw))
{
   serializer.Serialize(writer, product);
    // {"ExpiryDate":new Date(1230375600000),"Price":0}
}

*将本地文件中的Json格式数据，转化为JObject对象：*
JsonSerializer serializer = new JsonSerializer();
serializer.Converters.Add(new JavaScriptDateTimeConverter());//指定转化日期的格式
serializer.NullValueHandling = NullValueHandling.Ignore;//忽略空值

using (StreamReader sr = new StreamReader(@"d:\json.txt"))
using (JsonReader reader= new JsonTextReader(sr))
{
    JObject jo =(JObject) serializer.Deserialize(reader);
//    {
//  "Name": null,
//  "ExpiryDate": "2008-12-28T00:00:00",
//  "Price": 0.0,
//  "Sizes": null
//}
}
_案例中的 serializer.NullValueHandling = NullValueHandling.Ignore 表示忽略空值，也就是为null值的属性不转化，需要注意Decimal的默认值不是null，而是0。_

##3.LINQ to JSON
_Linq to Json可以非常快速的从JObject对象中查询数据，以及创建JObject对象。_

// create JObject
JObject o = JObject.Parse(@"{
   'CPU': 'Intel',
   'Drives': [
     'DVD read/writer',
     '500 gigabyte hard drive'
   ]
 }");

 // query JObject
 string cpu = (string)o["CPU"];
// Intel

string firstDrive = (string)o["Drives"][0];
// DVD read/writer

IList<string> allDrives = o["Drives"].Select(t => (string)t).ToList();
// DVD read/writer
// 500 gigabyte hard drive
##4.Converting XML(XML转化)
###4.1 Convert JSON to XML

string json = @"{
   '@Id': 1,
   'Email': 'james@example.com',
  'Active': true,
   'CreatedDate': '2013-01-20T00:00:00Z',
  'Roles': [
     'User',
     'Admin'
  ],
  'Team': {
   '@Id': 2,
    'Name': 'Software Developers',
    'Description': 'Creators of fine software products and services.'
  }
}";

XNode node = JsonConvert.DeserializeXNode(json, "Root");
//<Root Id="1">
//  <Email>james@example.com</Email>
//  <Active>true</Active>
//  <CreatedDate>2013-01-20T00:00:00Z</CreatedDate>
//  <Roles>User</Roles>
//  <Roles>Admin</Roles>
//  <Team Id="2">
//    <Name>Software Developers</Name>
//    <Description>Creators of fine software products and services.</Description>
//  </Team>
//</Root>

###4.2 Convert XML to JSON

string xml = @"<?xml version='1.0' standalone='no'?>
 <root>
   <person id='1'>
   <name>Alan</name>
   <url>http://www.google.com</url>
   </person>
   <person id='2'>
   <name>Louis</name>
   <url>http://www.yahoo.com</url>
  </person>
</root>";

XmlDocument doc = new XmlDocument();
doc.LoadXml(xml);

string json = JsonConvert.SerializeXmlNode(doc);

Console.WriteLine(json);
// {
//   "?xml": {
//     "@version": "1.0",
//     "@standalone": "no"
//   },
//   "root": {
//     "person": [
//       {
//         "@id": "1",
//         "name": "Alan",
//         "url": "http://www.google.com"
//       },
//       {
//         "@id": "2",
//         "name": "Louis",
//         "url": "http://www.yahoo.com"
//       }
//     ]
//   }
// }
