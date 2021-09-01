---
title: 折腾minikube的一天
tags:
  - kubernetes
url: 402.html
id: 402
categories:
  - Bob
  - 技术
date: 2017-12-21 14:06:37
---

从昨天下午开始，开始折腾minikube，记录一下折腾记录，顺带吐槽一下G-F-W windows下打算用hyperv做vm-driver，启动后各种水土不服，没具体深入研究，放弃了。 转移到linux工作站继续吧，好吧，剩下来的原本可能只需要一支烟的功夫生生的折腾了一天，基本在和墙作战。 linux下安装virtualbox，强烈不推荐linux直接host docker开始，那样会有好多东西都是root级别的，非常不安全。 linux下兴致勃勃地安装完了，顺带说一下，墙有时抽风，记得一开始minikube start后顺利的下载了minikube的iso，后来说什么也不行了。 可想而知，在伟大的墙上，撞得满头包。 好吧，我准备从头来，clone minikube源码，查找所有gcr.io的替换成阿里的镜像源，我记得我是整个目录所有文件都替换了，满以为这下该没问题了。启动，然后等着，等着，发现第一个kube-system namespace 下的pod（记得是addon-manager吧）就一直containercreating，这个慢慢无期。 kubectl describe pods --all-namespaces下看到这个pod的image源已经是阿里的url了，而且特意去阿里看看那个版本6.4beta2也有啊，就是卡在containercreating，看来还是有问题 minikube logs一下，我的个神啊，怎么替换了所有gcr，还去gcr pull image 找不到啥原因了，没办法，继续在阿里云上找，看到阿里貌似有个处理过的minikube二进制，应该是都用的国内源，这下好了。 curl -Lo minikube http://kubernetes.oss-cn-hangzhou.aliyuncs.com/minikube/releases/v0.23.0/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/ 版本0.23，没去管，貌似google上的是0.24了，不管了。 下载下来，然后minikube delete 原来的，重头再来 参考各种坑，这回加了一些启动参数， 具体启动命令行如下： minikube start --registry-mirror=https://registry.docker-cn.com --docker-env HTTP\_PROXY=http://172.16.64.210:7777 --docker-env HTTPS\_PROXY=http://172.16.64.210:7777 其中两个docker的代理是我的一台能ssr fuck g-f-w的windows机器通过cow（以前介绍过的golang编写的二级代理，sock5转HTTP）做的代理。 这回好了，启动... 等一会，运行 kubectl describe pods --all-namespaces 哈哈，3个kube-system的pod都正常启动了，状态终于是running了，热泪盈眶啊 这个中间也幸亏参考了如下两个记录 [http://blog.csdn.net/guizaijianchic/article/details/78421800](http://blog.csdn.net/guizaijianchic/article/details/78421800) [https://blog.alexellis.io/minikube-behind-proxy/](https://blog.alexellis.io/minikube-behind-proxy/) [https://yq.aliyun.com/articles/221687](https://yq.aliyun.com/articles/221687) 想起来前几周看老外那个《Advanced Cloud Native Go》教学视频，看人家轻轻松松 start一下，就启动minikube， 当时我可没想到在这里我付出的时间是n倍于人家，而且研究的一些东西都是没啥用的，主要的还没开始呢。