---
title: Travis CI 自动部署blog 
---
## 记录几个问题



### Hexo的帮助内容比较陈旧

按照里面的设置会遇到几个问题，尤其是在github pages页面介绍的Travis CI集成中发布分支gh-pages的有关内容，实际上现在user or organization的发布页面就是master，不能修改的；只有project sites的发布页是gh-pages。

```
sudo: false

language: node_js

node_js:

 \- 10 # use nodejs v10 LTS

cache: npm

branches:

 only:

  \- source # build master branch only

script:

 \- hexo generate # generate static files

deploy:

 provider: pages

 skip-cleanup: true

 github-token: $GH_TOKEN

 keep-history: true

 target_branch: master 

 on:

  all_branches: true

 local-dir: public
```

​	上面我把hexo source放在单独的source branch里面，然后把Travis deploy的branch改为master，这样就能直接发布到页面了，同时一个库管理了hexo的源和publish的页面。对于没有node的环境直接编辑markdown文件就能写blog了。

### 使用Next theme的问题

按照next的GitHub描述的第一种方式直接git clone 一个child project到theme 目录next子目录，这种方式本地hexo d可以，如果要使用Travis CI，有一个问题，貌似CI工具在clone代码没有选择recursion方式，这个在ci页面setting里没找到配置的地方，所以这些child repository是没法clone到CI的工作目录的，导致deploy的站点文件都是空文件。开始用CI deploy后，访问站点是空白页面，还没想到是这里的原因，找了半天发现这个问题。

只能老实的下载next主题的zip包更新到目录中，然后再次push到github，ci工作后再访问页面就正常了。



Btw，这个页面就是ci工具deploy的。




