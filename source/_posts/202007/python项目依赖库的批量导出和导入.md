title: python项目依赖库的批量导出和导入
date: '2020-07-24 18:00:44'
updated: '2020-07-24 18:02:19'
tags: [python]
permalink: /articles/2020/07/24/1595584844339.html
---
![](https://img.hacpai.com/bing/20191030.jpg?imageView2/1/w/960/h/540/interlace/1/q/100)

## 场景描述
python项目一般都会用到很多依赖环境，当我们迁移项目的时候，运行环境和运行代码都很好迁移。但是大量第三方的依赖库（自带的库就不用担心了）也需要很轻松的批量迁移。

## 解决方案
#### 批量导出

执行下面命令将依赖库到`python_lib.txt`文件中

```
pip freeze > python_lib.txt
```
python_lib.txt 中内容大致是这样：

```
mock==4.0.2
numpy==1.16.2
opencv-python==4.1.1.26
Pillow==7.1.2
protobuf==3.11.3
requests==2.23.0
scikit-learn==0.22.2.post1
scipy==1.2.1
Shapely==1.7.0
six==1.14.0
sklearn==0.0
tensorboard==1.13.1
tensorflow==1.13.1
tensorflow-estimator==1.13.0
termcolor==1.1.0
tornado==6.0.4
.......
```
#### 批量导入

将`python_lib.txt`文件拷贝到新的服务器上，执行下面命令，依赖库就一次安装上了

```
pip install -r python_lib.txt
```


## 更多

如果你有更多或者更好的解决方案，欢迎留言
