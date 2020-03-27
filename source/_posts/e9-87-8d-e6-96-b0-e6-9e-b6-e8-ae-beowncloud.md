---
title: 重新架设owncloud
url: 105.html
id: 105
categories:
  - Bob
date: 2017-02-06 10:33:18
tags:
---

去年在文件服务器上架设过一个基于docker的owncloud，用了一段时间，后来出了一个莫名其妙问题，那个docker image启动出错了，同步了好多文件。哎，没时间弄它。 昨天想着代码服务器负载不多，决定在那上面再弄一个owncloud，这东西太方便了，可以把常用的东西都放在上面，mobile，desktop等用都方便，主要是我花了6个大洋在app store上还买了它一个ios客户端，不能浪费。 鉴于我以前对bitnami的良好印象，直接找bitnami打包的owncloud，直接给我一个惊喜，运行不了。原来是服务器上弄得bitnami的gitlab是基于LAPP的，owncloud需要的bitnami基础架构是LAMP的，后来自己装了个mariadb，用bitnami owncloud lamp module插件安装也不行。 自己从头来吧，centos下添加owncoud源，安装apache、php、mariadb，一套东西弄下来还算顺利，最后给owncloud建数据库，添db 用户填了个本地权限，我恰恰没在本地浏览器打开owncloud初始化页面，结果就是提示mariadb数据库建立表失败。好吧，卸载重来，owncloud在/var/html/下的文件实际是owncloud-file包里，我折腾半天owncloud都没起作用，后来发现自己真是笨死了。 终于再一次乌龙后架设完毕了，呵呵，同步了work doc等一些常用目录，晚上回家还把家里的文件简单上传了同步一些。