title: 转载：centos7 编译安装 php7.4
date: '2020-04-14 11:27:31'
updated: '2020-04-14 11:29:47'
tags: [转载, PHP, CentOS7]
permalink: /articles/2020/04/14/1586834851714.html
---
![](https://img.hacpai.com/bing/20171111.jpg?imageView2/1/w/960/h/540/interlace/1/q/100) 

原文地址：https://www.cnblogs.com/liubaoqing/p/12176017.html

#### 1. 下载安装编译工具

```
yum groupinstall 'Development Tools'
```
#### 2.安装依赖包

```
yum install libxml2 libxml2-devel openssl openssl-devel bzip2 bzip2-devel libcurl libcurl-devel libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel gmp gmp-devel libmcrypt libmcrypt-devel readline readline-devel libxslt libxslt-devel zlib zlib-devel glibc glibc-devel glib2 glib2-devel ncurses curl gdbm-devel db4-devel libXpm-devel libX11-devel gd-devel gmp-devel expat-devel xmlrpc-c xmlrpc-c-devel libicu-devel libmcrypt-devel libmemcached-devel
```

#### 3.下载并解压php7.4

```
cd /usr/local/src
wget  https://www.php.net/distributions/php-7.4.4.tar.bz2
tar jxvf php-7.4.4.tar.bz2
cd php-7.4.4
```

#### 4.编译安装（./configure --help 查看编译参数）

##### 编译前，新增用户组，用户，用于编译使用
```
groupadd www
useradd -g www www
```

##### 开始编译（根据自己需要增减）

```
./configure   --prefix=/usr/local/php   --with-config-file-path=/etc   --with-fpm-user=www   --with-fpm-group=www    --with-curl   --with-freetype-dir   --enable-gd   --with-gettext    --with-iconv-dir   --with-kerberos   --with-libdir=lib64   --with-libxml-dir   --with-mysqli   --with-openssl   --with-pcre-regex   --with-pdo-mysql   --with-pdo-sqlite   --with-pear   --with-png-dir   --with-jpeg-dir   --with-xmlrpc   --with-xsl   --with-zlib   --with-bz2   --with-mhash   --enable-fpm   --enable-bcmath   --enable-libxml   --enable-inline-optimization   --enable-mbregex   --enable-mbstring   --enable-opcache   --enable-pcntl   --enable-shmop   --enable-soap   --enable-sockets   --enable-sysvsem   --enable-sysvshm   --enable-xml    --enable-zip   --enable-fpm
```

- 这里需要注意的是在php7.4 编译参数 --with-gd  要改成了 --enable-gd

---
##### 当报错checking for libzip... configure: error: system libzip must be upgraded to version >= 0.11

先删除旧版本
```
yum remove -y libzip
```

下载编译安装
```
wget https://nih.at/libzip/libzip-1.2.0.tar.gz
tar -zxvf libzip-1.2.0.tar.gz
cd libzip-1.2.0 ./configure
make && make install
```
---

##### error： Package requirements (sqlite3 > 3.7.4) were not met

```
yum install libsqlite3x-devel -y
```
---
##### error: Package requirements (oniguruma) were not met

```
yum install oniguruma-devel -y
```
---
##### 当yum install 提示 "没可用软件包"

```
yum install -y epel-release
```
---
##### off_t undefined 报错

```
configure: error: off_t undefined; check your library configuration
```

off_t 类型是在 头文件 unistd.h中定义的，  
在32位系统 编程成 long int ，64位系统则编译成 long long int ，  
在进行编译的时候 是默认查找64位的动态链接库，  
但是默认情况下 centos 的动态链接库配置文件/etc/ld.so.conf里并没有加入搜索路径，  
这个时候需要将 /usr/local/lib64 /usr/lib64 这些针对64位的库文件路径加进去。

###### 添加搜索路径到配置文件
```
echo '/usr/local/lib64
/usr/local/lib /usr/lib /usr/lib64'>>/etc/ld.so.conf
```
 ###### 然后 更新配置
```
ldconfig -v
```
---

##### /usr/local/include/zip.h:59:21: fatal error: zipconf.h: No such file or directory

```
cp /usr/local/lib/libzip/include/zipconf.h /usr/local/include/zipconf.h
```

##### 编译没问题了，执行make && make install  安装完毕
```
make && make install
```

#### 5.配置环境

执行完安装命令后php7.4就已经安装在到了/usr/local/php目录下了

查看版本

```
/usr/local/php/bin/php -v
```

添加环境变量

```
vim /etc/profile
```

添加到最后

```
PATH=$PATH:/usr/local/php/bin
export PATH
```

更新环境变量

```
source /etc/profile
```

查看版本
```
php -v
```

#### 6.配置php-fpm

```
cp /usr/local/src/php-7.4.4/php.ini-production /etc/php.ini
cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf
cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf
cp /usr/local/src/php-7.4.4/sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm
chmod +x /etc/init.d/php-fpm
```

启动php-fpm
```
/etc/init.d/php-fpm start
```
或者
```
service php-fpm start
```

#### 说明
本文为转载文章，对我帮助很大，所以转载记录一下。如需删除请及时联系

原文地址：https://www.cnblogs.com/liubaoqing/p/12176017.html
