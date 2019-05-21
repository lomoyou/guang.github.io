---
layout:     post
title:      MySQL基础（二）—— select（2）模糊查询
subtitle:   MySQL
date:       2019-05-21
author:     YiMiTuMi
header-img: 
catalog: true
tags:
    - MySQL
---
# MySQL基础（二）—— select（2）模糊查询/排序数据

注：文章中关键字都是大写，字段名、表名都是首字母大写。

在很多时候我们在查询数据中我们不知道具体数据到底是什么我们可能只记得数据的一部分内容，所以我们就不能进行精确查询，只能根据所知道的关键字进行模糊查询，模糊查询所用到的关键字为 **LIKE**。

## 模糊查询语句

查询字段中带有零的数据：

	SELECT Field FROM TableName WHERE Field LIKE '%0%';

其中 **%** 为占位符，代表0~n个任意字符。如：

查询第一个字符为s的数据：

	SELECT Field FROM TableName WHERE Field LIKE 's%';

查询最后一个字符为s的数据：

	SELECT Field FROM TableName WHERE Field LIKE '%s';

查询第二个字符为s的数据：

	SELECT Field FROM TableName WHERE Field LIKE '_s%';

其中下划线代表任意一个字符。

## 排序数据

排序数据我们通过关键字 **ORDER BY**来进行排序。

	SELECT Field FROM TableName ORDER BY Condition ASC；升序
	SELECT Field FROM TableName ORDER BY Condition DESC；降序

在MySQL中ASC（ascend）和DESC（descend）分别是升序和降序的意思。注意如果不加关键字，默认升序排序。

多字段排序：

	SELECT Field FROM TableName ORDER BY Condition (ASC，DESC)，Condition (ASC,DESC);


条件越靠前越主要，优先级越高。

使用字段位置进行排序：

	SELECT Field FROM TableName ORDER BY Condition；

Condition接受数字，即字段所在位置的数字。（尽量不使用）

## 未完待续...

##  蓝花楹--在绝望中等待爱情
