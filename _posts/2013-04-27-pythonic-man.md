---
layout: post
title: "Pythonic man"
description: ""
category: 
tags: []
---
{% include JB/setup %}

今天，确切的说是昨天，用python写一个http的小应用，监测系统的，照着一个例子上用httplib，结果显示:
InportError: No module named httplib

接着到phython的lib文件夹下面找了半天也没找到httplib，倒是有一个http的文件夹。然后在帮助文档里找到这个http的用法，似乎和httplib很像，需要这样import：

	import http.client

因为HTTPConnection是在http.client下面的。因为装的是python3，猜想是新的python有些名字改了，在google上一找，果然如此，和http相关的有这几个对应关系：

	httplib -> http.client
	Cookie -> http.cookies
	cookielib -> http.cookiejar
	BaseHTTPServer -> http.server
	SimpleHTTPServer -> http.server
	CGIHttpServer -> http.server

这个地址是关于python2到python3 port的一些变化：
`http://diveintopython3.org/porting-code-to-python-3-with-2to3.html`