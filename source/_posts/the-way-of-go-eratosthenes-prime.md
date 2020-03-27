---
title: The way of Go - Eratosthenes Prime
tags:
  - Golang
url: 582.html
id: 582
categories:
  - Bob
  - 技术
date: 2020-03-12 13:47:15
---

代码来自Rob Pike的分享

_/*_

_埃拉托斯特尼素数筛算法。_

_算法描述：先用最小的素数2去筛，把2的倍数剔除掉；下一个未筛除的数就是素数(这里是3)。_

_再用这个素数3去筛，筛除掉3的倍数... 这样不断重复下去，直到筛完为止_

_*/_

_func generate(ch chan<- int) {_

_    for i := 2; i < 100; i++ {_

_        ch <- i // Send 'i' to channel 'ch'._

_    }_

_    close(ch)_

_}_

_func filter(src <-chan int, dst chan<- int) {_

_    prime, ok := <-src_

_    if !ok {_

_        close(dst)_

_        return_

_    }_

_    fmt.Println(prime)_

_    out := make(chan int)_

_    go filter(out, dst)_

_    for num := range src {_

_        if num%prime != 0 {_

_            out <- num_

_        }_

_    }_

_    close(out)_

_}_

_//筛选素数_

_func Sieve() {_

_    origin, wait := make(chan int), make(chan int) // Create a new channel._

_    go generate(origin)_

_    go filter(origin, wait)_

_    <-wait_

_}_

完全的golang的并行方式，看看c/c++的对比实现可以看到什么是go的思考方式，从面向过程、面向对象到面向并行，这个里面思维的转变是关键，现在还是受过去的影响，golang妥妥的当成c来写....偶尔go 一下享受下飞翔的感觉....，大多还是func->func，obj->obj，看啦Rob Pike的素数写法，感觉这个真的很Go啊。