title: 如何使用github当作自己的静态文件库
date: '2020-03-11 17:33:38'
updated: '2020-04-22 22:11:21'
tags: [github, JSDelivr]
permalink: /articles/2020/03/11/1583919217986.html
---
![](https://img.hacpai.com/bing/20200218.jpg?imageView2/1/w/960/h/540/interlace/1/q/100) 

## 场景描述

很多时候自己的博客或者是网站项目都会涉及到一些静态文件的访问，如图片、js 文件，CSS 文件。如果全放在自己的服务器上，将会产生不必要的费用，并且访问的速度也不能得到保证。这个时候就需要一个方式能够提供免费高效稳定的方式来存放及访问我们的静态文件。

## 一、在 GitHub 上创建一个图床仓库

![image.png](https://cdn.jsdelivr.net/gh/barryzpc/pic-repo/my-blog/img/image-c1eaf3b5.png)

- PS：一定要选 Public 不然无法访问到资源

## 二、将项目克隆到本地

```shell
 git clone git@github.com:barryzpc/pic-repo.git
```

## 三、创建自己的项目

#### 在 pic-repo 项目中创建自己的项目，比如我有个博客的项目那么我就建立一个 my-blog 的文件夹，并在在文件夹下面分别创建 img,js 等目录：

![image.png](https://cdn.jsdelivr.net/gh/barryzpc/pic-repo/my-blog/img/image-589b7be6.png)

## 四、添加静态文件到对应的目录中

#### 比如，我添加一张图片到我的 img 目录下

![image.png](https://cdn.jsdelivr.net/gh/barryzpc/pic-repo/my-blog/img/image-7f90875c.png)

 ## 五、提交到 GitHub

```
git add .
git commit -m "add pic"
git push 
```

这时图片的已经提交到 GitHub 上

- PS: 如果包含有大量的图片，可以选择用 **[PicGo](https://github.com/Molunerfinn/PicGo)** 这个工具来上传

## 六、如何访问到静态文件

#### 访问的连接如何获取呢？这里就不得不提到一个厉害的 CDN 加速服务 [jsDelivr](https://www.jsdelivr.com/ "jsDelivr")

-  `jsDelivr` 为开发者提供免费公共 CDN 加速服务
- 在中国大陆唯一有 license 的公有 CDN，而且使用中的访问速度也很快
- 并且 `JSDelivr` 能够集成 GitHub、NPM、WordPress 资源，只需要通过符合 JSDelivr 规则的 URL 引用，即可直接使用他们中资源。

#### 获取直链

###### 找到刚才上传图片的链接

```url
https://github.com/barryzpc/pic-repo/blob/master/my-blog/img/04481283.jpg
```

###### 但是这个链接是无法直接访问的，这个时候就可以直接见前面的部分链接直接替换成 `JSDelivr` 的链接，就能在浏览器中直接访问到图片了

```url
https://cdn.jsdelivr.net/gh/barryzpc/pic-repo/my-blog/img/04481283.jpg
```

###### 注意链接替换的规则，如果 GitHub 的图床项目要 release 多个版本，那就需要在链接中加上版本号，具体规则可以查看 [jsDelivr](https://www.jsdelivr.com/ "jsDelivr") 的官网

---

## GitHub 地址

#### **[pic-repo](https://github.com/barryzpc/pic-repo)**

## 博客地址

#### **[http://myblog.zhengpc.com/articles/2020/03/11/1583919217986.html](http://myblog.zhengpc.com/articles/2020/03/11/1583919217986.html)**

## 说明
本文中所有图片均以此种方式存放。可进入github中查看
