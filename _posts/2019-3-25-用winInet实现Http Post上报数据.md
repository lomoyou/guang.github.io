---
layout:     post
title:      用winInet实现Http Post上报数据
subtitle:   网络协议
date:       2019-03-25
author:     YiMiTuMi
header-img: 
catalog: true
tags:
    - 网络协议
---
# 用winInet实现Http Post上报数据

winInet是微软封装的一个使用起来特别复杂的库，具体函数可以在微软官网中查阅。

主要用到的函数：

* [InternetCrackUrl](https://docs.microsoft.com/zh-cn/windows/desktop/api/wininet/nf-wininet-internetcrackurla) ：解析URL

* [InternetOpenW](https://docs.microsoft.com/zh-cn/windows/desktop/api/wininet/nf-wininet-internetopenw) ：初始化应用程序对WinINet函数的使用。

* [InternetConnectW](https://docs.microsoft.com/zh-cn/windows/desktop/api/wininet/nf-wininet-internetconnectw) ：打开给定站点的文件传输协议（FTP）或HTTP会话。

* [HttpOpenRequest](https://docs.microsoft.com/zh-cn/windows/desktop/api/wininet/nf-wininet-httpopenrequestw) ：创建HTTP请求句柄。 

* [HttpSendRequestExW](https://docs.microsoft.com/zh-cn/windows/desktop/api/wininet/nf-wininet-httpsendrequestexw) ：将指定的请求发送到HTTP服务器。

* [InternetWriteFile](https://docs.microsoft.com/zh-cn/windows/desktop/api/wininet/nf-wininet-internetwritefile) ：将数据写入打开的Internet文件。

* [HttpEndRequest](https://docs.microsoft.com/zh-cn/windows/desktop/api/wininet/nf-wininet-httpendrequestw) ：结束由HttpSendRequestEx启动的HTTP请求。

* [InternetCloseHandle](https://docs.microsoft.com/zh-cn/windows/desktop/api/wininet/nf-wininet-internetclosehandle) ：关闭一个Internet句柄。

## 程序如下：

函数接收一个URL，一个要发送的字符串和字符串的长度。

	#include <WinInet.h>
	#pragma comment(lib, "WinInet.lib")

	BOOL HttpReportToBuffer(wstring lpUrl, LPCVOID lpBuffer, DWORD sz)
	{
	    BOOL Judge = TRUE;
	
	    HINTERNET hInternet = NULL;
	    HINTERNET hConnect = NULL;
	    HINTERNET hRequest = NULL;
	
	    do 
	    {
	        //解析URL
	        URL_COMPONENTS urlComp = { 0 };
	
	        WCHAR Scheme[MAX_PATH] = { 0 };
	        WCHAR HostName[MAX_PATH] = { 0 };
	        WCHAR UserName[MAX_PATH] = { 0 };
	        WCHAR Password[MAX_PATH] = { 0 };
	        WCHAR UrlPath[MAX_PATH] = { 0 };
	        WCHAR ExtraInfo[MAX_PATH] = { 0 };
	
	        urlComp.dwSchemeLength = MAX_PATH;
	        urlComp.dwHostNameLength = MAX_PATH; ////主机名
	        urlComp.dwUserNameLength = MAX_PATH;
	        urlComp.dwPasswordLength = MAX_PATH;
	        urlComp.dwUrlPathLength = MAX_PATH;
	        urlComp.dwExtraInfoLength = MAX_PATH;
	
	        urlComp.dwStructSize = sizeof(urlComp);
	        urlComp.lpszScheme = Scheme;
	        urlComp.lpszHostName = HostName;
	        urlComp.lpszUserName = UserName;
	        urlComp.lpszPassword = Password;
	        urlComp.lpszUrlPath = UrlPath;
	        urlComp.lpszExtraInfo = ExtraInfo;
	
	        if (InternetCrackUrlW(lpUrl.c_str(), 0, 0, &urlComp) == FALSE)
	        {
	            Judge = FALSE;
	            break;
	        }
	
	        //建立会话
	        hInternet = InternetOpenW(L"shangbao", INTERNET_OPEN_TYPE_PRECONFIG, NULL, NULL, 0);
	        if (hInternet == NULL)
	        {
	            Judge = FALSE;
	            break;
	        }
	        cout << "建立会话" << endl;
	
	        //建立连接
	        hConnect = InternetConnectW(hInternet, urlComp.lpszHostName, INTERNET_DEFAULT_HTTP_PORT, urlComp.lpszUserName, urlComp.lpszPassword, INTERNET_SERVICE_HTTP, 0, 0);
	        if (hConnect == NULL)
	        {
	            Judge = FALSE;
	            break;
	        }
	        cout << "建立连接" << endl;
	
	        //http请求句柄
	        DWORD dwOpenRequestFlags = INTERNET_FLAG_IGNORE_REDIRECT_TO_HTTP | INTERNET_FLAG_KEEP_CONNECTION | INTERNET_FLAG_NO_AUTH | INTERNET_FLAG_NO_COOKIES | INTERNET_FLAG_NO_UI;
	        hRequest = HttpOpenRequestW(hConnect, L"POST",urlComp.lpszUrlPath, NULL, NULL, NULL, dwOpenRequestFlags, 0);
	        if (hRequest == NULL)
	        {
	            Judge = FALSE;
	            break;
	        }
	        cout << "请求成功" <<endl;
	
	        //发送请求
	        INTERNET_BUFFERS internetBuffers = { 0 };
	        ::RtlZeroMemory(&internetBuffers, sizeof(internetBuffers));
	        internetBuffers.dwStructSize = sizeof(internetBuffers);
	        internetBuffers.dwBufferTotal = sz;             //数据大小
	        if (HttpSendRequestExW(hRequest, &internetBuffers, NULL, 0, 0) == FALSE)
	        {
	            Judge = FALSE;
	            break;
	        }
	        cout << "请求发送成功" <<endl;
	
	        //发送数据
	        DWORD dwret = 0;
	        if (InternetWriteFile(hRequest, lpBuffer, sz, &dwret) == FALSE)
	        {
	            Judge = FALSE;
	            break;
	        }
	        cout << "发送数据成功" <<endl;
	
	        //结束请求
	        if (HttpEndRequestW(hRequest, NULL, 0, 0) == FALSE)
	        {
	            Judge = FALSE;
	            break;
	        }
	        cout << "结束成功" <<endl;
	
	    } while (FALSE);
	
	    if (hInternet != NULL)
	    {
	        InternetCloseHandle(hInternet);
	    }
	    if (hConnect != NULL)
	    {
	        InternetCloseHandle(hConnect);
	    }
	    if (hRequest != NULL)
	    {
	        InternetCloseHandle(hRequest);
	    }
	    return Judge;
	}

## 满天星--甘愿作配角