---
layout:     post
title:      C#-MybatisNet配置
subtitle:   MybatisNet配置文件详解
date:       2018-10-09
author:     XiongXz
header-img: 
catalog: true
tags:
    - C#
---

# MybatisNet配置

### 简介
_iBatis.Net是移植自java的一个持久性框架，在开发中简单易用，且可以灵活修改Sql。_

## 1、下载dll
_到[官网](http://code.google.com/p/mybatisnet/) 下载相关dll和文档_
--温馨提示需要翻墙

Doc-DataAccess-1.9.2.zip 
Doc-DataMapper-1.6.2.zip 
IBatis.DataAccess.1.9.2.bin.zip 
IBatis.DataMapper.1.6.2.bin.zip

_一共4个.zip,在项目里添加引用_

## 2、添加Providers.config

* 所有的配置文件属性，必须是嵌入式的资源，否则会报异常：‘无法嵌入程序集资源’ 。
* 把从官方下载的压缩包解开，就能找到providers.config文件，里面定义了MyBatis.Net支持的各种数据库驱动，本例以sqlserver为例，把其它不用的db provider全删掉，只保留下sqlServer2008，同时把enabled属性设置成true，参考下面这样：

```
	<?xml version="1.0"?>	<providers xmlns="http://ibatis.apache.org/providers"		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">    	<clear/>   		<!--sqlserver2008数据库驱动配置文件-->  		<provider    		name="sqlServer2008"    		enabled="true"    		default="true"    		description="Microsoft SQL Server, provider V4.0.0.0 in framework .NET 		V4.5"    		assemblyName="System.Data, Version=4.0.0.0, Culture=Neutral, 			PublicKeyToken=b77a5c561934e089"    		connectionClass="System.Data.SqlClient.SqlConnection"    		commandClass="System.Data.SqlClient.SqlCommand"   			parameterClass="System.Data.SqlClient.SqlParameter"    		parameterDbTypeClass="System.Data.SqlDbType"    		parameterDbTypeProperty="SqlDbType"    		dataAdapterClass="System.Data.SqlClient.SqlDataAdapter"    		commandBuilderClass=" System.Data.SqlClient.SqlCommandBuilder"    		usePositionalParameters = "false"    		useParameterPrefixInSql = "true"    		useParameterPrefixInParameter = "true"    		parameterPrefix="@"    		allowMARS="true"    	/>	</providers>

```
-----------------------------

## 3、添加sqlmap.config

_这个文件也复制到Web项目根目录下，它的作用主要是指定db连接串，告诉系统providers.config在哪? 以及db与entity的映射文件在哪?_

```
<?xml version="1.0" encoding="utf-8"?><sqlMapConfig xmlns="http://ibatis.apache.org/dataMapper"			  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  <settings>    <setting useStatementNamespaces="false"/>    <setting cacheModelsEnabled="true"/>  </settings>  <!--db provider配置文件路径-->  <providers resource="Providers.config"/>  <!--db provider类型及连接串-->  <database>    <provider name="sqlServer2008" />    <dataSource name="sqlServer" connectionString="Data Source=**.*.*.**;database=数据库名称;User ID=user;Password=passw0rd />  </database>  <!--db与Entity的映射文件-->  <sqlMaps>    <!-- user via embedded-->    <sqlMap embedded="Mappers.IsegB.xml,Dynamic"/>    <sqlMap embedded="Mappers.Sget_sapB.xml,Dynamic"/>  </sqlMaps></sqlMapConfig>

```
-------------

## 4、XXX.xml文件
_创建Mappers目录，并在该目录下，添加映射文件xxx.xml，内容如下：_

```
<?xml version="1.0" encoding="utf-8" ?><sqlMap namespace="Sget_sap" xmlns="http://ibatis.apache.org/mapping"xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >  <alias>    <typeAlias alias="Sget_sap" type="Dynamic.Sget_sap,Dynamic" />    <!--加载实体类，这里主要指下面要调用的实体类-->    <typeAlias alias="Sgetdet_sap" type="Dynamic.Sgetdet_sap,Dynamic" />    <!--次实体类，唯一string[]类型-->    <typeAlias alias="arrNos" type="Dynamic.arrNos,Dynamic" />      </alias>  <resultMaps>    <!--返回数据解析的实体-->    <!--表头-->    <resultMap id="SelectAllResult" class="Sget_sap">      <result property="Purdate" column="Purdate" />      <result property="Nos" column="Nos" />      <result property="Purcode" column="Purcode" />            <result property="Retpo" column="Retpo" />    </resultMap>    <!--明细-->    <resultMap id="SelectDetResult" class="Sgetdet_sap">      <result property="EBELP" column="EBELP" />      <result property="MATNR" column="MATNR" />      <result property="MENGE" column="MENGE" />      <result property="MEINS" column="MEINS" />      <result property="WERKS" column="WERKS" />      <result property="LGORT" column="LGORT" />      <result property="CHARG" column="CHARG" />    </resultMap>  </resultMaps>  <statements>    <!-- 这里主要写sql语句和存储过程-->    <!--WMS收货订单传SAP接口**表头-->    <select id="SelectSget" resultMap="SelectAllResult">      <![CDATA[select s.purdate as Purdate,s.nos as Nos,s.purcode as Purcode,p.RETPO as Retpo      from sget s,sgetdet d ,ekpo p      where s.purcode=d.purcode and s.nos=p.EBELN      and s.sendflag='N' and s.loadtime is null   ]]>         </select>    <!--WMS收货订单传SAP接口**明细-->    <select id="SelectSgetdet" resultMap="SelectDetResult" parameterClass="String">      <![CDATA[select 		p.EBELP as EBELP		,p.MATNR as MATNR		,d.sqty as MENGE		,p.MEINS as MEINS		,p.WERKS as WERKS		,s.storeno as LGORT		,d.blockno as CHARG		from sget s,sgetdet d ,ekpo p 		where s.purcode=d.purcode and s.nos=p.EBELN 		and s.sendflag='N' and s.loadtime is null		and s.nos=#nos#]]>          </select>        <!--传输成功后，修改表(sget)字段(sendflag)='Y',(loadtime)='当前时间'-->    <update id="UpdateSgetbyNos" parameterClass="arrNos" resultMap="int">      <![CDATA[update sget set sendflag='Y',loadtime=GETDATE() where sendflag='N' and loadtime is null and nos in]]>      <iterate open="(" close=")" conjunction="," property="ArrValue">        #ArrValue[]#      </iterate>    </update>      </statements></sqlMap>

```

## 5、封装mybatisnet
_写一个通用的BaseDA类，对MyBatis.Net做些基本的封装 _

```

using IBatisNet.DataMapper;using System;using System.Collections.Generic;using System.Linq;using System.Text;using System.Threading.Tasks;namespace Dynamic{    public class BaseDao<T> where T : class    {        public BaseDao()        {        }        public ISqlMapper isqlMapper { get; set; }        public IList<T> GetAllList(string key)        {            return isqlMapper.QueryForList<T>(key, null);        }        public IList<T> GetAllListby(string key,object str)        {            return isqlMapper.QueryForList<T>(key, str);        }        public T GetModel(string key, object id)        {            return isqlMapper.QueryForObject<T>(key, id);        }        public object Insert(string key, T model)        {            object o = null;            o = isqlMapper.Insert(key, model);            return o;        }        public void UpdateBy(string key, object arr)        {                           isqlMapper.Update(key, arr);                    }           }}

```

## 6、测试

```

 //查询            BaseDao<Sdeliver> @base = new BaseDao<Sdeliver>()            {                isqlMapper = sqlMapper            };            List<Sap_MM018.DTO_WMS_ERP_MM018SHEET> list_sheet = new List<Sap_MM018.DTO_WMS_ERP_MM018SHEET>();            IList<Sdeliver> sdelivers= @base.GetAllList("SelectDeliver");            foreach (var item in sdelivers)            {                Sap_MM018.DTO_WMS_ERP_MM018SHEET dTO_WMS_ERP_MM018SHEET = new Sap_MM018.DTO_WMS_ERP_MM018SHEET();                dTO_WMS_ERP_MM018SHEET.BUDAT = string.Format("{0:yyyyMMdd}", item.Budate);                dTO_WMS_ERP_MM018SHEET.CHARG = item.CHARG;                dTO_WMS_ERP_MM018SHEET.EBELN = item.EBELN;                dTO_WMS_ERP_MM018SHEET.EBELP = item.EBELP.ToString();                dTO_WMS_ERP_MM018SHEET.LGORT = item.LGORT;                dTO_WMS_ERP_MM018SHEET.MATNR = item.MATNR;                dTO_WMS_ERP_MM018SHEET.MEINS = item.MEINS;                dTO_WMS_ERP_MM018SHEET.MENGE = item.MENGE.ToString();                dTO_WMS_ERP_MM018SHEET.WERKS = item.WERKS;                dTO_WMS_ERP_MM018SHEET.ZWMNO = item.ZWMNO;                list_sheet.Add(dTO_WMS_ERP_MM018SHEET);            }
            
```
  		