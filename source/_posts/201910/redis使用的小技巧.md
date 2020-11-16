title: redis使用的小技巧
date: '2019-10-29 16:26:33'
updated: '2019-10-29 16:29:19'
tags: [redis]
permalink: /articles/2019/10/29/1572337593869.html
---
![](https://img.hacpai.com/bing/20180518.jpg?imageView2/1/w/960/h/540/interlace/1/q/100)

### redis使用的小技巧
##### 按照key批量删除某些值
    redis-cli -h 11.11.11.100  -p 6379 -a password keys "key*" | xargs redis-cli -h 11.11.11.100  -p 6379 -a password del
 - 说明 redis-cli -h 11.11.11.100   -p 6379 -a password用于登陆redis客户端；-h 是ip地址， -p是端口号，-a 是登录密码
 - "key*" 用于前匹配到相关的key的值
 - xargs redis-cli -h 11.11.11.100  -p 6379 -a password del 执行删除操作
 

* * *

##### 查看redis客户端最大连接数
###### 连接redis-cli 后执行：
    config get maxclients
###### 如果要修改，可到redis.conf中修改

##### 查看redis当前连接数
    info clients


