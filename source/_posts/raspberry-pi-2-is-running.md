---
title: Raspberry Pi 2 is Running
url: 75.html
id: 75
categories:
  - Bob
  - 技术
date: 2017-02-06 10:24:57
tags:
---

手头的Pi 一直没找到特别合适的应用场合，也就一直拖着没买官方那个屏幕，接着大显示器每天有时间就鼓捣一会，Android、Raspbian、Kodi什么的尝试了好多。android貌似一直没有合适的显示硬件驱动，图形能力好弱，基本不具备实际用途。 现在弄了个定制kodi装上了，linux下没有合适导航软件，那个navit国内地图好旧，没啥用，现在就是一个多媒体播放机。 最近鼓捣visualgdb，发现直接支持raspberry pi，好多toolchain、qt啥的都有了，闲着没事把原来一个qHmi弄到pi上了，整个过程及其简单，毕竟程序是基于qt4的，原来一直也用过。 pi上装qtcreator（visualgdb能交叉调试qt，暂时没实验）、qwt库等 qtcreator里设置编译器，选择正确的toolchain g++就可以。 然后编译，略慢（貌似只用了一个核心，cpu一直25%） 运行，一切正常。 看来要换个思路玩pi也可以，家用、车用什么的不想，考虑用在工作环境里试试。 弄个手持设备啥的太轻松了，加个usb wifi，弄个屏幕，整个壳子，就一个ugly pad，哈。   来源于2015-11-18