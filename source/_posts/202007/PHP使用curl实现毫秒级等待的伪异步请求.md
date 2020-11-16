title: PHP使用curl实现毫秒级等待的伪异步请求
date: '2020-07-16 23:45:41'
updated: '2020-07-27 09:23:20'
tags: [PHP, CURL, 毫秒级伪异步]
permalink: /articles/2020/07/16/1594914341041.html
---
![](https://img.hacpai.com/bing/20181116.jpg?imageView2/1/w/960/h/540/interlace/1/q/100) 


## 场景描述

在开发中会遇到这样的场景：需要做一个处理，而这个处理需要花费一定的时间，并且这个处理的结果并不是需要及时的得到结果。那么我们就可能会想到使用异步处理，来大大缩短整个流程所需要的时间。而PHP没有很好的异步支持，那么就会想到使用CURL来请求并主动断开链接，不去等待请求结果的返回来实现伪异步。

## 场景升级

这样处理也会出现一个问题，那么我们就不得不接受要花费一定的时间在请求上。这个时间是无法省去的，那么就只能是这个时间越短越好（其实不是越短越好，因为如果时间太短链接还没有请求到就主动断开了）。而之前版本中 `CUROPT_TIMEOUT`最小设置为1。也就是说，客户端至少必须等待1秒钟。这时候我们迫切需要使用毫秒级的等待，然后断开。为整个流程节省更多的时间！

## 代码实现

```php
/**
     * PHP发送异步请求 毫秒级
     * @author Barry
     * @date 2017-10-24
     * @param string $url 请求地址
     * @param array $param 请求参数
     * @param string $httpMethod 请求方法GET或者POST
     * @param array $header
     * @return array
     */
    static public function makeAsyncRequest($url, $param, $httpMethod = 'GET', $header=[]) {
        $oCurl = curl_init();
        if (stripos($url, "https://") !== FALSE) {
            curl_setopt($oCurl, CURLOPT_SSL_VERIFYPEER, FALSE);
            curl_setopt($oCurl, CURLOPT_SSL_VERIFYHOST, FALSE);
        }
        if ($httpMethod == 'GET') {
            curl_setopt($oCurl, CURLOPT_URL, $url . "?" . http_build_query($param));
            curl_setopt($oCurl, CURLOPT_RETURNTRANSFER, 1);
        } else {
            curl_setopt($oCurl, CURLOPT_URL, $url);
            curl_setopt($oCurl, CURLOPT_RETURNTRANSFER, 1);
            curl_setopt($oCurl, CURLOPT_POST, 1);
            curl_setopt($oCurl, CURLOPT_POSTFIELDS, http_build_query($param));
        }

        if (!empty($header)) {
            $headers = [];
            foreach ($header as $k=>$v){
                $headers[] = $k.":".$v;
            }
            curl_setopt($oCurl, CURLOPT_HTTPHEADER, $headers);
        }
        curl_setopt($oCurl, CURLOPT_NOSIGNAL, 1);//必须加，否则在CentOS环境下面设置毫秒级会报错
        curl_setopt($oCurl, CURLOPT_TIMEOUT_MS, 30);//设置cURL允许执行的最长毫秒数

        $sContent = curl_exec($oCurl);
//        print_r($sContent);exit;
//        $aStatus = curl_getinfo($oCurl);
        curl_close($oCurl);
        return json_decode($sContent, true);
    }
```

## CentOS下设置毫秒级会报错Bug

为什么在CentOS服务器下要增加这一行：
```
curl_setopt($oCurl, CURLOPT_NOSIGNAL, 1);//必须加，否则在CentOS环境下面设置毫秒级会报错
```


主要原因：

> 在Linux系统上, 如果使用了系统标准的DNS解析, 则会使用SIGALARM来提供控制域名解析超时的功能，但是SIGALARM不支持小于1s的超时。

分析与解决：可以阅读大神[风雪之隅](https://www.laruence.com/)专门这对该bug分析以及解决写的一篇文章：[Curl的毫秒超时的一个"Bug"](https://www.laruence.com/2014/01/21/2939.html)




## PHP更多异步实现方法

如果想了解更多的PHP实现异步调用方法可以去阅读[风雪之隅](https://www.laruence.com/)也就是大名鼎鼎的**鸟哥**写的一篇博客：[PHP实现异步调用方法研究](https://www.laruence.com/2008/04/14/318.html)
