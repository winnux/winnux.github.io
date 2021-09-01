---
title: go get 使用ssr
url: 376.html
id: 376
categories:
  - Bob
date: 2017-11-01 15:42:56
tags:
---

拜墙之故，大宋go get google包是真杀脑细胞啊 记录一下折腾的过程。 上午搞得火起，乱事还多，就放下了。下午静一静想了一下，估计是ssr代理这种方式和vpn的不同才导致的。 google了一下go get，也发现了关于代理的问题。 最后，还是从github上下了个cow的代理程序，这个cow也是golang编制的，可以把任意二级代理转为http代理 这样，我就先写个配置把ssr的本地代理sock5开开（实际cow直接可连ssr，具体没折腾）。然后再系统里添加http\_proxy 和 https\_proxy环境变量,设置为cow的端口http://127.0.0.1:7777.最后再重新登陆一次实验。 打开 ssr，运行cow，然后go get   一切正常了，终于折腾完了。