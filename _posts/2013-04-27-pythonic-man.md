---
layout: post
title: "Pythonic man"
description: ""
category: 
tags: []
---
{% include JB/setup %}

今天，确切的说是昨天，用python写一个http的小应用，监测系统的，照着一个例子上用httplib，结果运行显示:
InportError: No module named httplib

很明显，找不到httplib，搜索一番，发现需要这样import：

	import http.client

原因是网上的例子用的python2，而本机安装了python3。
HTTPConnection是在http.client下面的。其他和http相关的变动有这几个对应关系：

	httplib -> http.client
	Cookie -> http.cookies
	cookielib -> http.cookiejar
	BaseHTTPServer -> http.server
	SimpleHTTPServer -> http.server
	CGIHttpServer -> http.server

这个地址是关于python2到python3 port的一些变化：
`http://diveintopython3.org/porting-code-to-python-3-with-2to3.html`

Python 又不Pythonic了