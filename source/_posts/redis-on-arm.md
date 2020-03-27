---
title: redis on arm
url: 438.html
id: 438
categories:
  - Bob
  - 技术
date: 2018-04-10 09:43:09
tags:
---

redis对arm的已经支持，原来想这个交叉编译是否还有很多坑，今天简单测试发现异常方便，redis 4.0版本的makefile基本已经兼容arm的交叉编译了。 前提也是设置完tool chains 路径 参考了2012年的[一篇文章](https://jas0nliu.wordpress.com/2012/11/03/compile-redis-2-6-2-for-synology-ds212j-marvell-arm/)，对的目标是redis2.6，基本上4.0的编译也一样 具体设置 ![](http://imfiona.cn/wp/wp-content/uploads/2018/04/1-1024x583.png) 这里make完毕基本上就能用了。