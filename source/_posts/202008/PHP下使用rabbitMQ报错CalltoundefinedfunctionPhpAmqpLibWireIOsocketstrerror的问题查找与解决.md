title: PHP下使用rabbitMQ报错Call to undefined function PhpAmqpLib\\Wire\\IO\\socket_strerror()的问题查找与解决
date: '2020-08-03 16:15:18'
updated: '2020-08-03 16:16:59'
tags: [PHP, RabbitMQ]
permalink: /articles/2020/08/03/1596442518677.html
top: ture
---
![](https://img.hacpai.com/bing/20180626.jpg?imageView2/1/w/960/h/540/interlace/1/q/100)

## 场景描述
今天在一个新的环境下，部署一个项目，其中有用到rabbitMQ。但在执行过程中发现报错。错误信息：
```json
{
    "code": 3000,
    "message": "Call to undefined function PhpAmqpLib\\Wire\\IO\\socket_strerror()",
    "data": {
        "file": "D:\\project\\***\\vendor\\php-amqplib\\php-amqplib\\PhpAmqpLib\\Wire\\IO\\StreamIO.php",
        "line": 89
    }
}
```

## 查找问题
根据提示我在对应的文件和行数下找到代码：
![image.png](https://b3logfile.com/file/2020/08/image-9083abe9.png)

我在本地环境下找到`socket_strerror()`所在的文件：
```
\***\plugins\php\lib\php.jar!\stubs\sockets\sockets.php
```

这是PHP的一个扩展，既然是`Call to undefined function`那很可能就是`sockets`相关的扩展没有安装或者打开。

经过查看`php.ini`文件，发现果然是扩展没有打开
![image.png](https://b3logfile.com/file/2020/08/image-3f0d6c1f.png)

## 解决问题
1. 去掉`;extension=php_sockets.dll`前面的`;`，重启服务`PHP`,`apache`服务。

2. 查看`phpinfo()`:
![image.png](https://b3logfile.com/file/2020/08/image-f62cebbb.png)
可以看到`sockets`扩展已可用。报错消失，项目可正常使用。


