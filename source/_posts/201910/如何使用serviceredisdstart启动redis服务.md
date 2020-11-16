title: 如何使用service redisd start 启动redis服务
date: '2019-10-29 16:08:22'
updated: '2020-06-28 16:08:55'
tags: [redis]
permalink: /articles/2019/10/29/1572336502745.html
---
![](https://img.hacpai.com/bing/20200109.jpg?imageView2/1/w/960/h/540/interlace/1/q/100) 

#### 场景描述： 
如何使用service redisd start 启动redis服务。为了方便我们习惯使用service redisd start来启动redis服务，但是很多时候却发现直接运行service redisd start 并没有成功启动redis服务。这是因为我们并没有添加redis的service。如果你也遇到这样的问题，希望这篇文章可以帮助到你。


#### 首先，cd到redis的安装目录下，再cd到util，接着执行`./install_server.sh`

```
cd /usr/local/src/redis-3.0.7/utils
./install_server.sh
```

- 说明：该过程会选择相关的配置，一般情况下，默认的就可以了

#### 然后修改服务名称，将原来的redis_6379更名为redisd，这样下次启动比较方便，命令如下：

``````````````````````````````````````
cd /etc/init.d/ 
mv redis_6379 redisd
``````````````````````````````````````

#### 然后，就可以启动redis服务了

`````````````````````
service redisd start
`````````````````````

*ps：如果是想按照自己的redis.conf配置文件启动redis，就需要进入到redis的安装目录，执行redis-server加上配置文件的全路径，如：*

```sh
cd /usr/local/src/
redis-server /path/redis.conf
```
