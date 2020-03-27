---
title: android下读写usb设备
url: 291.html
id: 291
categories:
  - Bob
date: 2017-05-26 10:15:14
tags:
---

对Android了解的还是不多，原以为还得用底层libusb那些从linux层面解决usb通讯。查android developer才发现已经提供上层api接口了，直接可以工作于用户层面作为host操作usb设备。 按developer网站整理 两种方式： 1.写manifest文件filter关注的vid pid，这里的好处是可以在设备插入时系统会提示是否开启app处理。 2.枚举所有usb设备，按vid pid 找到自己要用的 其他的interface、endpoint都有对应的类，直接使用就可以了。 另外就是有个不太方便的地方，没找到测试endpoint是in/out的方式，当前只是测试出来写（补充，endpoint类 里有方法辨别in/out，方式和标准一样，0x80 表示in 0x00 表示out）。 整个发送接收流程也挺方便。当然，我测试的都是同步模式，异步模式或者同步线程没测试。