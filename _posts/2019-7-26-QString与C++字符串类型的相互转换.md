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
	
BYTE* 转 QString

	void BYTEtoQString(BYTE* byStr, QString &strBy)
	{
	    for (int i = 0; i < 16; i++)
	    {
		char pBuff[2];
		sprintf(pBuff, "%02x", byStr[i]);
		strBy = strBy + pBuff;
	    }
	}
	
TCHAR *类型转为QString类型：

	QString WcharToChar(const TCHAR* wp, size_t codePage = CP_ACP)
	{
	QString str;
	int len = WideCharToMultiByte(codePage, 0, wp, wcslen(wp), NULL, 0, NULL, NULL);
	char *p = new char[len + 1];
	memset(p, 0, len + 1);
	WideCharToMultiByte(codePage, 0, wp, wcslen(wp), p, len, NULL, NULL);
	p[len] = '\0';
	str = QString(p);
	delete p;
	p = NULL;
	return str;
	}
	
QString类型转为TCHAR *类型

	TCHAR *CharToWchar(const QString &str)
	{
	QByteArray ba = str.toUtf8();
	char *data = ba.data(); //以上两步不能直接简化为“char *data = str.toUtf8().data();”
	int charLen = strlen(data);
	int len = MultiByteToWideChar(CP_ACP, 0, data, charLen, NULL, 0);
	TCHAR *buf = new TCHAR[len + 1];
	MultiByteToWideChar(CP_ACP, 0, data, charLen, buf, len);
	buf[len] = '\0';
	return buf;
	}

最后在使用完后应将内存释放：

	QString str("你好");
	TCHAR *data = CharToWchar(str);
	......
	delete data;
	data = NULL;


## 向日葵--沉默的爱
