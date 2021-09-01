---
title: licode编译摘记
url: 312.html
id: 312
categories:
  - Bob
  - 技术
date: 2017-06-26 13:29:38
tags:
---

这几天研究视频实时传输方案，rtmp这类的基于tcp的延迟有些大。 简单测试局域网的rtmp（用的simple-rtmp-server推流）延迟就不小，可能是我实验方式问题，感觉不应该这么大。 最后看还是要回到原来的udp传输上，除了私有协议外，webrtc算个不错的选择。 google的apprtc搭建起来太麻烦了，而且最近用的好好的green vpn也要挂了（7月1日停服了）。新用的ss都不是很快，后来翻到这个licode开源视频会议类项目，看github很多star，可以试试webrtc的延迟性能。 谁知道安装编译过程好多地方它官网都没写啊，执行完它那个ubuntudeps脚本还有好多库需要安装，编译的时候一开始好多库找不到，都是avutils avcodec这类的，还有个log4cxx。 库全了，编译选项还得改改，缺省的编译是把过时的数据定义当成错误报告，编译不通过，在cmakefile里手工添加一个忽略选项 -Wno-decrated-delcarations 才能继续编译。 然后是第二个坑，没搞懂是不是版本不兼容还是啥其他原因，libavtuils-dev包里的common.h定义报错，google一下参考[http://blog.csdn.net/friendan/article/details/46576699](http://blog.csdn.net/friendan/article/details/46576699)这里所说添加定义 #ifdef \_\_cplusplus #define \_\_STDC\_CONSTANT\_MACROS #ifdef \_STDINT\_H #undef \_STDINT\_H #endif # include "stdint.h" #endif #ifndef INT64\_C #define INT64\_C(c) (c ## LL) #define UINT64_C(c) (c ## ULL) #endif 才编译通过