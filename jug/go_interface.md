# Golang interface

---

[TOC]



## Overview

接口的意义:
. 项目分离, 分层中各层都面向接口开发
. 代码解耦
. 

接口思想由来, 鸭子模型, 是不是鸭子取决于他的能力. 能游泳会叫那就可以是鸭子.

接口:
go提倡面向接口编程
一组函数签名的组合
基于功能的面向对象
接口就是一个需要实现的方法列表
Go语言在传递参数时都是传值的

编译时对必要类型的检查
 Go接口是一组方法的集合
 可以理解为抽象类型, 为什么呢? 不管什么类型A, 只要实现了该接口中的方法集, 那么类型A就属于这个类型.

非侵入式的接口, 非侵入式的设计. 即, 并不是将接口和类型直接显示声明/定义/绑定, 而是分别通过接口中的方法在现实时指定接收者的方式来绑定的.
https://www.zhihu.com/question/318138275/answer/699989214

空接口:
interface {}
package main

import "fmt"

func main() {
    var i interface{} = 1
    fmt.Println(i)
}


Go的多态就是通过接口实现的.
何谓多态: 
指的是同一接口的不同实现方式, 多态允许基类的指针指向子类方法。
多态的目的:
1不必编写每一子类的功能调用，可以直接把不同子类当父类看，屏蔽子类间的差异，提高代码的通用率/复用率
2父类引用可以调用不同子类的功能，提高了代码的扩充性和可维护性
多态是面向对象的重要特性,简单点说:“一个接口，多种实现”，就是同一种事物表现出的多种形态。

编程其实就是一个将具体世界进行抽象化的过程，多态就是抽象化的一种体现，把一系列具体事物的共同点抽象出来, 再通过这个抽象的事物, 与不同的具体事物进行对话。


在某一个函数中发现了一个奇怪的现象：一个函数的返回值类型声明的是一个接口的类型，但是实际在函数体内返回的却是一个结构体类型的对象。
自己得到了如下的解释: 一个结构体实现了一个接口，那么函数中返回值类型为接口时，就应该返回这个结构体。


接口允许嵌套, 但不允许方法重复



```go
interface{} (空接口)类型描述了一个具有零个方法的接口. 每个 Go 类型至少实现零个方法, 因此满足空接口.
空接口用作通用容器类型:
var i interface{}
i = "a string"
i = 2011
i = 2.777
```





## Duck typing

在golang中，只要对象实现了某个接口i的所有方法，就能赋值给该接口变量，这是golang实现鸭子类型的基础或前提，有了这个前提，当函数的接收参数是接口的时候，无需关系传入的对象是什么类型，只要它实现了该接口的所有方法，就说该对象是满足该函数的输入参数的数据类型，即只要它满足了鸭子的方法，不管是不是鸭子，都认为它是鸭子，就可以往函数里传.



## FAQ and troubleshooting















## See Also

https://www.jb51.net/article/210555.htm

https://zhuanlan.zhihu.com/p/370707755












