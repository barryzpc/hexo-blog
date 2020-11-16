title: mysql 计算两个时间的差
date: '2019-10-29 16:43:40'
updated: '2019-12-29 22:34:01'
tags: [mysql]
permalink: /articles/2019/10/29/1572338620193.html
---
![](https://img.hacpai.com/bing/20171210.jpg?imageView2/1/w/960/h/540/interlace/1/q/100)

#### example：
	TIMESTAMPDIFF(MINUTE, FROM_UNIXTIME(SourceTime) , FROM_UNIXTIME(AccountTime)) AS '开户时间',

 - 时间差函数：TIMESTAMPDIFF()

##### 返回值是 AccountTime (时间戳)  减去  SourceTime(时间戳) 的时间差；

##### 返回值可以是秒，分……：
	SECOND   -->秒 
	MINUTE     -->分钟 
	HOUR       -->小时
	DAY          -->天 
	MONTH     -->月 
	YEAR        -->年

- PS：如果时间格式是（'Y-m-d H:i：s'）则不需要用FROM_UNIXTIME()函数，如果是时间戳则需要转换时间格式。
