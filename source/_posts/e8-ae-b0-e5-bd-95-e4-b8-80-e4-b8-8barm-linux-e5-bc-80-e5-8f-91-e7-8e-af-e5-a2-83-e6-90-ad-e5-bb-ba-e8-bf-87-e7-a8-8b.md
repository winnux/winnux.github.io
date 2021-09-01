---
title: 记录一下arm-linux开发环境搭建过程
url: 431.html
id: 431
categories:
  - Bob
  - 技术
date: 2018-04-09 11:23:46
tags:
---

最近手头有一个am335芯片的测试设备，厂家给的系统，其中linux没验证是否是RT的。 原来没有弄过这种arm cross compile，要了一份toolchain。 后来发现ubuntu 本身就有arm交叉编译工具链包，具体几个包是gcc-arm-linux-gnueabi 、g++-arm-linux-gnueabi。 结果想当然也有个gdb-arm-linux-gnueabi，结果没发现库里有，只有一个gdb-arm-none-gnueabi，安装测试也能用。 用系统库里的省却了手工设置路径一套东西，没仔细研究那些。 下载了一个eclipse cdt，直接测试了一下，发现最新的cdt已经支持ssh remote debug了，貌似现在有个不方便的是目标机目录必须和调试机一样，不知道是不是没找到设置的地方。后来还是用原来的方式直接用了gdb manaual remote debug launch方式，这种方式远程机需要用gdbserver启动程序，然后调试机去debug 目标机运行 gdbserver :1234 programname,调试开始，就可以了。 另外，golang直接可以编译输出arm的应用程序，拷贝到目标机直接执行没任何问题，记录一下命令 #编译一般go程序 GOARM=7 GOARCH=arm GOOS=linux go build -o outfile #编译内嵌c代码的go程序 CGO_ENABLED=1 CC=arm-linux-gnueabihf-gcc GOARM=7 GOARCH=arm GOOS=linux go build -o outfile golang太方便了，调试啥的没实验，目前liteide设置arm编译会有错误，goland没测试，有时间再试试。   追加记录一下，目前看用eclipse cdt做cross compile非常方便，又下载了一个linaro的gcc 工具包，里面包括所有的gcc、gdb、g++都有，而且google上看StackOverflow有人推荐用这个chain，貌似网站上的gcc版本都很新。 eclipse 目前在项目属性里直接设置cross compile predix和目录就可以选择用哪个工具链了，而且如果不用把tool chain必须加入PATH路径里面，直接就可以使用。 这对搞cross compile 来说真的是太方便了，我这换来换去实验了好几个cross compiler，就是改改cross compliler predix而已，貌似我得目标机对编译完了的执行文件如果是静态链接的都没问题，一些程序如果动态链接会出现目标机无法运行问题。 把开源的lib-60870的库实验了一下，做了个104的简单server，测试跑一下很好，非常简单就搞定了。