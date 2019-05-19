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

MySQL数据库中我们最常用的就是查询语句（DQL）。

查询语句关键字：**select**，**from**，**where** 是查询语句常用的关键字。

## 查询语句基本写法


	select 字段 from 表名；（其中如果存在多个字段中间用逗号隔开。）


## 查询所有字段


	select * from 表名；（可读性差，效率低）

字段上可以使用数学表达式（带select的语句不会修改表中数据，只能查询）。

## 数据字段进行显示改名


	select 字段 as 新字段名 from 表名；（只是显示的时候改名，字段改名依旧可以用逗号实现多字段改名，注意不会修改底层数据）

改名可以省略as用空格代替，不能使用逗号代替空格，中文要加单引号。

注意：在SQL中没有双引号（只有MySQL可以使用双引号）。

##  根据条件查询

	select 字段 from 表名 where 条件；
 
在MySQL的查询语句中，首先执行的是 **from**（选中表），然后执行 **where**（使用条件进行过滤），最后执行 **select** 进行字段的查询。

##  山茶花（白）————你怎能轻视我的爱情


