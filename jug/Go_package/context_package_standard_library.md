# context package, standard library

## 目录

[TOC]

## Documentation

### Overview

Package context 包上下文定义了 Context type, 比如, 传递的死线, 取消的信号, 在进程之间跨越 API 边界请求值的范围.

向服务器传入请求应创建 Context，向服务器发出调用时应接受 Context。
它们之间的函数调用链必须传播 Context, 可选地将其替换为使用"WithCancel"、"WithDeadline", "WithTimeout", or "WithValue" 创建的 Context.
当取消 Context 时, 从这个 Context 中衍生出的所有其他 Context 也会被取消.

WithCancel, WithDeadline, 和 WithTimeout 函数采取 Context(父), 然后返回一个衍生的 Context(孩子)和一个 CancelFunc.
调用 CancelFunc 会取消孩子及其子女，删除父对孩子的引用, 并停止任何相关的计时器。
调用 CancelFunc 失败, 溢出他的孩子和他们的子女, 直到父被取消或者计时器完成.
go vet 工具检查 CancelFunc, 被用在所有控制流路径上.

使用 Context 的程序应遵循这些规则, 以保持包中的接口一致, 并启用静态分析工具来检查上下文繁殖:

不要将 Context 存储在 struct 类型中; 而是将 Context 显式的传递给每个需要它的函数. Context 应该是第一个参数, 通常命名为 ctx：

```
func DoSomething(ctx context.Context, arg Arg) error {
	// ... use ctx ...
}
```

即使如果一个函数允许也不要传 nil Context.
如果你不确定使用哪个 Context 那么就传 context.TODO

仅用于传输进程和 API 的请求范围数据的 Context Values，而非将可选参数传递到函数.

相同的 Context 可以传递给以不同 goroutines 运行的函数; Context 对于多个 goroutines 同时使用是安全的.

请参阅 https://blog.golang.org/context 例如，使用 Contexts 的服务器的代码.

https://blog.golang.org/context
Go Concurrency Patterns: Context
Go 并发模式: 上下文

### Index

Variables
func WithCancel(parent Context) (ctx Context, cancel CancelFunc)
func WithDeadline(parent Context, d time.Time) (Context, CancelFunc)
func WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc)
type CancelFunc
type Context
func Background() Context
func TODO() Context
func WithValue(parent Context, key, val interface{}) Context

### Examples

WithCancel
WithDeadline
WithTimeout
WithValue

### Constants

This section is empty.

### Variables

```
var Canceled = errors.New("context canceled")
```

当 context 被退出时是 Context.Err 返回的错误

```
var DeadlineExceeded error = deadlineExceededError{}
```

### Functions



### See Also

https://www.cnblogs.com/qcrao-2018/archive/2019/06/12/11007503.html
