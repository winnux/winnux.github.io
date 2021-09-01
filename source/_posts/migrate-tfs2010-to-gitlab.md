---
title: Migrate TFS2010 to Gitlab
url: 95.html
id: 95
categories:
  - Bob
date: 2017-02-06 10:31:22
tags:
---

好久以前就想把项目从TFS上迁移到Git下面，今天终于有时间来试试了。 git管理就选择Gitlab吧，正好centos上有新建立的。 但tfs的内容太多了，去下载了git-tfs工具，解压目录（这个工具要.net4.0和vs什么的，好在开发机器这些都有了） 直接运行git-tfs看看帮助，开始一直在windows 的cmd shell下运行发现各种错误，原来我的git不在环境变量中，就直接打开git bash shell来运行各种git-tfs命令。 用git-tfs clone命令去clone tfs的版本库，可无论如和都提示找不到路径 ./git-tfs clone http://ip:8080/tfs $/projectname 后来google了一下发现是文件路径转换问题，在命令前加入MSYS\_NO\_PATHCONV=1就好了，具体如下

MSYS\_NO\_PATHCONV=1 ./git-tfs clone http://ip:8080/tfs $/projectname

这样运行后，从tfs把所有信息读到本地git仓库了，下面就简单了，把本地git仓库映射到远程gitlab就可以了。 呵呵，完毕~