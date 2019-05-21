---
layout:     post
title:      MySQL基础（二）—— select（1）
subtitle:   MySQL
date:       2019-05-19
author:     YiMiTuMi
header-img: 
catalog: true
tags:
    - MySQL
---
# MySQL基础（二）—— select（1）

注：文章中关键字都是大写写，字段名、表名都是首字母大写。

MySQL数据库中我们最常用的就是查询语句（DQL）。

查询语句关键字：**SELECT**，**FROM**，**WHERE** 是查询语句常用的关键字。

## 查询语句基本写法


	SELECT Field FROM TableName；（其中如果存在多个字段中间用逗号隔开。）


## 查询所有字段


	SELECT * FROM TableName；（可读性差，效率低）

字段上可以使用数学表达式（带SELECT的语句不会修改表中数据，只能查询）。

## 数据字段进行显示改名


	SELECT Field AS NewField FROM TableName；（只是显示的时候改名，字段改名依旧可以用逗号实现多字段改名，注意不会修改底层数据）

改名可以省略as用空格代替，不能使用逗号代替空格，中文要加单引号。

注意：在SQL中没有双引号（只有MySQL可以使用双引号）。

##  根据条件查询

	SELECT Field FROM TableName WHERE condition；
	
## WHERE常用条件

在where后面跟的是一个条件，**WHERE** 是用来过滤数据的，过滤数据就要根据某种条件进行，想当与 **IF** 的判断语句，在SQL中也有很多常用的条件判断。


* `between...and... `两值之间，同 `>= and <= `。

* `is null` 为空，`is not null `不为空。

* `not `取非 。

* `<>`相当与`!=`。

* `and`并且，`or`或者。其中 **and** 的优先级高，同时出现的时候必须加括号。

* `in()`是一个区间，`not in()`不是一个区间。
 
在MySQL的查询语句中，首先执行的是 **FROM**（选中表），然后执行 **WHERE**（使用条件进行过滤），最后执行 **SELECT** 进行字段的查询。

## 未完待续...

[MySQL基础（二）-select(2)模糊查询排序数据](http://yimitumi.com/2019/05/21/MySQL基础-二-select-2-模糊查询排序数据/)

##  山茶花（白）--你怎能轻视我的爱情


