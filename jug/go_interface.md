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





## Example

> http://c.biancheng.net/view/58.html

```go
package main

import (
    "fmt"
)

// 调用器接口
type Invoker interface {
    // 需要实现一个Call方法
    Call(interface{})
}

// 结构体类型
type Struct struct {
}

// 实现Invoker的Call
func (s *Struct) Call(p interface{}) {
    fmt.Println("from struct", p)
}

// 函数定义为类型
type FuncCaller func(interface{})

// 实现Invoker的Call
func (f FuncCaller) Call(p interface{}) {

    // 调用f函数本体
    f(p)
}

func main() {

    // 声明接口变量
    var invoker Invoker

    // 实例化结构体
    s := new(Struct)

    // 将实例化的结构体赋值到接口
    invoker = s

    // 使用接口调用实例化结构体的方法Struct.Call
    invoker.Call("hello")

    // 将匿名函数转为FuncCaller类型，再赋值给接口
    invoker = FuncCaller(func(v interface{}) {
        fmt.Println("from function", v)
    })

    // 使用接口调用FuncCaller.Call，内部会调用函数本体
    invoker.Call("hello")
}
```

有如下一个接口:

```go
// 调用器接口
type Invoker interface {
    // 需要实现一个Call()方法
    Call(interface{})
}
```

这个接口需要实现 Call() 方法, 调用时会传入一个 interface{} 类型的变量，这种类型的变量表示任意类型的值。

接下来, 使用结构体进行接口实现.

结构体实现接口

结构体实现 Invoker 接口的代码如下：

```go
// 结构体类型
type Struct struct {
}

// 实现Invoker的Call
func (s *Struct) Call(p interface{}) {
    fmt.Println("from struct", p)
}
```

代码说明如下:

- 第 2 行，定义结构体，该例子中的结构体无须任何成员，主要展示实现 Invoker 的方法。
- 第 6 行，Call() 为结构体的方法，该方法的功能是打印 from struct 和传入的 interface{} 类型的值。

将定义的 Struct 类型实例化，并传入接口中进行调用，代码如下:

```go
// 声明接口变量
var invoker Invoker

// 实例化结构体
s := new(Struct)

// 将实例化的结构体赋值到接口
invoker = s

// 使用接口调用实例化结构体的方法Struct.Call
invoker.Call("hello")
```

代码说明如下：

- 第 2 行，声明 Invoker 类型的变量。
- 第 5 行，使用 new 将结构体实例化，此行也可以写为 s:=&Struct。
- 第 8 行，s 类型为 *Struct，已经实现了 Invoker 接口类型，因此赋值给 invoker 时是成功的。
- 第 11 行，通过接口的 Call() 方法，传入 hello，此时将调用 Struct 结构体的 Call() 方法。


接下来，对比下函数实现结构体的差异。

代码输出如下：

```go
from struct hello
```

函数体实现接口

函数的声明不能直接实现接口，需要将函数定义为类型后，使用类型实现结构体，当类型方法被调用时，还需要调用函数本体

```go
// 函数定义为类型
type FuncCaller func(interface{})

// 实现Invoker的Call
func (f FuncCaller) Call(p interface{}) {

    // 调用f()函数本体
    f(p)
}
```

代码说明如下：

- 第 2 行，将 func(interface{}) 定义为 FuncCaller 类型。
- 第 5 行，FuncCaller 的 Call() 方法将实现 Invoker 的 Call() 方法。
- 第 8 行，FuncCaller 的 Call() 方法被调用与 func(interface{}) 无关，还需要手动调用函数本体。


上面代码只是定义了函数类型，需要函数本身进行逻辑处理，FuncCaller 无须被实例化，只需要将函数转换为 FuncCaller 类型即可，函数来源可以是命名函数、匿名函数或闭包，参见下面代码：

```go
// 声明接口变量
var invoker Invoker

// 将匿名函数转为FuncCaller类型, 再赋值给接口
invoker = FuncCaller(func(v interface{}) {
    fmt.Println("from function", v)
})

// 使用接口调用FuncCaller.Call, 内部会调用函数本体
invoker.Call("hello")
```

代码说明如下：

- 第 2 行，声明接口变量。
- 第 5 行，将 func(v interface{}){} 匿名函数转换为 FuncCaller 类型（函数签名才能转换），此时 FuncCaller 类型实现了 Invoker 的 Call() 方法，赋值给 invoker 接口是成功的。
- 第 10 行，使用接口方法调用。


代码输出如下：

```go
from function hello
```

HTTP包中的例子

HTTP 包中包含有 Handler 接口定义，代码如下：

```go
type Handler interface {    ServeHTTP(ResponseWriter, *Request)}
```

Handler 用于定义每个 HTTP 的请求和响应的处理过程。

同时，也可以使用处理函数实现接口，定义如下：

```go
type HandlerFunc func(ResponseWriter, *Request)func (f HandlerFunc) ServeHTTP(w ResponseWriter, r *Request) {    f(w, r)}
```

要使用闭包实现默认的 HTTP 请求处理，可以使用 http.HandleFunc() 函数，函数定义如下：

```go
func HandleFunc(pattern string, handler func(ResponseWriter, *Request)) {    DefaultServeMux.HandleFunc(pattern, handler)}
```

而 DefaultServeMux 是 ServeMux 结构，拥有 HandleFunc() 方法，定义如下：

```go
func (mux *ServeMux) HandleFunc(pattern string, handler func(ResponseWriter, *Request)) {    mux.Handle(pattern, HandlerFunc(handler))}
```

上面代码将外部传入的函数 handler() 转为 HandlerFunc 类型，HandlerFunc 类型实现了 Handler 的 ServeHTTP 方法，底层可以同时使用各种类型来实现 Handler 接口进行处理。







## See Also

https://www.jb51.net/article/210555.htm

https://zhuanlan.zhihu.com/p/370707755













