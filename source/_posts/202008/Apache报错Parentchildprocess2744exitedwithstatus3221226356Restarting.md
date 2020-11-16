title: 'Apache报错 Parent: child process 2744 exited with status 3221226356 -- Restarting'
date: '2020-08-04 10:49:26'
updated: '2020-08-04 10:49:26'
tags: [wamp, PHP]
permalink: /articles/2020/08/04/1596509366225.html
---
![](https://img.hacpai.com/bing/20190711.jpg?imageView2/1/w/960/h/540/interlace/1/q/100)

## 问题描述
Windows系统+apache+mysql+php环境下，访问量大的时候在`apache_error.log` 中出现报错：
```
Parent: child process 2744 exited with status 3221226356 -- Restarting.
```

## 问题分析
这是由于apache的默认堆栈值较小，所以Windows中经常会出现此问题。 当使用分配了大量堆栈的php代码时，就会发生这种情况。

所以要修改堆栈大小（ThreadStackSize）。 建议将其设置为16 Mb（16777216）

## 解决方案
`ThreadStackSize`的设置是在httpd-mpm.conf文件中。但是默认情况下`apache`是没有使用`mpm`模块的。所以要现修改`httpd.conf`文件中的配置。

1. 打开`httpd.conf`中的注释：
```
Include conf/extra/httpd-mpm.conf
```
2. 找到`ThreadStackSize`修改其大小：
```
<IfModule mpm_netware_module>
    ThreadStackSize      16777216
    StartThreads           250
    MinSpareThreads         25
    MaxSpareThreads        250
    MaxThreads            1000
    MaxConnectionsPerChild   0
</IfModule>
```
3. 重启wamp服务。



