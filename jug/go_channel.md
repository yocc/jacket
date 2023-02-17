# Channel

---



[toc]



## Overview

### 概念

不要通过共享内存来通信, 而通过通信来共享内存. 通信强制协作.

channel，是一个可以让一个 goroutine 与另一个 goroutine 传输信息的通道，我把他叫做信道，也有人将其翻译成通道，二者都是一个概念。

信道，就是一个管道，连接多个goroutine程序 ，它是一种队列式的数据结构，遵循先入先出的规则。



### 声明, 初始化, 

```go
每个信道都只能传递一种数据类型的数据，所以在你声明的时候，你得指定数据类型（string int 等等）


1. 声明:
var 信道实例 chan 信道类型

2. 初始化:
信道实例 = make(chan 信道类型)

3. 定义, 合并 1, 2
信道实例 := make(chan 信道类型)

4. 关闭
信道用完了，可以对其进行关闭，避免有人一直在等待。但是你关闭信道后，接收方仍然可以从信道中取到数据，只是接收到的会永远是 0。
close(pipline)
对一个已关闭的信道再关闭，是会报错的。所以我们还要学会，如何判断一个信道是否被关闭？
当从信道中读取数据时，可以有多个返回值，其中第二个可以表示 信道是否被关闭，如果已经被关闭，ok 为 false，若还没被关闭，ok 为true。
x, ok := <-pipline

5. example:
// 定义信道
pipline := make(chan int)

信道的数据操作，无非就两种：发送数据与读取数据
// 往信道中发送数据
pipline<- 200

// 从信道中取出数据，并赋值给mydata
mydata := <-pipline

// 关闭
close(pipline)

// 判断是否关闭
x, ok := <-pipline
```

### 信道的容量与长度

```go

一般创建信道都是使用 make 函数，make 函数接收两个参数
第一个参数：必填，指定信道类型
第二个参数：选填，不填默认为0，指定信道的容量（可缓存多少数据）
对于信道的容量，很重要，这里要多说几点：
当容量为0时，说明信道中不能存放数据，在发送数据时，必须要求立马有人接收，否则会报错。此时的信道称之为无缓冲信道。
当容量为1时，说明信道只能缓存一个数据，若信道中已有一个数据，此时再往里发送数据，会造成程序阻塞。 利用这点可以利用信道来做锁。
当容量大于1时，信道中可以存放多个数据，可以用于多个协程之间的通信管道，共享资源。
至此我们知道，信道就是一个容器。
若将它比做一个纸箱子
它可以装10本书，代表其容量为10
当前只装了1本书，代表其当前长度为1
信道的容量，可以使用 cap 函数获取 ，而信道的长度，可以使用 len 长度获取。

package main
import "fmt"
func main() {
    pipline := make(chan int, 10)
    fmt.Printf("信道可缓冲 %d 个数据\n", cap(pipline))
    pipline<- 1
    fmt.Printf("信道中当前有 %d 个数据", len(pipline))
}
输出如下:
信道可缓冲 10 个数据
信道中当前有 1 个数据

```

### 缓冲信道与无缓冲信道

```go
按照是否可缓冲数据可分为：缓冲信道 与 无缓冲信道

缓冲信道

允许信道里存储一个或多个数据，这意味着，设置了缓冲区后，发送端和接收端可以处于异步的状态。

pipline := make(chan int, 10)
无缓冲信道

在信道里无法存储数据，这意味着，接收端必须先于发送端准备好，以确保你发送完数据后，有人立马接收数据，否则发送端就会造成阻塞，原因很简单，信道中无法存储数据。也就是说发送端和接收端是同步运行的。

pipline := make(chan int)

// 或者
pipline := make(chan int, 0)

```

### 双向信道与单向信道

```go
通常情况下，我们定义的信道都是双向通道，可发送数据，也可以接收数据。

但有时候，我们希望对信道的数据流向做一些控制，比如这个信道只能接收数据或者这个信道只能发送数据。

因此，就有了 双向信道 和 单向信道 两种分类。

双向信道

默认情况下你定义的信道都是双向的，比如下面代码

import (
    "fmt"
    "time"
)

func main() {
    pipline := make(chan int)

    go func() {
        fmt.Println("准备发送数据: 100")
        pipline <- 100
    }()

    go func() {
        num := <-pipline
        fmt.Printf("接收到的数据是: %d", num)
    }()
    // 主函数sleep，使得上面两个goroutine有机会执行
    time.Sleep(time.Second)
}
单向信道

单向信道，可以细分为 只读信道 和 只写信道。

定义只读信道

var pipline = make(chan int)
type Receiver = <-chan int // 关键代码：定义别名类型
var receiver Receiver = pipline
定义只写信道

var pipline = make(chan int)
type Sender = chan<- int  // 关键代码：定义别名类型
var sender Sender = pipline
仔细观察，区别在于 <- 符号在关键字 chan 的左边还是右边。

<-chan 表示这个信道，只能从里发出数据，对于程序来说就是只读

chan<- 表示这个信道，只能从外面接收数据，对于程序来说就是只写

有同学可能会问：为什么还要先声明一个双向信道，再定义单向通道呢？比如这样写

type Sender = chan<- int
sender := make(Sender)
代码是没问题，但是你要明白信道的意义是什么？(以下是我个人见解

信道本身就是为了传输数据而存在的，如果只有接收者或者只有发送者，那信道就变成了只入不出或者只出不入了吗，没什么用。所以只读信道和只写信道，唇亡齿寒，缺一不可。

当然了，若你往一个只读信道中写入数据 ，或者从一个只写信道中读取数据 ，都会出错。

完整的示例代码如下，供你参考：

import (
    "fmt"
    "time"
)
 //定义只写信道类型
type Sender = chan<- int

//定义只读信道类型
type Receiver = <-chan int

func main() {
    var pipline = make(chan int)

    go func() {
        var sender Sender = pipline
        fmt.Println("准备发送数据: 100")
        sender <- 100
    }()

    go func() {
        var receiver Receiver = pipline
        num := <-receiver
        fmt.Printf("接收到的数据是: %d", num)
    }()
    // 主函数sleep，使得上面两个goroutine有机会执行
    time.Sleep(time.Second)
}
```



## FAQ



## See Also

https://golang.iswbm.com/c04/c04_04.html

