title: 使用PHP下载网络图片或文件
date: '2020-08-12 15:45:34'
updated: '2020-08-12 16:04:49'
tags: [PHP, CURL]
permalink: /articles/2020/08/12/1597218334271.html
---
![](https://img.hacpai.com/bing/20181023.jpg?imageView2/1/w/960/h/540/interlace/1/q/100)

## 变量说明

```php
//图片要保存的绝对路径
$path = '/test/test.jpg';

//要下载的网络图片链接
$img_url = 'http://az29176.vo.msecnd.net/videocontent/GrizzlyPeakSF_FF_768_HD_ZH-CN1078880766.jpg'； 
```

## file_get_contents下载网络图片

```php
file_put_contents($path, file_get_contents($img_url));
```

- ps：当用file_get_contents()函数去下载带有https协议的图片或者内容时，报错：

```
file_get_contents(): SSL operation failed with code 1. OpenSSL Error message
```

解决方案：

```
$arrContextOptions = array(
    "ssl" => array(
        "verify_peer" => false,
        "verify_peer_name" => false,
    ),
);

file_put_contents($path,file_get_contents($img_url, false, stream_context_create($arrContextOptions)));
```

## curl下载网络图片

```
 //根据图片URL获取图片内容，适用于https
function getImg($url)
{

    $ch = curl_init();
    curl_setopt($ch, CURLOPT_CUSTOMREQUEST, 'GET');
    curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);//跳过ssl验证
    curl_setopt($ch, CURLOPT_URL, $url);
    ob_start();
    curl_exec($ch);
    $return_content = ob_get_contents();
    ob_end_clean();
    return $return_content;
}


file_put_contents($path, getImg($img_url));
```
