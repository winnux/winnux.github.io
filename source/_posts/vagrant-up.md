---
title: vagrant up
tags:
  - vagrant
url: 506.html
id: 506
categories:
  - Bob
  - 技术
date: 2018-09-10 16:42:08
---

今天研究了一下vagrant。 以前一直没太关注这个工具，一直想docker的时代，这类vm shell工具是不是夕阳产业了。最近在做grpc分布式的实验，一直在win下，没想用docker，就想起来这个工具了，也是hashicorp的（consul出品方~），试用了一下发现这个在自动化编排虚拟机上还是真方便，如果用docker这些实验环境可能也要写不少脚本，而且还的小心检查yaml的格式和各个docker的配置。在vagrant环境中这些都是基本很简单的几个脚本设置就ok了，而且这些设置可以拷贝使用。 几个简单的命令加几个简单的脚本就能管理一个比较复杂的测试环境了，和docker比虽然比较重（毕竟vm vs container），好在机器内存大，随便跑几个vm基本无压力。 另外，这个工具的好处还有就是网络和文件分享的设置方式简单灵活，docker的网络设置感觉要比使用这个繁琐一些。 还找到一个比较好的gui管理工具vagrant-manager，这个工具win，mac都能有对应，win下是.net 的平台开发，测试一下发现最新版vagrant配合还有个bug，clone下来简单debug一下发现是对virtualbox的settings的参数解析有个bug，有一些参数没配置解析出错了。更改一下运行起来，这个工具实际是调用了 vagrant global-status 获取了系统所有注册的vagrant 虚拟机设置，然后可以在图形菜单中up、halt、ssh等操作，非常方便，连命令行都不用敲了。   准备在以后的开发测试环境建立中基于这个方案，保证一下一致性。