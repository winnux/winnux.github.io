---
title: 第一个服务上线了
url: 262.html
id: 262
categories:
  - Bob
  - 技术
date: 2017-04-26 13:07:40
tags:
---

今天上午部署了一台centos7的服务器，作为第一个线上项目服务器使用。 centos7镜像用的usbwriter写入u盘的，应该是UEFI引导启动的。开始直接用ultraiso写了安装不上。 服务器里直接yum安装了node6.10，添加了EPEL源。 在mongodb网站有rhel、centos等安装最新版mongodb的源文件，直接建立然后yum install mongo-org 就是最新的3.4版本了。centos自己源里面的mongo是2.6的。 然后拷贝nodejs项目，简单的跑起来。 这个服务器前端是一个dmz网关，做了反向代理，开了9000端口 android 手机开4g，运行我的app，然后就一切正常了，数据刷新速度和局域网测试基本感觉不出明显区别。   至此，第一版服务算可以上线了，哈。