title: 如何隐藏HTTP响应头里的server和X-Powered-By
date: '2019-11-21 18:35:17'
updated: '2019-12-30 15:46:46'
tags: [运维, PHP]
permalink: /articles/2019/11/21/1574332517065.html
---
![](https://img.hacpai.com/bing/20180107.jpg?imageView2/1/w/960/h/540/interlace/1/q/100)

## 场景描述

很多时候我们为了安全考虑，需要隐藏我们环境的一些版本号，如：server、X-Powered-By

## 未隐藏时http响应头中包含了如下信息

![image.png](https://img.hacpai.com/file/2019/11/image-0226a7ee.png)

## apache 隐藏 server

修改httpd.conf 设置 

	ServerSignature Off
	ServerTokens Prod

## nginx 隐藏 server

修改nginx.conf  在http里面设置 
	
	server_tokens off;

## 隐藏X-Powered-By  
  
修改 php.ini 文件 设置 
	
	expose_php = Off

## 隐藏之后显示

![image.png](https://img.hacpai.com/file/2019/11/image-04c2fd90.png)



