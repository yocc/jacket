# go json

----

[toc]



## Overview

1. 能 encode/decode

   1. encoding/json是官方提供的标准json, 实现RFC 7159中定义的JSON编码和解码。使用的时候需要预定义struct，原理是通过reflection和interface来完成工作, 性能低。

   2. easyjson, ffjson, 标准库性能的瓶颈在反射。easyjson, ffjson 并没有使用反射方式实现，而是在Go中为结构体生成静态MarshalJSON和UnmarshalJSON函数，类似于预编译。调用编码解码时直接使用生成的函数，从而减少了对反射的依赖，所以通常快2到3倍。但相比标准JSON包，使用起来略为繁琐。

   3. json-iterator/go, 这是一个很神奇的库，滴滴开发的，不像 easyjson 和 ffjson 都使用了预编译，而且 100% 兼容encoding/json的高性能json库。根据作者的压测介绍，该库比easyjson性能还要好那么一点点（有点怀疑~）。json-iterator使用modern-go/reflect2来优化反射性能。然后就是通过大幅度减少反射操作，来提高速度。

2. 只能 decode

   4. go-simplejson, gabs, jason, 这几个包都是在encoding/json的基础上进行开发的，为了是更方便的操作JSON：它不需要创建struct，而是动态按字段取内容。它们大部分只是一个解析库，并没有序列化的接口。有时候我们仅仅想取JSON里的某个字段，用这个非常有用。

   5. jsonparser, jsonparser 功能与go-simplejson类似，只是一个解析库，并没有序列化的接口。但是由于底层不是基于encoding/json开发的，官方宣称它比encoding/json快10倍。



## encoding/json

### Overview

```go
// https://pkg.go.dev/encoding/json
概述 ¶
json 包实现了 RFC 7159 (https://www.rfc-editor.org/rfc/rfc7159.html) 中定义的 JSON 编码和解码. 
JSON 和 Go 值之间的映射在 Marshal 和 Unmarshal 函数的文档中进行了描述.
有关此包的介绍, 请参阅 "JSON and Go": https://golang.org/doc/articles/json_and_go.html
```

### Encoding

```go
// func Marshal(v interface{}) ([]byte, error)
type Message struct {
    Name string
    Body string
    Time int64
}
m := Message{"Alice", "Hello", 1294706395881547000}
b, err := json.Marshal(m)
if err != nil {
		return nil, err
}
if err != nil {
		log.Fatal(err)
}
// b == []byte(`{"Name":"Alice","Body":"Hello","Time":1294706395881547000}`)
```

### Outline

```go
只有可以表示为有效 JSON 的数据结构才会被编码:
. JSON 对象仅支持字符串作为键; 要对 Go 映射类型进行编码, 它必须是 map[string]T 格式 (其中 T 是 json 包支持的任何 Go 类型)
. 通道、复数和函数类型无法编码.
. 不支持循环数据结构; 它们将导致 Marshal 进入无限循环.
. 指针将被编码为它们指向的值(如果指针为 nil, 则为"null")
json 包 仅访问结构类型(以大写字母开头的字段)的导出字段. 因此, JSON 输出中将仅显示结构的导出字段.
```

### Decoding

```go
// func Unmarshal(data []byte, v interface{}) error
type Message struct {
    Name string
    Body string
    Time int64
}
var m Message
err := json.Unmarshal(b, &m)
/*
m = Message{
    Name: "Alice",
    Body: "Hello",
    Time: 1294706395881547000,
}
*/

```

```go
Unmarshal 如何识别存储解码数据的字段? 对于给定的 JSON 键 "Foo", Unmarshal 将查看目标结构的字段以查找(按优先顺序):
. 先看 tag; 带有 "Foo" 标签的导出字段(有关结构标签的更多信息，请参见 Go 规范)
. 再看 field; 名为 "Foo" 的导出字段, 或
. 在看 大小写; 名为 "FOO" 或 "FoO" 的导出字段或 "Foo" 的其他一些不区分大小写的匹配项

当 JSON 数据的结构与 Go 类型不完全匹配时会发生什么?
b := []byte(`{"Name":"Bob","Food":"Pickle"}`)
var m Message
err := json.Unmarshal(b, &m)
Unmarshal 将仅解码它可以在目标类型(结构体中定义的)中找到的字段.
在这种情况下, 只会填充 m 的 Name 字段, 而忽略 Food 字段.
当您希望从大型 JSON blob 中仅选择几个特定字段时, 此行为特别有用.
这也意味着目标结构中任何未导出的字段都不会受到 Unmarshal 的影响.
```

### Generic JSON with interface

```go
但是，如果您事先不知道 JSON 数据的结构怎么办？
interface{} (空接口)类型描述了一个具有零个方法的接口. 每个 Go 类型至少实现零个方法, 因此满足空接口.
空接口用作通用容器类型:
var i interface{}
i = "a string"
i = 2011
i = 2.777
类型断言访问底层的具体类型:
r := i.(float64)
fmt.Println("the circle's area", math.Pi*r*r)
或者, 如果底层类型未知, 则类型开关确定类型:
switch v := i.(type) {
case int:
    fmt.Println("twice i is", v*2)
case float64:
    fmt.Println("the reciprocal of i is", 1/v)
case string:
    h := len(v) / 2
    fmt.Println("i swapped by halves is", v[h:]+v[:h])
default:
    // i isn't one of the types above
}
json 包使用 map[string]interface{} 和 []interface{} 值来存储任意 JSON 对象和数组; 
它会很高兴地将任何有效的 JSON blob 解码为一个普通的 interface{} 值. 默认的具体 Go 类型是:
. bool for JSON booleans,
. float64 for JSON numbers,
. string for JSON strings, and
. nil for JSON null.
```

### Decoding arbitrary data,  解码具体数据

```go
思考一下这个存储在变量 b 中的 JSON 数据:
b := []byte(`{"Name":"Wednesday","Age":6,"Parents":["Gomez","Morticia"]}`)
不用管这个数据的结构, 我们也能解码数据到 interface{} 类型的值里:
var f interface{}
err := json.Unmarshal(b, &f)
f 变量的地址上保存了一个 map, 这个 map 的 keys 是 strings, 这个 map 的 values 本身保存的值 是 空接口:
f = map[string]interface{}{
    "Name": "Wednesday",
    "Age":  6,
    "Parents": []interface{}{
        "Gomez",
        "Morticia",
    },
}

要访问这些数据, 我们可以使用类型断言来访问 f 的底层 map[string]interface{}:
m := f.(map[string]interface{})

然后我们可以使用 range 语句遍历 map 并使用 type switch (类型开关) 来访问其 值 作为它们的 具体类型:
for k, v := range m {
    switch vv := v.(type) {
    case string:
        fmt.Println(k, "is string", vv)
    case float64:
        fmt.Println(k, "is float64", vv)
    case []interface{}:
        fmt.Println(k, "is an array:")
        for i, u := range vv {
            fmt.Println(i, u)
        }
    default:
        fmt.Println(k, "is of a type I don't know how to handle")
    }
}
通过这种方式, 您可以处理未知的 JSON 数据, 同时仍然享受类型安全的好处.
```

### Reference Types, 引用类型(pointers, slices, maps)

```go
定义一个 结构体 容纳前面的数据:
type FamilyMember struct {
    Name    string
    Age     int
    Parents []string
}
var m FamilyMember
err := json.Unmarshal(b, &m)

将这些数据解码为 FamilyMember 值按预期工作, 但如果我们仔细观察, 我们会发现发生了一件了不起的事情.
使用 var 语句, 我们分配了一个 FamilyMember 结构, 然后将指向该值的指针提供给 Unmarshal, 但当时 Parents 字段是一个 nil 切片值.
为了填充 Parents 字段, Unmarshal 在幕后分配了一个新切片.
这是 Unmarshal 与支持的引用类型(指针、切片和映射)一起工作的典型方式.

思考解码到这个数据结构中:
type Foo struct {
    Bar *Bar
}
如果 JSON 对象中有一个 Bar 字段, Unmarshal 将分配一个新的 Bar 并填充它.
如果不是, Bar 将被保留为 nil 指针.
由此产生了一个有用的模式: 如果您有一个接收几种不同消息类型的应用程序, 您可以定义 "接收器" 结构, 如:
type IncomingMessage struct {
    Cmd *Command
    Msg *Message
}
并且发送方可以填充顶级 JSON 对象的 Cmd 字段和/或 Msg 字段, 具体取决于他们想要传达的消息类型.
Unmarshal 在将 JSON 解码为 IncomingMessage struct 时, 只会分配 JSON 数据中存在的数据结构.
要知道要处理哪些消息, 程序员只需测试 Cmd 或 Msg 是否不为 nil.
```

### Streaming Encoders and Decoders

```go
json 包 提供了 Decoder 和 Encoder 类型来支持 JSON 数据流读写的常用操作.
NewDecoder 和 NewEncoder 函数包装了 io.Reader 和 io.Writer 接口类型. 
func NewDecoder(r io.Reader) *Decoder
func NewEncoder(w io.Writer) *Encoder
下面是一个示例程序, 它从标准输入读取一系列 JSON 对象, 从每个对象中删除除 Name 字段之外的所有对象, 然后将对象写入标准输出:
package main
import (
    "encoding/json"
    "log"
    "os"
)
func main() {
    dec := json.NewDecoder(os.Stdin)
    enc := json.NewEncoder(os.Stdout)
    for {
        var v map[string]interface{}
        if err := dec.Decode(&v); err != nil {
            log.Println(err)
            return
        }
        for k := range v {
            if k != "Name" {
                delete(v, k)
            }
        }
        if err := enc.Encode(&v); err != nil {
            log.Println(err)
        }
    }
}
由于 Readers 和 Writers 无处不在, 这些 Encoder 和 Decoder 类型可用于广泛的场景, 例如 读取 和 写入 HTTP 连接、WebSockets 或文件.
```



## easyjson, ffjson

```go
// https://github.com/mailru/easyjson/
包 easyjson 提供了一种快速简便的方法来编组/解组 Go 结构到/从 JSON 而不使用反射reflection. 在性能测试中, easyjson 比标准 encoding/json 包的性能高出 4-5 倍, 其他 JSON 编码包的性能高出 2-3 倍.
easyjson 旨在针对性的保持生成的 Go 代码足够简单, 以便可以轻松地对其进行优化或修复. 另一个目标是通过提供标准 encoding/json 包不可用的选项, 例如生成 "snake_case" 名称或默认启用 omitempty省略 行为, 使用户能够自定义生成的代码.

Usage
# install
# for Go >= 1.17
go get github.com/mailru/easyjson && go install github.com/mailru/easyjson/...@latest		// 要在含 go.mod 目录里
# run
easyjson -all <file>.go
以上将生成 <file>_easyjson.go, 其中包含 <file>.go 中包含的所有结构的适当编组器和解组器函数.
请注意, easyjson 需要完整的 Go 构建环境并设置 GOPATH 环境变量. 
这是因为 easyjson 代码生成调用运行在一个临时文件上(一种从 ffjson 借来的代码生成方法)

Serialize
someStruct := &SomeStruct{Field1: "val1", Field2: "val2"}
rawBytes, err := easyjson.Marshal(someStruct)

Deserialize
someStruct := &SomeStruct{}
err := easyjson.Unmarshal(rawBytes, someStruct)

Options
. 使用 -all 将为文件中的所有 Go 结构生成 编组器/解组器, 不包括前面注释以 easyjson:skip 开头的那些结构. 例如: 
//easyjson:skip
type A struct {}

. 如果未提供 -all, 则只有前面注释以 easyjson:json 开头的结构才会生成编组器/解组器. 例如:
// easyjson:json
type A struct {}
注: // easyjson:json, // 和 e 之间可以有空格或者没有空格; easyjson:json, :冒号后面和 json之间不能有空格; 只能是easyjson:json, 不能是[包名:json], 这种写法官方文档里并未提及, 实验也未通过

附加选项说明:
-snake_case 告诉 easyjson 默认生成 snake_case 字段名称(除非被字段标签覆盖). CamelCase 到 snake_case 的转换算法应该适用于大多数情况(即 HTTPVersion 将被转换为 "http_version")
-build_tags 将指定的构建标签添加到生成的 Go 源代码中.
-gen_build_flags 将执行 easyjson 引导代码以使用提供的标志启动实际的生成器命令. 多个参数应该用空格分隔, 例如-gen_build_flags="-mod=mod -x"

Structure json tag options, 结构 json 标签选项
除了像 "omitempty" 这样的标准 json 标签选项之外, 还支持以下内容:
. 'nocopy' - 禁用字符串值的分配和复制, 使它们引用原始 json 缓冲内存. 这对于解码和立即使用后未保存在内存中的短期对象非常有用. 请注意, 如果字符串需要取消转义, 它将照常处理.
. 'intern' - 字符串 "interning"(重复数据删除)以在整个结构中经常遇到相同的字符串字典值时节省内存. 请参阅下面的更多细节.

Generated Marshaler/Unmarshaler Funcs, 生成的 Marshaler/Unmarshaler 函数
对于 Go 结构类型, easyjson 生成函数 MarshalEasyJSON / UnmarshalEasyJSON 用于编组/解组 JSON.
反过来, 它们满足了 easyjson.Marshaler 和 easyjson.Unmarshaler 接口, 并且当与 easyjson.Marshal / easyjson.Unmarshal 结合使用时, 可以避免在 Go 结构的 JSON 编组/解组期间不必要的反射/类型断言.
easyjson 还为与标准 json.Marshaler 和 json.Unmarshaler 接口兼容的 Go 结构类型生成 MarshalJSON 和 UnmarshalJSON 函数.
请注意, 与使用 easyjson.Marshal / easyjson.Unmarshal 相比, 使用标准 json.Marshal / json.Unmarshal 进行编组/解组将导致显着的性能损失.
此外, easyjson 公开了使用 MarshalEasyJSON 和 UnmarshalEasyJSON 对标准读取器和写入器进行编组/解组的实用函数.
例如, easyjson 提供了easyjson.MarshalToHTTPResponseWriter, 它可以编组到标准的http.ResponseWriter. 请参阅 GoDoc 列表以获取可用的实用函数的完整列表.

控制 easyjson 编组和解组行为, Controlling easyjson Marshaling and Unmarshaling Behavior
Go 类型可以提供自己的 MarshalEasyJSON 和 UnmarshalEasyJSON 函数来满足 easyjson.Marshaler / easyjson.Unmarshaler 接口. 当为 Go 类型定义时，这些将由 easyjson.Marshal 和 easyjson.Unmarshal 使用.
Go 类型也可以满足 easyjson.Optional 接口, 它允许类型定义自己的 omitempty省略 逻辑.

Type Wrappers, 类型包装器
easyjson 提供了在 easyjson/opt 包中定义的附加类型包装器. 这些包装了标准的 Go 原语，进而满足了 easyjson 接口.
当需要区分缺失值和/或需要指定默认值时, easyjson/opt 类型包装器很有用. 
类型包装器允许 easyjson 避免额外的指针和堆分配, 并且在正确使用时可以显着提高性能.

Memory Pooling, 内存池
easyjson 使用一个缓冲池, 它以从 128 到 32768 字节的递增块分配数据.
512 字节和更大的块将在 sync.Pool 的帮助下被重用.
块的最大大小是有界的, 以减少冗余内存分配并允许更大的可重用缓冲区.
easyjson 的自定义分配缓冲池在 easyjson/buffer 包中定义, 默认行为池行为可以通过在任何封送或解组之前调用 buffer.Init() 来修改(如果需要).请参阅 GoDoc 列表以获取更多信息.

String interning, 字符串暂留|驻留, https://en.wikipedia.org/wiki/String_interning
在解组期间, 可以选择性地对字符串字段值进行实习, 以通过对内存中的字符串进行重复数据删除来减少内存分配和使用, 但代价是 CPU 使用率略有增加.
这仅对经常具有相同值的正在解码的字符串字段有效(例如, 如果您有一个只能假设少量可能值的字符串字段).
要启用 字符串暂留, 请将 intern 关键字标签添加到 string 字段上的 json 标签, 例如:
type Foo struct {
  UUID  string `json:"uuid"`         // will not be interned during unmarshaling
  State string `json:"state,intern"` // will be interned during unmarshaling
}
```

```go
easyjson vs. ffjson
easyjson 使用与 ffjson 相同的 JSON 编组方法, 但在解组期间对 JSON 进行词法分析和解析时采用了截然不同的方法.
这意味着 easyjson 的解组速度大约快 2-3 倍, 非并发解组速度快 1.5-2 倍.
在撰写本文时, ffjson 在并发使用时似乎存在问题: 具体而言, 大型请求池会损害 ffjson 的性能并导致可伸缩性问题.
ffjson 的这些问题可能会得到解决, 但在撰写本文时, ffjson 仍然存在未解决/已知的问题.
easyjson 和 ffjson 对于小请求具有相似的性能, 但是当与 writer 一起使用时, 对于大请求, easyjson 的性能大约是 ffjson 的 2-5 倍.
```



## go-simplejson, gabs, jason

```go
```



## jsonparser

```go
```



## Go and JSON

### JSON and Static Typing

Go 是 statically type 静态类型语言.
JSON 内容的类型多种多样, 编译器如何合理的根据不同类型分配内存?
那么对此会有两个答案:
#### 一. Parsing into a Struct, 解析进结构体
最简单的选择是, 已知JSON 的结构, 将 JSON 解析为您定义的结构体. 不在您定义的结构体范围内的字段都会被忽略.
伪代码, 将 序列化 JSON 串解码 进 结构体实例化:
type App struct {}          // 定义一个结构体
var app App                 // 实例化这个结构体
data := []byte(`JSON串`)    // 准备一个字节流 []byte
err := json.Unmarshal(字节流, 实例化地址)   // 解码 JSON
将结构体实例化中的数据 编码 成 JSON 串输出
data, err := json.Marshal(app)           // 编码 JSON

Struct Tags, 结构体标签
`json:"JSON串中字段名"` 形如, 此为结构体标签
结构体标签的作用是: 告诉 JSON 解析器 如何解析 该字段 和 该字段值 一些附加线索.

1. FIELD NAME, 字段名称
`json:"JSON串中字段名"`
JSON 解释时, 结构体字段首字母大写, 但 JSON串 中字段并不是大写, 此时通过 `json:"JSON串中字段名"` 来告诉 JOSN解析器, 将那个JSON字段的值对应到结构体的该字段上.
type MyStruct struct {
    SomeField string `json:"some_field"`
}

2. FIELD IS EMPTY, 字段和字段值为空
`json:"JSON串中字段名,omitempty"`
"omitempty" 标记是告诉 JSON解析器当字段的值(空字符串"", 0, false, null)为空时, 忽略整个字段和字段值的输出, 即, {"some_field": ""} 变为 {}

3. SKIPPING FIELDS, 跳过字段解析
`json:"-"`
"-" 标记是告诉 JSON解析器 跳过该字段的解析

Nested Fields, 嵌套字段
我想说的是, 实例化的是最外层结构体, 里面嵌套的子结构体是可以不用逐一实例化的, 而是直接用最外层实例化加JSON字段来直接访问的.

Pointers
在 JSON编码时, 指针被解引用. 这么做的意义是就好像您使用的是真实值, 而不是指针. [当您将解引用指针的结果分配给新变量时, 该变量将获取其指向的值的副本.????困惑]

Handling Errors, 处理错误
请务必检查 Marshal() 和 Unmarshal() 返回的 err 参数. 例如, 如果您尝试编码包含 nil 指针的内容.
使用 MustMarshal() 将错误转换为 panic()

#### 二. Parsing into an Interface, 解析进接口
如果您在解析JSON串前并不知道JSON的结构, 那么您可以把 JSON串解析进一个通用 interface{}
空 interface{} 在 Go 中, 将变量定义为 "这可能是任何东西" 的办法
在 runtime 运行时, Go 将分配适当的内存, 以适应您决定存储在其中的任何内容

```go
伪代码:
var parsed map[string]interface{}       // 有个前置条件, 开头得知道是数组还是对象, 然后依次声明实例化
data := []byte(`JSON串`)                // 准备字节流
err := json.Unmarshal(data, &parsed)    // 解析进实例化
idString := parsed["id"].(string)       // 通过 类型动态转换/查询 来访问内存中字段
```

对象: map[string]interface{}
数组: []interface{}

访问时:
idString := parsed["id"].(string)       // 类型动态转换/查询

UseNumber(), http://golang.org/src/pkg/encoding/json/stream.go?s=720:751#L22
通过 类型动态转换/查询 只能是 float64, 如果必须需要直接是 int 的话, 可以使用 UseNumber() 方法

```go
类型动态转换/查询:
bool,                           for JSON booleans
float64,                        for JSON numbers
string,                         for JSON strings
[]interface{},                  for JSON arrays
map[string]interface{},         for JSON objects
nil,                            for JSON null
```

```go
inj.go 文件, 举例了, 如果通过 interface{} 的方式实现多级对象的解析和访问.
package main

import (
    "encoding/json"
    "fmt"
    "io"
    "log"
    "net/http"
)

func main() {

    du := "https://super.sports.sina.cn/players/api/various/datum?league=cba_1&id=4400"
    resp, err := http.Get(du)
    if err != nil {
        log.Fatal(err)
    }
    defer resp.Body.Close()
    
    fmt.Printf("resp.Status:            %T, %s\n", resp.Status, resp.Status)
    fmt.Printf("resp.Body:              %T, %#v\n", resp.Body, resp.Body)
    fmt.Printf("resp.Close:             %T, %t\n", resp.Close, resp.Close)
    body, err := io.ReadAll(resp.Body) // 返回字节流
    if err != nil {
        log.Fatal(err)
    }
    
    var res map[string]interface{}
    err1 := json.Unmarshal(body, &res)
    if err1 != nil {
        log.Fatal(err1)
    }
    //fmt.Printf("%T, %+v", res, res)
    msg := res["result"].(map[string]interface{})["status"].(map[string]interface{})["msg"].(string)
    //stt := msg["status"].(map[string]interface{})
    fmt.Println(msg)
    // fmt.Println(stt)
} 
```



### Numbers, 数字

在数字处理上, 我认为如果为了对端上友好和通用, 那么将数字, 小数, 超大数(不同语言可能越界溢出)等都转成 string 输出即可.
Int64String int64 `json:",string"`


### Creating Encodable Types, 创建可编码类型

### Deferred Decoding, 延迟解码
延迟解码是针对某些 JSON串 的结构是有上下文关联的情况, 比如当A字段为true时, B字段是对象, 当A字段为false时, B字段是数组, 此时需要用到延迟解码.
RawMessages 结构不会立即解码, http://golang.org/pkg/encoding/json/#RawMessage, 允许你在部分解析内容准备好后在执行另一部分解析.

### Conclusion, 结论
没什么可总结的

### 可见性
大写首字母结构体
大写首字母结构体中字段

## FAQ & troubleshooting



## See Also

[easyjson]:(https://github.com/mailru/easyjson)
[jsonparser]: (https://github.com/buger/jsonparser)
[Speeding Up JSON]: (https://yalantis.com/blog/speed-up-json-encoding-decoding/)







