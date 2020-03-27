---
title: influxdb体验-服务器监控
url: 232.html
id: 232
categories:
  - Bob
  - 技术
date: 2017-03-31 12:54:42
tags:
---

昨天看了下influxdb的资料，觉得这个时序数据库有点意思，今天上午本想能很快测试一下，结果整整弄了一上午才跑起来服务器监控，其中不少坑，网上的例子都是过时的东西，好多还得查手册。 事情原由还要从我的samba server服务不稳说起，最近总有人说访问慢，无法访问等，我没在意，重启过服务，后来发现还有人说不行，有点怀疑是否是samba那个服务器用的hub有问题，一直没结论。正好influxdb 配合collectd监控可以一用，结合grafana挺好。 看上去很美的组合，按照前人写的文章，本以为很容易，没想到坑很多。 首先我这个samba server是个centos6.5的“老”服务器， collectd这个包就没有，后来用个其他rpm源安装上了，而且新版本collectd的配置文件基本上core部分都没打开注释，而且plugin的路径在64位系统在/usr/lib64/collectd 目录下，这些搞定了，设置plugin network 只保留server ip port一行，其余注释掉就行，它和influxdb之间用udp，而不是有人说的tcp通信。这个坑半天。 influxdb比较简单，go写的，安装配置没啥说的，但collectd 插件配置时候注意其缺省的type文件路径有误，要改成 collectd实际的types.db文件路径。   最后 grafana这个早就不用外部server了，下载其rpm包自己安装完毕，配置data source ,dashboard，添加graph，比较坑的是添加graph的数据源方法是双击graph的标题，其余没任何提示，找半天才找到。 不过效果还可以，collectd打开的插件很少，没深入研究，但看它实际可用插件非常多，应该能很好。 ![](http://imfiona.cn/wp/wp-content/uploads/2017/03/无标题-1024x695.png) 贴一些主要更改的配置文件 collectd.conf Hostname "localhost" FQDNLookup true BaseDir "/usr/var/lib/collectd" PIDFile "/usr/var/run/collectd.pid" PluginDir "/usr/lib64/collectd" TypesDB "/usr/share/collectd/types.db" Interval 10 Timeout 2 ReadThreads 5 ... LoadPlugin network ... <plugin network> server "127.0.0.1" " 25286" <plugin> influxdb.conf \[\[collectd\]\] enable = true bind-address=":25826" database="collectd" typesdb="/usr/share/collectd/types.db" ...   grafana可以下载基本没有用配置，缺省监听3000，用户密码都是admin。