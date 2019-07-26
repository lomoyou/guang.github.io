---
layout:     post
title:      QString与C++字符串类型的相互转换
subtitle:   QT
date:       2019-07-26
author:     YiMiTuMi
header-img: 
catalog: true
tags:
    - QT编程
---
# QString与c++字符串类型的相互转换

在C/C++编程中最繁琐的就是个个类型之间的相互转换，最近用到QT也牵扯到QString和字符串之间的类型转换，整理一下转换方法。

const char*

	const char* const_str = "Hello World";

const char * 转 QString

	QString q_str = const_c_str;   

QString 转 char*

	QByteArray ba1 = q_str.toUtf8();
	char* c_str = ba1.data();

QString 转 const char*

	QByteArray ba2 = q_str.toUtf8();
	const char * const_c_str1 = ba2.constData();

QString 转 std::string

	std::string std_str = q_str.toStdString();

QString 转 std::wstring

	std::wstring std_wstr = q_str.toStdWString();

QString 转 const wchar_t *

	const wchar_t* wchar_str = reinterpret_cast<const wchar_t *>(q_str.utf16());

wchar_t* 转 QString

	QString qstr1 = QString::fromWCharArray(wchar_str);

string 转 QString

	QString qstr2 = QString::fromStdString(std_str);

wstring 转 QString

	QString qstr3 = QString::fromStdWString(std_wstr);

## 向日葵--沉默的爱