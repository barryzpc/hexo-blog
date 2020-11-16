title: RabbitMQ的安装以及对应的PHP扩展的安装
date: '2020-03-17 10:49:44'
updated: '2020-03-17 10:58:50'
tags: [RabbitMQ, PHP]
permalink: /articles/2020/03/17/1584413384830.html
---
![](https://img.hacpai.com/bing/20180610.jpg?imageView2/1/w/960/h/540/interlace/1/q/100)

记于2019-2-27

###### 环境：windows 平台，PHP7.1.9

## 1、安装 Erlang

- 因为 RabbitMQ 是用 Erlang 语言编写，安装之前需要先安装 Erlang， 下载地址[http://www.erlang.org/downloads](http://www.erlang.org/downloads)

![image.png](https://img.hacpai.com/file/2020/03/image-b1a355b3.png)

- 我选择的是图中的版本，一直next即可

 ## 2、下载安装RabbitMQ  

- 下载地址[http://www.rabbitmq.com/download.html](http://www.rabbitmq.com/download.html)

- 也是一直next
- 安装完成后并没有启动RabbitMQ，需要执行下面的操作：

![image.png](https://img.hacpai.com/file/2020/03/image-3ecf633e.png)


```shell
 rabbitmq-plugins.bat enable rabbitmq_management

 rabbitmq-service.bat start
```


PS: 2019 年 2 月 12 日添加

- 如果执行 rabbitmq-plugins.bat enable rabbitmq_management 报错，并且是设置好了环境变量后依然出现这个问题：

![image.png](https://img.hacpai.com/file/2020/03/image-ce381e48.png)



- 那么就执行下面的命令后，在执行上面的命令

![image.png](https://img.hacpai.com/file/2020/03/image-fe471b8d.png)


```
 set ERLANG_HOME=C:\Program Files\erl10.2
```

- 可能会报错提示 但已经运行 这个时候执行
```
rabbitmq-service.bat stop
```
- 访问[http://localhost:15672/](http://localhost:15672/) 即可。默认的帐号：guest  密码：guest

![image.png](https://img.hacpai.com/file/2020/03/image-1458e7cd.png)


## 3、安装PHP的rabbitMQ扩展

#### 下载 dll 文件 地址  [http://pecl.php.net/package/amqp](http://pecl.php.net/package/amqp)

1. 先查看 PHP 版本。

![image.png](https://img.hacpai.com/file/2020/03/image-51e11d34.png)

2. 选择问稳定版的 dll

![image.png](https://img.hacpai.com/file/2020/03/image-1cbd4339.png)

 我选择的是 1.9.3 。因为 Thread Safety  enable  ,所以选择的是 TS。

![image.png](https://img.hacpai.com/file/2020/03/image-ae7f0118.png)

3. 下载后解压

![image.png](https://img.hacpai.com/file/2020/03/image-9c95aaed.png)

4. 将 php_amqp.dll 文件放在 PHP 安装目录 ext 下

![image.png](https://img.hacpai.com/file/2020/03/image-6a3a5330.png)

5. php.ini 里面添加

```
 extension=php_amqp.dll
```

6. 将 rabbitmq.4.dll 文件放在 php/php7.1.9/目录下，并在 apach 的 httpd.conf 文件中加上一句：

```
 LoadFile "C:/wamp64/bin/php/php7.1.9/rabbitmq.4.dll"
```
![image.png](https://img.hacpai.com/file/2020/03/image-861d7af8.png)

7. 重启 apache 和 PHP 服务。

8. 访问 phpinfo();

9. 有 amqp 则安装成功

![image.png](https://img.hacpai.com/file/2020/03/image-77d95e4d.png)



## 4、项目中安装php-amqplib库

#### 使用 composer 安装

1. 项目目录下的 composer.json 文件中增加下面内容

```
{
    "require": {
    "php-amqplib/php-amqplib": "^2.8",  //增加这行
  }
}
````
2. 然后执行:

```
 composer update php-amqplib/php-amqplib。
```

#### 更加方便的做法是直接执行

```
composer require php-amqplib/php-amqplib 
```
便可以直接进行安装了

![image.png](https://img.hacpai.com/file/2020/03/image-b6ba5d5b.png)



## 5、接下来就可以尽情的玩耍了。。。
