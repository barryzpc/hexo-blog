title: Apache(wamp)下配置ssl证书
date: '2020-08-04 11:40:58'
updated: '2020-08-04 11:44:35'
tags: [wamp, ssl]
permalink: /articles/2020/08/04/1596512458890.html
---
![](https://img.hacpai.com/bing/20180603.jpg?imageView2/1/w/960/h/540/interlace/1/q/100) 

## 环境
WAMP + SSL

**W**indow + **A**pache + **M**ySQL + **P**HP 

## 配置步骤
1. 在apache安装目录下打开httpd.conf文件，找到，并打开注释：
```
LoadModule socache_shmcb_module modules/mod_socache_shmcb.so 
LoadModule ssl_module modules/mod_ssl.so 
Include conf/extra/httpd-ssl.conf 
Include conf/extra/httpd-mpm.conf     //若没有，可忽略该项操作
```
2. 打开 conf/extra/httpd-ssl.conf

打开 apache 安装目录下 conf/extra/httpd-ssl.conf 文件 (也可能是conf.d/ssl.conf，与操作系统及安装方式有关)， 在配置文件中查找以下配置语句:

```
#添加 SSL 协议支持协议，去掉不安全的协议
SSLProtocol all -SSLv2 -SSLv3

# 修改加密套件如下 不同的证书，参数不同
SSLCipherSuite HIGH:!RC4:!MD5:!aNULL:!eNULL:!NULL:!DH:!EDH:!EXP:+MEDIUM   
SSLHonorCipherOrder on

# 证书公钥配置  填写绝对路径
SSLCertificateFile cert/public.pem

# 证书私钥配置 填写绝对路径
SSLCertificateKeyFile cert/214873197740010.key

# 证书链配置，如果该属性开头有 '#'字符，请删除掉
SSLCertificateChainFile cert/chain.pem

```

3. 配置完成后重启apache可能会无法重启。进行一下操作可捕捉错误信息
退出wamp,打开cmd
cd C:\wamp\bin\apache\apache2.4.9\bin     //apache 的安装目录
httpd.exe -k install 
httpd.exe -k start 
检查是否报错   
如果有报错，根据错误提示调整
如果没有报错 则运行 
httpd.exe -k stop

4. 启动wamp

---

## httpd-ssl.conf 完整配置精简demo

```
<VirtualHost *:443>
    DocumentRoot "D:\project\app-test"
    ServerName www.test.com:443
    ServerAlias www.test.com
    ServerAdmin admin@example.com
    <Directory "D:\project\app-test">
        #DirectoryIndex index-dev.php
        Options +Indexes +Includes +FollowSymLinks +MultiViews
        AllowOverride All
        Require local
        Require all granted
    </Directory>
    ErrorLog "C:/wamp64/bin/apache/apache2.4.27/logs/errors.log"
    TransferLog "C:/wamp64/bin/apache/apache2.4.27/logs/access.log"

    SSLEngine on
    SSLCertificateFile "C:/wamp64/bin/apache/apache2.4.27/cert/public.pem"
    SSLCertificateKeyFile "C:/wamp64/bin/apache/apache2.4.27/cert/214873197740010.key"
    SSLCertificateChainFile "C:/wamp64/bin/apache/apache2.4.27/cert/chain.pem"
    <FilesMatch "\.(cgi|shtml|phtml|php)$">
        SSLOptions +StdEnvVars
    </FilesMatch>
    <Directory "C:/wamp64/bin/apache/apache2.4.27/cgi-bin">
        SSLOptions +StdEnvVars
    </Directory>
    BrowserMatch "MSIE [2-5]" \
             nokeepalive ssl-unclean-shutdown \
             downgrade-1.0 force-response-1.0
    CustomLog "C:/wamp64/bin/apache/apache2.4.27/logs/ssl_request.log" \
              "%t %h %{SSL_PROTOCOL}x %{SSL_CIPHER}x \"%r\" %b"
</VirtualHost>
```


