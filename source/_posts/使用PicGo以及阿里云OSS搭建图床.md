---
title: 使用PicGo以及阿里云OSS搭建图床
date: 2024-02-19 14:43:45
tags:
index_img: /img/picgo.png
---

在写博客的时候，总会碰到图片存储的问题，如果图片只存储在本地，那么上传服务器之后博客的文章将无法预览。

这里根据网上的教程自己成功复现了一遍，将大致流程记录一下。



### 一、开通阿里云OSS

进入阿里云首页，搜索OSS服务，并选择 “立即购买”

![image-20240219151132045](https://zzh-blog-images.oss-cn-guangzhou.aliyuncs.com/img/image-20240219151132045.png)

选择合适的资源包（建议40GB + 1年）

![image-20240219151243082](https://zzh-blog-images.oss-cn-guangzhou.aliyuncs.com/img/image-20240219151243082.png)

进入控制台，创建Bucket

![image-20240219151537060](https://zzh-blog-images.oss-cn-guangzhou.aliyuncs.com/img/image-20240219151537060.png)



### 二、下载PicGo

下载地址如下：https://github.com/Molunerfinn/PicGo，下载完成安装即可



### 三、创建用户用于PicGo配置

选择右上角的 “访问控制”

![image-20240219151709013](https://zzh-blog-images.oss-cn-guangzhou.aliyuncs.com/img/image-20240219151709013.png)

创建用户（注意这里的 KeyId 和 KeySecret 需要保存下来）

![image-20240219151815182](https://zzh-blog-images.oss-cn-guangzhou.aliyuncs.com/img/image-20240219151815182.png)

给创建的用户添加权限，选择 “OSS权限”

![image-20240219152013109](https://zzh-blog-images.oss-cn-guangzhou.aliyuncs.com/img/image-20240219152013109.png)



### 四、PicGo配置

将保存的信息拷贝进OSS配置对应的信息框内

![image-20240219151027121](https://zzh-blog-images.oss-cn-guangzhou.aliyuncs.com/img/image-20240219151027121.png)



### 五、Typora配置

“图像” 栏目中选择 “上传图片” 和  “PicGo.app”

![image-20240219145901333](https://zzh-blog-images.oss-cn-guangzhou.aliyuncs.com/img/image-20240219145901333.png)

到此配置完成，接下来只要拷贝图片至Typora下，会自动将图片上传到OSS对应的目录中
