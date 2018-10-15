---
layout:     post
title:      java-Field类
subtitle:   java.lang.reflect.Field详解
date:       2018-10-15
author:     XiongXz
header-img: 
catalog: true
tags:
    - java IOC
---

# java.lang.reflect.Field详解
## 1、Feild是什么
_Field是一个类,位于java.lang.reflect包下。<br>
在Java反射中 Field类描述的是 类的属性信息,通俗来讲 有一个类如下：_

```
package com.testReflect;
public class FieldDemo {
    public int num1 = 1;
    protected int num2 = 2;
    int num3 = 3;
    private int num4 = 4;
    
    public String s1 = "a";
    protected String s2 = "b";
    String s3 = "c";
    private String s4 = "d";
}

```
_在Java反射中FieldDemo类中的属性: num1、num2、num3、num4 都是Field类的实例，这个Field类的实例描述了属性的全部信息。（包括：属性名称、属性类型、属性修饰符、属性注解 等等_

## 2、如何获取Field类对象
_一共有4种方法,全部都在Class类中:<br>_

* getFields(): 获取类中public类型的属性
* getField(String name)： 获取类特定的方法，name参数指定了属性的名称
* getDeclaredFields(): 获取类中所有的属性(public、protected、default、private),但不包	括继承的属性。
* getDeclaredField(String name): 获取类特定的方法，name参数指定了属性的名称

## 3、Field类中常用的方法
_对于类中的属性，我们常用的操作：<br>_

```
package com.testReflect;

import java.lang.reflect.Field;
import java.lang.reflect.Modifier;

public class FieldTest {
    public static void main(String[] args) throws Exception {
        //使用反射第一步:获取操作类FieldDemo所对应的Class对象
        Class<?> cls = Class.forName("com.testReflect.FieldDemo");
        //使用FieldDemo类的class对象生成 实例
        Object obj = cls.newInstance();
                
        //通过Class类中getField(String name)：获取类特定的方法，name参数指定了属性的名称
        Field field = cls.getField("num1");        

        //拿到了Field类的实例后就可以调用其中的方法了
        
        //方法:getModifiers()  以整数形式返回由此 Field 对象表示的字段的 Java 语言修饰符
        System.out.println("修饰符:  " +Modifier.toString(field.getModifiers()));

        //方法:getType()  返回一个 Class 对象，它标识了此 Field 对象所表示字段的声明类型
        System.out.println("类型:  "+field.getType());
        
        //方法:get(Object obj) 返回指定对象obj上此 Field 表示的字段的值
        System.out.println("属性值:  "+field.get(obj));
        
        //方法: set(Object obj, Object value)  将指定对象变量上此 Field 对象表示的字段设置为指定的新值
        field.set(obj, 55);
        System.out.println("修改属性值后 --> get(Object obj):  "+field.get(obj));
    }
}
```