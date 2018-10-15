---
layout:     post
title:      String,toString(),String.valueOf()
subtitle:   String(),toString(),String.valueOf()的区别
date:       2018-10-15
author:     XiongXz
header-img: 
catalog: true
tags:
    - java
---

# String(),toString(),String.valueOf()的区别
## 1、String

_String进行转换的时候，如果类型不匹配会抛出异常。因此在转化的时候如果不确定该类型是否为String类型，需要进行类型判断。_

```
public static void main(String[] args) {
		Object num = 3.2d;
		String str = (String) num;	
		System.out.println(str);
	}
```
_以上程序运行会抛出异常：_

```
Exception in thread "main" java.lang.ClassCastException: java.lang.Double cannot be cast to java.lang.String
	at com.king.jcy.mianshi.Test1.main(Test1.java:8)

```
### 1、1正确的编码方式

```
public static void main(String[] args) {
		String str;
		Object num = 3.2d;
		if(num instanceof String){
			str = (String) num;
		}
		str = "default";
		System.out.println(str);
	}

```

## 2、toString()
![](https://ws1.sinaimg.cn/large/006tNbRwly1fw8rbwzfqej30st01hdfn.jpg)

_在这种使用方法中，因为java.lang.Object类里已有public方法.toString()，所以对任何严格意义上的java对象都可以调用此方法。但在使用时要注意，必须保证object不是null值，否则将抛出NullPointerException异常。采用这种方法时，通常派生类会覆盖Object里的toString（）方法。_

```
public static void main(String[] args) {
		Object obj = null;
		System.out.println(obj.toString());
	}
```
_以上程序运行会抛出异常：_

```
Exception in thread "main" java.lang.NullPointerException
	at com.king.jcy.mianshi.Test1.main(Test1.java:8)
```
_但是toString并不会关注类型的转换，toString方法是返回该对象的字符串，如下代码。_

```
public static void main(String[] args) {
		Object num1 = 1l;
		Object num2 = 3.2D;
		System.out.println("num1 = " + num1.toString());
		System.out.println("num2 = " + num2.toString());
	}

//执行结果
num1 = 1
num2 = 3.2
```

## 3、String.valueOf()

_直接先看源码：_

```
public static String valueOf(Object obj){
    return (obj==null) ? "null" : obj.toString()
};
```
源码可以看出，String.valueOf()方法会将非空的对象直接调用其toString方法。_但是<font color="red">为空的情况下会返回<b>"null"</b>而不是<strong>null</strong></font>_,此处一定注意。

```
public static void main(String[] args) {
		Object obj = null;
		if(null == String.valueOf(obj)){
			System.out.println("obj 为 null");
		}else if("null".equals(String.valueOf(obj))){
			System.out.println("obj 为 null字符串");
		}
	}
	
//执行结果：
obj 为 null字符串
```








