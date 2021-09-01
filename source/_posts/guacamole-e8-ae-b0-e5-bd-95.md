---
title: guacamole记录
url: 93.html
id: 93
categories:
  - Bob
  - 技术
date: 2017-02-06 10:30:59
tags:
---

公司一款基于DCOM的软件用户一直要web远程浏览人机界面，好大一堆基于windows dna的东西难以打包移植到B/S结构下面。真的逼急了，想起一个办法，web的rdp是否可以呢？google发现一个已经发展到0.99版本的open source 的remote desktop的proxy（gateway？），就是本文主角-guacamole。 一般open source的软件1.0就已经意味着很成熟了，这个0.99了，而且各大发行版都有打包，看来应用成熟度应该完全ok。 先在Fedora23的系统下直接用dnf安装打包好的guacd、client、tomcat等一系列东西，弄了半天远程页面一直无响应，估计是tomcat哪里配置问题，没细查。正好有一个ubuntu14.04的vm 系统，直接开始吧，14.04打包的guacamole版本还是0.8.3的，很古老，那就直接源码编译吧。 按照guacamole网站的manual，编译没什么问题，make install没问题。但当时没有手工建立etc下配置文件目录guacamole和配置文件guacamole.properties和user-mapping.xml，这两个文件没有导致后来打开页面登陆不上的。 在tomcat网站下载8.0版本，解压设置java目录，运行即可。这里面的webapp直接在guacamole网站下载war 客户端打包文件拷贝到tomcat webapps目录下，没有编译client那个，实际是一样的。 guacd配置文件具体参考了[http://www.linuxintro.org/wiki/Guacamole](http://www.linuxintro.org/wiki/Guacamole) 这里的有关描述。

> *   create a folder for guacamole’s configuration:
> 
> mkdir /etc/guacamole
> 
> *   create a file /etc/guacamole/guacamole.properties with the content
> 
> \# Hostname and port of guacamole proxy
> guacd-hostname: localhost
> guacd-port:     4822
> 
> \# Location to read extra .jar's from
> lib-directory:  /var/lib/tomcat6/webapps/guacamole-0.9.3/WEB-INF/classes
> 
> \# Authentication provider class
> auth-provider: net.sourceforge.guacamole.net.basic.BasicFileAuthenticationProvider
> 
> \# Properties used by BasicFileAuthenticationProvider
> basic-user-mapping: /etc/guacamole/user-mapping.xml
> 
> *   create a file /etc/guacamole/user-mapping.xml with the content
> 
> <user-mapping>
>    <authorize username="user" password="password">
>       <protocol>vnc</protocol>
>          <param name="hostname">localhost</param>
>          <param name="port">5901</param>
>          <param name="password">password</param>
>     </authorize>
> </user-mapping>