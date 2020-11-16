title: PHP遍历删除文件夹以及文件夹里的文件
date: '2019-12-30 15:36:58'
updated: '2019-12-30 16:33:00'
tags: [PHP]
permalink: /articles/2019/12/30/1577691418354.html
---
![](https://img.hacpai.com/bing/20190108.jpg?imageView2/1/w/960/h/540/interlace/1/q/100) 


## 场景描述：

#### 遍历去清空一个目录下的文件夹以及文件

## 代码示例：

```PHP
//删除一个文件夹下面的所有的文件以及文件夹
    static public function delDir($path){
        error_reporting(0);
        //如果是目录则继续
        if(is_dir($path)){
            //扫描一个文件夹内的所有文件夹和文件并返回数组
            $p = scandir($path);
            foreach($p as $val){
                //排除目录中的.和..
                if($val !="." && $val !=".."){
                    //如果是目录则递归子目录，继续操作
                    if(is_dir($path.$val)){
                        //子目录中操作删除文件夹和文件
                        self::deldir($path.$val.'/');
                        //目录清空后删除空文件夹
                        @rmdir($path.$val.'/');
                    }else{
                        //如果是文件直接删除
                        @unlink($path.$val);
                    }
                }
            }
            @rmdir($path);
        }
    }
```
	

