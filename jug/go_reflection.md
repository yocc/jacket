# go run-time reflection

----

[toc]





## Overview

reflection
run-time reflection







### The Laws of Reflection
#### Introduction, 介绍

计算中的 反射Reflection 是程序检查其自身结构的 能力, 特别是通过类型来检查自身; 
它是 元编程metaprogramming 的一种形式. 这也是混乱的一大根源.
注意 2022年1月添加: 这篇博客文章写于2011年, 早于 Go 中的参数化多态性(又名泛型)

#### types and interfaces, 类型和接口

Go 静态类型, 编译时就要固定一个已知的类型.
Go 是静态类型的. 每个变量都有一个静态类型, 也就是说, 只有一种在编译时已知且固定的类型: int、float32、*MyType、[]byte 等.
如果我们声明

```go
type MyInt int
var i int
var j MyInt
```

然后我有 int 类型, j 有 MyInt 类型.
变量 i 和 j 具有不同的静态类型, 尽管它们具有相同的底层类型, 但它们不能在没有转换的情况下相互分配.

一类重要的类型是接口类型, 它表示固定的方法集. (在讨论反射时, 我们可以忽略将接口定义用作多态代码中的约束.) 
接口变量可以存储任何具体(非接口)值, 只要该值实现接口的方法即可. 一对著名的示例是 io.Reader 和 io.Writer，它们是 io 包中的 Reader 和 Writer 类型:

```go
// Reader is the interface that wraps the basic Read method. 
// Reader 是封装了基本 Read 方法的接口.
type Reader interface {
    Read(p []byte) (n int, err error)
}

// Writer is the interface that wraps the basic Write method. 
// Writer 是封装了基本 Write 方法的接口.
type Writer interface {
    Write(p []byte) (n int, err error)
}
```

使用此签名实现读取(或写入)方法的任何类型都被称为实现 io.Reader(或 io.Writer).
出于本讨论的目的, 这意味着 io.Reader 类型的变量可以保存其类型具有 Read 方法的任何值:

```go
var r io.Reader
r = os.Stdin
r = bufio.NewReader(r)
r = new(bytes.Buffer)
// and so on
```

重要的是要清楚, 无论 r 可能持有什么具体值, r 的类型始终是 io.Reader: Go 是静态类型的, 而 r 的静态类型是 io.Reader.

接口类型的一个极其重要的例子是空接口:

```go
interface{}
```

或其等效别名, 

```go
any
```

它代表空的方法集, 任何值都可以满足它, 因为每个值都有零个或多个方法.

**有人说 Go 的接口是动态类型的, 但这是一种误导.** 
**它们是静态类型的: 接口类型的变量始终具有相同的静态类型, 即使在运行时存储在接口变量中的值可能会改变类型, 该值也将始终满足接口.**
我们需要对所有这些都保持精确, 因为反射和接口密切相关.

#### The representation of an interface, 接口的表示
Russ Cox 写了一篇关于 Go 中接口值表示的详细博客文章(https://research.swtch.com/2009/12/go-data-structures-interfaces.html). 这里没有必要重复完整的故事, 但有必要做一个简化的总结.

**接口类型的变量包括一对儿, value 和 type, 其中 value 是指具体的值, type 是 value 的类型.**
接口类型的变量存储一对儿: 分配给变量的具体值, 以及该值的类型描述符.
更准确地说, 值是实现接口的底层具体数据项, 类型描述了该项的完整类型. 例如, 之后

```go
var r io.Reader					// 接口类型
tty, err := os.OpenFile("/dev/tty", os.O_RDWR, 0)		// 具体数据和接口类型 (concrete data, type(concrete data))
if err != nil {
    return nil, err
}
r = tty									// r: (tty, *os.File), 此时, r 已经从只有一个 Read() 方法, 变成了含有 *os.File 结构体指针类型下面的一大堆方法了.

/*
对儿, (tty, *os.File):
tty: concrete data
type: *os.File 结构体指针类型
*/
```

`r` 包含 (value, type) 对儿,  (tty, *os.File)
请注意, *os.File 类型实现了 Read() 以外的方法: 尽管接口值 r 仅提供对 Read() 方法的访问, 但内部的值包含有关该值的所有类型信息. 这就是为什么我们可以这样做:

> 声明 变量a 是 A 接口类型, A 接口类型 中有 aa( )方法, 此时 变量a 并未被赋值, 还没有 (value, type) 对儿 (tty, *os.File);
> 变量b 是 B 接口类型, B 接口类型中 含有 aa() 方法, 同时还有其他 bb(), cc() 方法等;
> 此时将 变量b 赋值给 变量a, 那么 变量a 也具有 变量b 的所有方法了. 这里有两个问题: 能不能赋值和赋值的内容是什么?;
> 声明 变量c 是 C 接口类型, C 接口类型 中有 cc() 方法;
> 此时将 变量a 进行 类型断言 为 C 接口类型, 然后将 变量a 赋值给 变量c;
> 此时可以通过 类型断言 的方式, 将 变量a 进行 类型断言, 并赋值给 变量c, 那么, 变量c 就只具有 变量a 断言后的类型了.

```go
var w io.Writer
w = r.(io.Writer)				// 类型断言, `io.Writer`是一个接口类型, 包含一个 Write() 方法; `os.File` 结构体类型中也有 Write() 方法, 所以说 `os.File` 结构体类型 实现了 `io.Writer` 接口.

/* 
赋值后, w 就拥有了一对儿(具体值和值类型): 
tty: concrete data
type: *os.File 结构体指针类型

w 的具体值(concrete data) 具有了值类型中的很多方法, 但并不代表 w 可以使用这些方法, w 只能使用 w 在最初定义时的静态接口类型里面的方法.
*/
```

这个赋值中的表达式是一个类型断言; 
它断言的是 r 中的项目也实现了 io.Writer, 因此我们可以将它分配给 w.
赋值后, w 将包含对 (tty, *os.File). 这与 r 中持有的对是同一对.
接口的静态类型决定了可以使用接口变量调用哪些方法, **即使内部的具体值可能具有更大的方法集**.
继续, 我们可以这样做:

```go
var empty interface{}
empty = w
```

我们的空接口值 empty 将再次包含同一对 (tty, *os.File).
这很方便: 一个空接口可以保存任何值 ,并包含我们可能需要的关于该值的所有信息.
(我们在这里不需要类型断言, 因为静态已知 w 满足空接口. 在我们将值从 Reader 移动到 Writer 的示例中, 我们需要显式并使用类型断言, 因为 Writer 的方法是不是 Reader 的子集.)
**一个重要的细节是 接口变量 内部的 对儿, 总是有形式(value, concrete type)(值, 具体类型)(tty, *os.File), 不能有形式(value, interface type)(值, 接口类型). 接口保存不住接口值.**
现在我们准备好反射了.

#### The first law of reflection, 反射第一定律 
1. Reflection goes from interface value to reflection object.

   反射从接口值到反射对象

   最基本的, 反射是一种机制, 这种机制是检查存储在 接口类型变量中的 值和值的类型(这是一对儿, pair)
   首先, 我们需要了解 package reflect 中的 Type 和 Value, 这两种类型:
   要访问接口变量的内容, 给了两个 type类型, 和 两个 func函数, 叫做 reflect.TypeOf 和 reflect.ValueOf, 从接口值中取出值用 reflect.Type 和 reflect.Value
   (另外, 从一个 reflect.Value 很容易得到相应的 reflect.Type, 但现在让我们将 Value(Value是一种接口类型) 和 Type(Type是一种接口类型) 概念分开.)

```go
type Type(大写 Type 是一种接口类型)
type Type interface {
...
}
Type(Type是一种接口类型) 是 Go 类型的表示形式.
并非所有方法都适用于所有类型的类型. 每个方法的文档中都注明了限制(如果有).
在调用特定于类型的方法之前, 请使用 Kind() 方法找出类型的种类. 
调用不适合该类型的方法会导致运行时恐慌panic.
"Type values 类型的值(Type是一种接口类型)" 是可比较的, 例如使用 == 运算符, 因此它们可以用作 map 的 keys. 如果两个 "Type values 类型的值(Type是一种接口类型)" 表示相同的类型，则它们相等.

type Value(大写 Value 是一种接口类型)
type Value struct {
	// contains filtered or unexported fields
}
Value(Value是一种接口类型) 是 Go 值的反射接口.
并非所有方法都适用于所有类型的值. 每种方法的文档中都会注明限制条件(如果有).
在调用特定于种类的方法之前, 使用 Kind() 方法找出值的种类. 调用不适合该类型的方法会导致运行时恐慌panic.
零 Value(Value是一种接口类型) 表示没有值. 它的 IsValid() 方法返回 false, 它的 Kind() 方法返回 Invalid, 它的 String() 方法返回 "<invalid Value>", 所有其他方法都恐慌panic.
大多数函数和方法从不返回无效值. 如果是这样, 它的文档会明确说明条件.
一个 Value(Value是一种接口类型) 可以被多个 goroutine 同时使用, 前提是底层的 Go 值可以同时用于等效的直接操作.
要比较两个 Value(Value是一种接口类型), 请比较 Interface() 方法的结果. 对两个 Value(Value是一种接口类型)使用 == 不会比较它们所代表的底层值.

func: 
    func reflect.TypeOf: 
    func TypeOf(i any) Type
    TypeOf 返回反射 Type(Type是一种接口类型), 这个反射 Type(Type是一种接口类型) 代表的是 i 的动态类型. 如果 i 是 nil 接口值, 那么 TypeOf 返回 nil.

    func reflect.ValueOf: 
    func ValueOf(i any) Value
    ValueOf 返回一个新 Value(Value是一种接口类型), 这个新 Value(Value是一种接口类型)初始化, 成为具体的值, 这个具体值存储在这个接口i中. ValueOf(nil) 返回零 Value(Value是一种接口类型)
```

让我们从 TypeOf 开始:
package main

import (
    "fmt"
    "reflect"
)

func main() {
    var x float64 = 3.4
    fmt.Println("type:", reflect.TypeOf(x))
}
此程序打印 
type: float64

您可能想知道接口在这里的位置, 因为程序看起来像是传递了 float64 类型的变量 x, 而不是一个接口值, 给 reflect.TypeOf(). 但它就在那里; 正如 godoc reports https://go.dev/pkg/reflect/#TypeOf，reflect.TypeOf() 签名 包含一个空接口:
// TypeOf returns the reflection Type of the value in the interface{}.
func TypeOf(i interface{}) Type
当我们调用 reflect.TypeOf(x) 时, x 首先被存储在一个空接口中, 然后作为参数传递; reflect.TypeOf 解包该空接口以恢复类型信息.

当然，reflect.ValueOf() 函数, 会恢复值(从这里开始, 我们将省略样板文件, 只关注可执行代码): 
var x float64 = 3.4
fmt.Println("value:", reflect.ValueOf(x).String())
打印
value: <float64 Value>
(我们 显式explicitly 调用 String() 方法, 因为默认情况下, fmt 包 会挖掘 reflect.Value 以显示内部的具体值. String() 方法不会.)

reflect.Type 和 reflect.Value, 两者都反映了这一点. 有很多方法可以让我们检查和操纵它们.
一个重要示例是 Value(Value是一种接口类型) 具有一个 Type(Type是一种接口类型) 方法，该方法返回 一个 reflect.Value 的 Type(Type是一种接口类型).
另一个是 Type(Type是一种接口类型) 和 Value(Value是一种接口类型) 都有一个 Kind() 方法, 该方法返回一个常量, 指示存储的项目类型: Uint、Float64、Slice 等.
此外, 具有 Int 和 Float 等名称的 Value(Value是一种接口类型) 方法允许我们获取存储在其中的值(如 int64 和 float64):

var x float64 = 3.4
v := reflect.ValueOf(x)
fmt.Println("type:", v.Type())
fmt.Println("kind is float64:", v.Kind() == reflect.Float64)
fmt.Println("value:", v.Float())

打印

type: float64
kind is float64: true
value: 3.4
还有像 SetInt 和 SetFloat 这样的方法, 但要使用它们, 我们需要理解可置性, 这是反射第三定律的主题, 下面讨论.

反射库有几个属性值得单独挑选.
首先, 为了保持 API 的简单性, Value(Value是一种接口类型) 的 "getter" 和 "setter" 方法对可以保存值的最大类型进行操作:
例如, 对于所有有符号整数, int64. 也就是说, Value(Value是一种接口类型) 的 Int() 方法返回 int64, SetInt 值返回 int64; 
可能需要转换为所涉及的实际类型:

var x uint8 = 'x'
v := reflect.ValueOf(x)
fmt.Println("type:", v.Type())                            // uint8.
fmt.Println("kind is uint8: ", v.Kind() == reflect.Uint8) // true.
x = uint8(v.Uint())                                       // v.Uint returns a uint64.

第二个属性是, 反射对象的 Kind() 描述基础底层类型, 而不是静态类型.
如果反射对象包含用户定义的整数类型的值, 如 

type MyInt int
var x MyInt = 7
v := reflect.ValueOf(x)

v 的 Kind() 仍然 reflect.Int, 即使 x 的静态类型是 MyInt, 而不是 int. 换句话说, Kind() 不能将 int 与 MyInt 区分开来, 即使 Type(Type是一种接口类型) 可以.

#### The second law of reflection, 反射第二定律 
2. Reflection goes from reflection object to interface value.
2. 反射从反射对象到接口值.
像物理反射一样, Go 中的反射会产生自己的逆向. 

给出一个 reflect.Value 我们可以使用 接口方法 恢复接口值; 实际上, 该方法将 类型 和 值 信息打包回接口表示形式并返回结果: 

// Interface returns v's value as an interface{}.
func (v Value) Interface() interface{}

因此, 我们可以说 

y := v.Interface().(float64) // y will have type float64.
fmt.Println(y)
打印 由反射对象 v 表示的 float64类型的值

不过, 我们可以做得更好.
fmt.Println, fmt.Printf 等的参数都作为空接口值传递, 然后由 fmt 包在内部解压缩, 就像我们在前面的示例中所做的那样. 
因此, 打印 reflect.Value 的内容需要全部. 正确的值是将 Interface() 方法的结果传递给格式化的打印例程: 
fmt.Println(v.Interface())
(由于本文是首次编写的, 因此对 fmt 包进行了更改, 以便它自动解压缩 reflect.Value 像这样, 所以我们可以说 
fmt.Println(v)
对于相同的结果, 但为了清楚起见, 我们将保留 .Interface() 调用这里. 

由于我们的值是 float64, 因此如果需要, 我们甚至可以使用浮点格式:
 fmt.Printf("value is %7.1e\n", v.Interface())
并得到在这种情况下
3.4e+00 
同样, 没有必要将 v.Interface() 的结果类型断言为 float64; 空接口值内部有具体值的类型信息, Printf 将恢复它.
简而言之, Interface() 方法是 ValueOf() 函数的反函数, 只不过其结果始终是静态类型的 interface{}.
重申: 反射从接口值到反射对象, 然后再返回.


#### The third law of reflection, 第三反射定律
3. To modify a reflection object, the value must be settable.
3. 要修改反射对象, 该值必须是可设置的.
第三定律是最微妙和最令人困惑的, 但如果我们从第一性原理开始, 就很容易理解.
这里有一些代码不起作用, 但值得研究.
var x float64 = 3.4
v := reflect.ValueOf(x)
v.SetFloat(7.1) // Error: will panic.
如果运行此代码，它将死机并显示神秘的消息 

panic: reflect.Value.SetFloat using unaddressable value
问题不在于值 7.1 是不可寻址的; 而是 v 是不可设置的.
可设置性是反射值的属性, 并非所有反射值都具有它.

Value(Value是一种接口类型)的 CanSet() 方法报告 Value(Value是一种接口类型)的可设置性; 在我们的例子中, 
var x float64 = 3.4
v := reflect.ValueOf(x)
fmt.Println("settability of v:", v.CanSet())

打印

settability of v: false
对不可设置的 Value(Value是一种接口类型)调用 Set() 方法是一个错误. 但什么是可设定性?

可设置性有点像可寻址性, 但更严格. 
它是反射对象可以修改用于创建反射对象的实际存储的属性. 可设置性取决于反射对象是否包含原始项目. 当我们说
var x float64 = 3.4
v := reflect.ValueOf(x)
我们传递 x 的副本给 reflect.ValueOf, 因此创建的接口值作为参数 reflect.ValueOf 是 x 的副本, 而不是 x 本身.
因此, 如果语句 
 v.SetFloat(7.1)
允许成功, 它不会更新 x, 即使 v 看起来像是从 x 创建的.
相反, 它将更新存储在反射值内的x的副本, 并且x本身不受影响.
这将是令人困惑和无用的, 所以它是非法的, 可设置性是用来避免这个问题的属性.

如果这看起来很奇怪, 那就不是.
这实际上是一个熟悉的情况, 穿着不寻常的服装.考虑将 x 传递给函数: 
f(x)
我们不希望 f 能够修改 x, 因为我们传递了 x 值的副本, 而不是 x 本身.
如果我们希望 f 直接修改 x, 我们必须将函数的地址传递给 x(即指向 x 的指针):
f(&x)
这是简单而熟悉的, 反射的工作方式相同.
如果我们想通过反射来修改 x, 我们必须给反射库一个指向我们要修改的值的指针.

让我们这样做. 首先, 我们像往常一样初始化 x, 然后创建一个指向它的反射值, 称为p
var x float64 = 3.4
p := reflect.ValueOf(&x) // Note: take the address of x.
fmt.Println("type of p:", p.Type())
fmt.Println("settability of p:", p.CanSet())

到目前为止的输出是 
type of p: *float64
settability of p: false

反射对象 p 是不可设置的, 但它不是我们想要设置的 p, 它是(实际上)*p.
为了得到 p 指向的内容, 我们调用 Value(Value是一种接口类型) 的 Elem() 方法, 该方法通过指针间接, 并将结果保存在名为 v 的反射Value(Value是一种接口类型)中:
v := p.Elem()
fmt.Println("settability of v:", v.CanSet())
现在 v 是一个可设置的反射对象, 如输出所示, 
settability of v: true
并且由于它表示 x, 我们终于可以使用 v.SetFloat 来修改 x 的值:
v.SetFloat(7.1)
fmt.Println(v.Interface())
fmt.Println(x)
如预期的那样, 输出为 
7.1 
7.1 
反射可能很难理解, 但它正在做语言所做的尽管通过反射Type(Type是一种接口类型)和Value(Value是一种接口类型)可以掩盖正在发生的事情. 请记住, 反射Value(Value是一种接口类型)需要某些内容的地址才能修改它们所表示的内容.

#### Structs,结构
在我们之前的例子中, v 本身并不是一个指针, 它只是从一个指针派生而来.
出现这种情况的常见方法是使用反射来修改结构的字段. 
只要我们有结构的地址, 我们就可以修改它的字段.

这是一个分析结构值 t 的简单示例.
我们使用结构的地址创建反射对象, 因为我们稍后会修改它.
然后我们将 typeOfT 设置为其类型, 并使用简单的方法调用迭代字段(有关详细信息，请参阅包反射).
请注意, 我们从 struct 类型中提取字段的名称, 但字段本身是常规的 reflect.Value 对象.
type T struct {
    A int
    B string
}
t := T{23, "skidoo"}
s := reflect.ValueOf(&t).Elem()
typeOfT := s.Type()
for i := 0; i < s.NumField(); i++ {
    f := s.Field(i)
    fmt.Printf("%d: %s %s = %v\n", i,
        typeOfT.Field(i).Name, f.Type(), f.Interface())
}
这个程序的输出是
0: A int = 23
1: B string = skidoo
这里顺便介绍了一点关于可设置性: T 的字段名称是大写的(导出的), 因为只有结构的导出字段是可设置的.
因为 s 包含一个可设置的反射对象, 所以我们可以修改结构体的字段.
s.Field(0).SetInt(77)
s.Field(1).SetString("Sunset Strip")
fmt.Println("t is now", t)
结果如下:
t is now {77 Sunset Strip}
如果我们修改程序以便从 t 创建 s, 而不是 &t, 则对 SetInt 和 SetString 的调用将失败, 因为 t 的字段不可设置.

#### Conclusion, 结论
这里又是反射定律:
. 反射从接口值到反射对象.
. 反射从反射对象到接口值.
. 要修改反射对象, 该值必须是可设置的.
一旦你理解了这些定律, Go 中的反射就会变得更容易使用, 尽管它仍然很微妙.
这是一个强大的工具, 除非绝对必要, 否则应谨慎使用并避免使用.
还有很多我们没有涉及到的反射 —— 在通道上发送和接收、分配内存、使用切片和映射、调用方法和函数 —— 但这篇文章已经足够长了.
我们将在后面的文章中介绍其中一些主题.



## FAQ & troubleshooting





## See Also

https://pkg.go.dev/reflect

[ The Laws of Reflection ]:(https://go.dev/blog/laws-of-reflection)
