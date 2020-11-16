title: nohup不输出日志文件方法
date: '2019-10-31 11:15:04'
updated: '2020-08-04 12:00:46'
tags: [nohup, linux]
permalink: /articles/2019/10/31/1572491704006.html
---
![](https://img.hacpai.com/bing/20190222.jpg?imageView2/1/w/960/h/540/interlace/1/q/100) 

## 场景描述：

###### 用 nohup 启动程序时，会在当前的目录下生成 nohup.log 文件。这样就会在很短的时间内将磁盘全部写满（*当时的磁盘是 100G，全部写满*），导致别的程序无法正常运行。（*如启动：Jenkins 等*）
###### 那么如何让nohup命令不产生大量的日志文件呢

## 解决思路

既然 nohup 会输出大量的日志。那如何让 nohup 不输出日志呢？
查了很多的资料发现没办法让 nohup 不输出日志。但是可以 利用liunx的黑洞/dev/null，它就像一个无底洞，所有重定向到它的信息都会消失得无影 无踪。这一点非常有用，当我们不需要回显程序的所有信息时，就可以将输出重定向到/dev/null。

	/dev/null

**/dev/null**  ： 在类Unix系统中，/dev/null，或称空设备，是一个特殊的设备文件，它丢弃一切写入其中的数据（但报告写入操作成功），读取它则会立即得到一个EOF。  
在程序员行话，尤其是Unix系统中，/dev/null 被称为位桶(bit bucket)或者黑洞(black hole)。空设备通常被用于丢弃不需要的输出流，或作为用于输入流的空文件。当你读它的时候，它会提供无限的空字符(NULL, ASCII NUL, 0x00)。

## 操作示例
	 nohup java -cp WEB-INF/lib/*:WEB-INF/classes org.b3log.solo.Starter >/dev/null 2>&1 &

- *>/dev/null* 将信息输出到/dev/null 
-  *2>&1* 将错误信息重定向到标准输出
- 最后一个&符号，表示程序在后台运行

## 关于Linux的重定向
- 0:表示标准输入
- 1:标准输出,在一般使用时，默认的是标准输出
- 2:标准错误信息输出

## 补充
这样问题就得到了解决。
如果大神们有更好解决方案可以在写在评论中






