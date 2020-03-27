---
title: vagrant跑kubeadm测试k8s的问题
tags:
  - kubernetes
url: 511.html
id: 511
categories:
  - Bob
  - 技术
date: 2018-09-19 16:33:53
---

由于vagrant缺省会有一个nat的网卡，如果按照k8s官网的说明，直接运行kubeadm init 或者join都会有网卡绑定问题。 缺省都会绑定到nat那块卡，我测试环境即使在init时指定ip，但在node的详细信息yaml中还是看到ip地址绑定到了10.0.2.15那个nat网卡。 只有手工设置了/etc/hosts文件，把两个vm的主机名绑到内网ip地址，这时候运行 就可以了。 在node节点上，改完hosts文件，还要修改一下/etc/systemd/system/kubelet.service.d/10-kubeadm.conf文件里，增加kubelet的 --node-ip地址为新增的内网ip。这个操作要在join之前执行，完毕后重启kubelet服务。再kubeadm join 这个地方费了半天，在k8s的issue里好多呼吁在kubeadm join时候增加--node-ip选项的。可能生产环境部署没这些需求吧。 今天才算把k8s这个测试环境完全搭建完毕，跑k8s官网的例子都没问题了。