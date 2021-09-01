---
title: Supervisor xml-rpc简记
tags:
  - Qt
url: 342.html
id: 342
categories:
  - Bob
  - 技术
date: 2017-09-29 12:37:40
---

通讯机的进程管理用了supervisor，其自身带http的服务，可以通过web浏览到管理的进程情况。 但浏览器中控制进程的过程中总容易出现浏览器假死状况，没搞清是服务端还是浏览器的问题，chrome稍好，但也出现过。 本身的C/S配置平台也要对目标机进行管理，研究一下其提供的xml-rpc接口，发现基本够用了。 在GitHub上找到一个qt的xml-rpc库，基本思路就是QNetworkManager进行网络的http通讯，然后整理发送/接收的xml参数为Qt的QVariant类型。 用这个libmaia直接连接目标机的supervisor xml-rpc服务非常方便，直接调用它的一些方法可以直接管理所有的RPC服务。 其中进程的管理方法很多，用起来也很方便。 好工具+好平台 = 快速迭代的好产品，Qt的，Go语言的这些良好的社区环境大大的加快了项目的推进。 最后，最近越来越不愿意动手coding了，总有个思路就不愿意亲手去实现，不是好苗头。 还得坚持写一些，虽然不多，保持“手感”，哈哈。