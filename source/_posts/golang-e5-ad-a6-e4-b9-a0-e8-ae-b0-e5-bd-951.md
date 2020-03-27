---
title: Golang学习记录1
tags:
  - Golang
url: 348.html
id: 348
categories:
  - Bob
  - 技术
date: 2017-10-10 12:34:05
---

Golang也看了几本书了，做了几个小项目了。 好久没用，最近有个简单的web server准备用它做个后端服务，发现掉坑里了 具体记录如下： 如果属性名小写则在数据解析（如json解析,或将结构体作为请求或访问参数）时无法解析 type User struct { name string age  int } func main() { user:=User{"Tom",18} if userJSON,err:=json.Marshal(user);err==nil{ fmt.Println(string(userJSON))   //数据无法解析 } }   如上面的例子，如果结构体中的字段名为小写，则无法数据解析。所以一般建议结构体中的字段大写   如果struct里面变量不都是小写的更坑，json marshal的时候只会把首字母大写的解析正确，而且还没有err 返回给客户端发现首字母小写的属性统统不见了。