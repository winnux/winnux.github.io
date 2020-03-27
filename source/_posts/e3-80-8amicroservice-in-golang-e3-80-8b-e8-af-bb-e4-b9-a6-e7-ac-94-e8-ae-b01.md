---
title: 《Microservice in golang》读书笔记1
tags:
  - Golang
url: 471.html
id: 471
categories:
  - Bob
  - 技术
date: 2018-05-09 10:07:46
---

最近比较感兴趣的是天文学、数学分析、AI、还有这个Cloud Native。 《Microservice in golang》这个系列教程是Ewan Valentine在博客中写的一个golang在microservice中的应用，包括了cloud native的很多方面，作为一个练习项目足够。以前粗略的看过前4期，没有看完，最近看这个系列到第10期了，已经结束了。 从我看到的情况，应该是没有涵盖servcie mesh部分内容，但我对这个mesh的体系一直心存敬而远之的感觉。前几天看程序员头条里一个作者写的一段话比较有同感，大致意思是如果micro service在1000个以下不建议上service mesh，有一个统一的gateway api网关做鉴权、限流、接入管理就可以；大于1000个service以上，建议考虑service mesh，毕竟这个时候再自行管理实际工作量要大于直接使用现有的工具了。 我的观点是目前的基于http restful api的service之间交互（代表是spring）应该会慢慢转变为二进制rpc机制，如gRPC等。我一贯对google的技术fans，所以对gRPC更是非常喜欢，尤其是4种rpc调用方式涵盖了server push，stream 等方方面面，protobuf也很好，用着体系感觉很强，开源界的一个大腿啊。 记录一下测试机器（ubuntu16.04 x64）上建立环境 安装grpc https\_proxy=..... go get google.golang.org/grpc -v 安装protobuf 直接在github下载了protobuf的二进制包拷贝到/usr/bin下了 安装protobuf-go-gen go get -a github.com/golang/protobuf/protoc-gen-go 然后把go的bin目录加入PATH 在.zshrc下 export PATH=$PATH：/go bin source ~/.zshrc 然后就可以直接用protobuf建立服务模型，编译出golang源码了 如 protoc -I. proto/consignment/consignment.proto --go\_out=plugins=grpc:. 这样在proto/consignment目录下就生成一个对应的golang源文件了。 btw,发现这个oh-my-zsh很好，都该zsh shell了