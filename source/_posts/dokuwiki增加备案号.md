---
title: dokuwiki增加备案号
date: 2020-04-16 15:29:36
tags:
---







dokuwiki部署

直接用了bitnami的docker 镜像，采用其官网推荐方式docker compose启动，yaml文件改动如下

```
version: '2'
services:
  dokuwiki:
    image: 'bitnami/dokuwiki:0'
    ports:
      - '8082:80'
      - '8443:443'
    environment:
      - DOKUWIKI_USERNAME=
      - DOKUWIKI_PASSWORD=    
      - DOKUWIKI_EMAIL=  
      - DOKUWIKI_WIKI_NAME=
    volumes:
      - ./data:/bitnami
      - ./my_vhost.conf:/vhosts/my_vhost.conf:ro
```

其中把数据文件存在宿主机./data目录里，dokuwiki采用文件方式保存wiki，这样方便存储

另外需要更改一下vhost，my_vhost.conf

```
<VirtualHost *:80>
  ServerName example.com
  DocumentRoot "/app"
  <Directory "/app">
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
  </Directory>
</VirtualHost>
```

其中这个文件在yaml中配置



这么基本配置完了就可以使用容器化的dokuwiki



今天接到主机厂通知，要添加页面网站备案号。

这个footer的位置在data目录下/data/dokuwiki/lib/tpl/dokuwiki，footer文件时tpl_footer.php,修改该文件添加备案

```
<div style="text-align: center">
        	<a href=http://www.beian.miit.gov.cn>网站备案号:1111号</a>
</div>
```

更改后，重新刷新就看到了。