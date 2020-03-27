---
title: vue.js直接操作svg对象
url: 245.html
id: 245
categories:
  - Bob
date: 2017-04-12 16:31:22
tags:
---

下午看到以前的singalr的svg实时刷新，好久没弄，只是个原型。 想起来是否能用vue直接操作svg对象呢。 简单实验了一下，如果svg是直接写在html里面的还是可以的，就当成dom元素一行可以各种vm操控。 但是用object标签导入的svg文件，不可以，这个结果也在预料之中。 引用一段   “通过object、embed或者iframe标签将SVG文件引入到HTML页面上呈现出来。对于该种情况， 这些标签实际上会把通过`data`或`src`属性指定的内容相对独立的引入到页面上来，也就是其中的内容会有完全属于自己的`document`对象。所以使用原来的`document`对象就无法取得通过上述标签引入进来的SVG文档中元素，更不用说去修改上边的属性了。” 上述原因估计直接本页面去弄，肯定是拿不到关联的。 好在当使用JavaScript获取到这些元素对象的时候，它们都一个方法可以获取所引用的SVG文档的document对象，那就是[`getSVGDocument()`](http://msdn.microsoft.com/en-us/library/ie/hh772865(v=vs.85).aspx)。 如果不用vue这类的，直接写js脚本应该也能搞object引进来的svg对象。这个在做signalR时候已经验证了。 如果想用vue直接操作这些引用对象的内部，估计不行，实际实现起来我觉得也不是没有可能，vue目前的el 取得都是dom树，加一个关键字去取svg对象树就可以。可能不是这么简单，vue的实现没有太多概念。   记录一下，看看以后有没有可能，也不失为一种方式。