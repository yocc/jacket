# goroutine

---



[toc]



## Overview

### 异步



```go
1.
package main

import (
    "fmt"
)

func main() {
        go func(na string) {
            fmt.Println("子线程正在执行")
        }()
}
返回: 空
// 因为 main() 主线程 和 子线程并行, 当main()主线程执行完并且退出时, 子线程还未来及启动. 
// 又由于无论子线程是否执行完毕也都会随主线程一起退出. 所以没有返回结果.


2.
package main

import (
    "fmt"
		"time"
)

func main() {
  	go func(na string) {
    		fmt.Println("子线程正在执行")
  	}()
    time.Sleep(time.Second * 2)
}
返回:
子线程正在执行
// 增加了主线程执行时间, 可以等到子线程的返回了


3. 
package main

import (
    "fmt"
    "time"
)

func main() {
    names := []string{"niko", "mike", "tony"}
    for _, name := range names {
        go func() {
            fmt.Printf("%s\n", name)
        }()
    }
    time.Sleep(time.Second * 2)
}
返回:
tony
tony
tony
// 主进程中的for里的name虽然在循环时每次都变化, 但不等子线程启动完成, for 循环就已经完成了, name 就只是最后一个 tony. 此时子线程才启动起来处理, 那么三个子线程都拿到了同样的 name 值, 也就都是 tony


4.
package main

import (
    "fmt"
    "time"
)

func main() {
    names := []string{"niko", "mike", "tony"}
    for _, name := range names {
        go func(na string) {
            fmt.Printf("%s\n", na)
        }(name)
    }
    time.Sleep(time.Second * 2)
}
返回:
tony
niko
mike
// for的每次循环时, 将 name 传入 子线程, 
```



## Example

```go
import (
    "fmt"
    "time"
)

func mygo(name string) {
    for i := 0; i < 10; i++ {
        fmt.Printf("In goroutine %s\n", name)
        // 为了避免第一个协程执行过快，观察不到并发的效果，加个休眠
        time.Sleep(10 * time.Millisecond)
    }
}

func main() {
    go mygo("协程1号") // 第一个协程
    go mygo("协程2号") // 第二个协程
    time.Sleep(time.Second)
}

返回:
In goroutine 协程2号
In goroutine 协程1号
In goroutine 协程1号
In goroutine 协程2号
In goroutine 协程2号
In goroutine 协程1号
In goroutine 协程1号
In goroutine 协程2号
In goroutine 协程1号
In goroutine 协程2号
In goroutine 协程1号
In goroutine 协程2号
In goroutine 协程1号
In goroutine 协程2号
In goroutine 协程1号
In goroutine 协程2号
In goroutine 协程1号
In goroutine 协程2号
In goroutine 协程1号
In goroutine 协程2号
```





## FAQ







## See Also

https://golang.iswbm.com/c04/c04_03.html

https://zhuanlan.zhihu.com/p/410781320

