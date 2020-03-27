---
title: powerdesigner reverse Oracle database
url: 541.html
id: 541
categories:
  - Bob
  - 技术
date: 2019-01-28 09:18:23
tags:
---

pd好久没用，才发现变成sap下了。准备逆向个oracle库看看。 目前16.6有64位版了，配置了oracle的odbc始终在连接时提示ora一个dll加载失败，我没装oracle的客户端，是直接用的instant client加odbc包，原来要把这个放在用户或者系统PATH里。配置全局odbc源时就没用设置，pd里就不行，琢磨半天发现这里的问题。