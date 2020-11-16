title: '系统更新导致：PHP报错 ''libonig.so.2: cannot open shared object file... ''问题解决'
date: '2020-08-27 17:56:07'
updated: '2020-10-09 11:26:53'
tags: [linux, PHP]
permalink: /articles/2020/08/27/1598522167215.html
---
![](https://img.hacpai.com/bing/20200304.jpg?imageView2/1/w/960/h/540/interlace/1/q/100)

## 问题描述

今天没事查看了一下自己服务器运行的php定时推送任务的日志，发现今天的运行报错了。而之前的运行都成功了。

> 报错信息：

```
/usr/local/php/bin/php: error while loading shared libraries: libonig.so.2: cannot open shared object file: No such file or directory
```

我回想了一下肯定是昨天执行了`yum -y update` 这个命令更新了系统，导致一些旧的库被删除了。

## 问题分析

错误提示`libonig.so.2`找不到了。去到`/usr/lib64`目录下发现确实没有这个库。
那么就要想办法把这个库装上去应该就没问题了

## 解决方案

1. 执行命令：

```
yum install oniguruma - y
yum install libsodium -y
```

2. 然后执行 php -v ：

```
php -v
```

发现还是报原来的错误。再次进入`/usr/lib64`目录下发现目录下没有`libonig.so.2`，但是有`libonig.so`，那么是不是把`libonig.so`软链到`libonig.so.2`就可以了。

3. 于是执行：

```
ln -s /usr/lib64/libonig.so /usr/lib64/libonig.so.2
```

4. 再次运行`php -v`：

```
[root@****~]# php -v 
PHP 7.4.4 (cli) (built: Apr 14 2020 10:27:44) ( NTS )
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
```

5.发现php正常运行， 问题得到解决，所以千万不要轻易执行`yum -y update`

