---
title: openshift starter deactive email
tags:
  - Golang
url: 457.html
id: 457
categories:
  - Bob
  - 技术
date: 2018-04-27 14:28:17
---

早上打开邮箱看到openshift的提示邮件，原来申请的starter用户要被down 赶紧去看看，好久没用这里了。 想着看看能不能把昨天弄得signalr的chat例子弄一个saas去试试。 ok，我upload到github了，然后openshift的.net core环境只有2.0，偶这个2.1的build failed   好吧，暂时这样吧，记录一下，openshift starter project 跑了一个空.net core application，这样不会再被下线了吧。什么时候支持2.1了，我在试试。   另外，今天发现golang编译的程序GC在windows和linux下有很多的区别，同样的代码，在win平台编译后运行发现内存是一直再增加，在达到了一个峰值后可能会稳定并减少，感觉是gc工作了。 linux平台下这个程序执行后内存占用没有太大波动，一直很稳定，没感觉gc在工作似的。 不知道具体区别的原因，而且go 的pprof的memory profile，一直没实验成功，没仔细调，以后再关注吧。