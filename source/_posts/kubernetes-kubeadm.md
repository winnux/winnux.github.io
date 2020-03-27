---
title: kubernetes-kubeadm
tags:
  - kubernetes
url: 509.html
id: 509
categories:
  - Bob
  - 技术
date: 2018-09-18 12:52:28
---

vagrant 跑起来，总想实验点什么，原来在单机上minikube感觉了一下，这不vagrant方便搭建测试环境了，就来个完整版kubernetes实验吧。 还真把这个事情想简单了，最重要的和跑minikube那时候开始遇到的问题一样，还是这个墙的问题，当时可是一碰到这个问题没经验，搞了半天才明白我等要用这个还是要有些中国特色的。 这次看了好多kubeadm的安装，由于版本迭代很快，实际最靠谱的还是官网的说明，按照官网说明，整理了一个sh脚本，在vm启动后执行，其中k8s的有关镜像我是前期pull下来的，当时是挂着vpn弄得，后来看有人从github上pull下来，然后重新tag成k8s的image name，在rm原来的，这样也行。我为了方便（测试vm没少destroy）就用go的http file在宿主机上搭了个文件服务，然后vm里去wget一下。 脚本如下：

> #!/usr/bin/env bash setenforce 0 sed -i "s/^SELINUX=enforcing/SELINUX=disabled/g" /etc/sysconfig/selinux systemctl stop firewalld systemctl disable firewalld swapoff -a sed -i 's/.\*swap.\*/#&/' /etc/fstab cat <<EOF > /etc/sysctl.d/k8s.conf net.bridge.bridge-nf-call-ip6tables = 1 net.bridge.bridge-nf-call-iptables = 1 EOF sysctl --system cat <<EOF > /etc/yum.repos.d/kubernetes.repo \[kubernetes\] name=Kubernetes baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86\_64/ enabled=1 gpgcheck=1 repo\_gpgcheck=1 gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg EOF yum install -y net-tools nano wget sudo yum install -y yum-utils \ device-mapper-persistent-data \ lvm2 sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo yum install -y --setopt=obsoletes=0 \ docker-ce-17.03.1.ce-1.el7.centos \ docker-ce-selinux-17.03.1.ce-1.el7.centos systemctl enable docker && systemctl restart docker sudo mkdir -p /etc/docker sudo tee /etc/docker/daemon.json <<-'EOF' { "registry-mirrors": \["https://*****.mirror.aliyuncs.com"\] } EOF sudo systemctl daemon-reload sudo systemctl restart docker yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes systemctl enable kubelet && systemctl start kubelet wget -q http://172.16.64.210:8801/coredns-1.1.3.tar wget -q http://172.16.64.210:8801/kube-scheduler-amd64-1.11.3.tar wget -q http://172.16.64.210:8801/etcd-amd64.3.2.18.tar wget -q http://172.16.64.210:8801/kube-apiserver-amd64-1.11.3.tar wget -q http://172.16.64.210:8801/pause-3.1.tar wget -q http://172.16.64.210:8801/kube-controller-manager-amd64-1.11.3.tar wget -q http://172.16.64.210:8801/kube-proxy-amd64-1.11.3.tar wget -q http://172.16.64.210:8801/kubernetes-dashboard-amd64.1.10.0.tar wget -q http://172.16.64.210:8801/kubernetes-dashboard.yaml wget -q http://172.16.64.210:8801/kubernetes-dashboard-admin.rbac.yaml docker load < coredns-1.1.3.tar docker load < kube-scheduler-amd64-1.11.3.tar docker load < etcd-amd64.3.2.18.tar docker load < kube-apiserver-amd64-1.11.3.tar docker load < pause-3.1.tar docker load < kube-controller-manager-amd64-1.11.3.tar docker load < kube-proxy-amd64-1.11.3.tar docker load < kubernetes-dashboard-amd64.1.10.0.tar

其中docker的镜像从阿里云获取，其他k8s的直接从文件load回来   这样一来我得vagrant up后基本k8s的基本环境就搭建完毕了，然后就是kubeadm init一下，另一个节点join进来就可以了。网络模型简单起见就用了flannel，看介绍这个性能和calico要差很多，测试无所谓，就先跑起来了。 vagrantfile

> \# -*- mode: ruby -*- # vi: set ft=ruby : ENV\["LC\_ALL"\] = "en\_US.UTF-8" app\_nodes = { :master => '192.168.88.10', :node => '192.168.88.20' } Vagrant.configure("2") do |config| config.vm.box="centos/7" app\_nodes.each do |app\_node\_name,app\_node\_ip| config.vm.define app\_node\_name do |app\_config| app\_config.vm.hostname = "#{app\_node\_name.to\_s}.vagrant.internal" app\_config.vm.network :private\_network, ip: app\_node\_ip app\_config.vm.provision "shell",path:"set.sh" app\_config.vm.provider "virtualbox" do |vb| vb.name = app\_node\_name.to\_s vb.customize \["modifyvm", :id, "--memory", "2048"\] vb.customize \["modifyvm", :id, "--cpus", "2"\] end end end end

这两个一个是master，一个跑node节点   最后还有折腾够呛的是kubernetes-dashboard，官网对kubeadm安装明确说明要添加用户访问方式，没细看。本质上这个实际也是个容器deployment，看有人用nodeport模式暴漏的，然后这样测试了一下，注意官网的yaml缺省用户名就可以了。但是，这样在master上访问还是有权限问题（我理解可能还是apiserver的访问权限问题），只有在dashboard的pods所在node节点ip上访问可以。 目前看vagrant这套跑kubernetes测试环境还是很方便的。