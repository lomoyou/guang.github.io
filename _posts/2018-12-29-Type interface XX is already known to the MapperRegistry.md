---
layout:     post
title:      Type interface XX is already known to the MapperRegistry
subtitle:   Type interface XX is already known to the MapperRegistry 
date:       2018-12-29
author:     Xionghz
header-img: 
catalog: true
tags:
    - java Mybatis
---

# Mybatis报错:Type interface XX is already known to the MapperRegistry

<b>日志报错提示:</b>

```
Exception in thread "main" java.lang.ExceptionInInitializerError
 at top.xionghz.util.TestUtil.sds(TestUtil.java:24)
 at top.xionghz.util.TestUtil.main(TestUtil.java:17)
Caused by: java.lang.RuntimeException: loading file MybaitsConfig.xml failureorg.apache.ibatis.exceptions.PersistenceException: 
### Error building SqlSession.
### The error may exist in top/xionghz/mapper/IMapper.java (best guess)
### Cause: org.apache.ibatis.builder.BuilderException: Error parsing SQL Mapper Configuration. Cause: org.apache.ibatis.binding.BindingException: Type interface top.xionghz.mapper.ShopCaseMapper is already known to the MapperRegistry.
 at top.xionghz.helper.MybatisHelper.createSqlSessionFactory(MybatisHelper.java:61)
 at top.xionghz.helper.MybatisHelper.<clinit>(MybatisHelper.java:44)
 ... 2 more
```

<b>错误位置:</b><br/>

```
<mappers>
    <mapper resource="StoW/ShopCaseMapper.xml"/>
    <package name="top.xionghz.mapper"/>
</mappers>
```

由于.xml映射文件的 namespace 的ID是唯一的，并且只能注册一次。上述<mappers>标签里加载两次Mapper。所以导致报错。