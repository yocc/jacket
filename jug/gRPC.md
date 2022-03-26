# gRPC

---

[toc]



## Overview

### Introduction

- gRPC 是现代的开源远程过程调用 (RPC) 框架

- gRPC 用 protocol buffers 作为 接口定义语言 IDL 并且还作为底层消息交换格式.

- gRPC 是基于定义服务的思想, 即指定方法名, 参数和返回类型. 

  服务端一侧: 实现这个接口, 并处理客户端请求进来的调用.

  客户端一侧: 有一个存根(stub), 这个存根提供了与服务器端相同的方法.

- Protocol Buffers, 序列化结构化数据.

  第一步是在 proto 文件(扩展名为 .proto 的普通文本文件)中定义将要序列化的数据的结构. 

  数据就是消息本身, 其中每条消息都是包含一系列称为字段的名称-值对的信息的小逻辑记录.

  ```c++
  message Person {
    string name = 1;
    int32 id = 2;
    bool has_ponycopter = 3;
  }
  ```

  protoc 是 protocol buffer 编译器(不同语言用不同的 protoc 编译器), 用 protoc 生成数据访问类, 即 Person Class

- gRPC plugin + protoc + proto file => client code, server code

-   尽量使用 proto3 而不是 proto2



### Core concepts 体系和生命周期

gRPC 服务(service)允许您定义四种服务(service)方法:

. 一元(unary) RPCs: client 发送一个单一请求给服务器(server), 然后得到一个单一响应返回, 就像普通的函数调用.

```shell
rpc SayHello(HelloRequest) returns (HelloResponse);
```

. 服务器流式 RPCs: client 发送一个请求给服务器server, 然后得到一个流, 从流里读出返回的消息序列. 

 客户端一直从返回的流里读消息, 直到消息全部读完为止. 在一个RPC调用ID里gRPC保证了消息的顺序.
```shell
rpc LotsOfReplies(HelloRequest) returns (stream HelloResponse);
```

. 客户端流式 RPCs: client 写一个消息序列, 同时发送给她的服务器server, 再次使用已经提供的流. 

 客户端每完成一次写消息(可以多条), 他都会等待服务器读取然后返回. 在一个RPC调用ID里gRPC保证了消息的顺序.
```shell
rpc LotsOfGreetings(stream HelloRequest) returns (HelloResponse);
```

. 双向流式 RPC: 两边即同时读消息流也同时写消息流. 

 两个流独立运作, 所以客户端和服务器都能用他们喜欢的 任何顺序读写: 

 比如, 服务器可以等待接收所有客户端消息然后再开始响应返回, 或者还能交替的读一条消息然后写一条消息, 或者其他读写组合.

 在每个流中的消息顺序是被持久化的.
```shell
rpc BidiHello(stream HelloRequest) returns (stream HelloResponse);
```

- 使用API

  来自在 .proto 文件中定义的一个服务service的流式, gRPC 提供 protocol buffer 编译器插件, 这个编译器插件用来生成 client 和 server 端的代码.

  gRPC 使用者在客户端上典型的调用这些 APIs 并且在服务端上执行对应的代码.

  - 在服务器端, 服务器执行的方法被服务service声明, 并且运行一个 gRPC 服务器来处理 client 调用.

     gRPC 基础设施对传入的请求进行解码时, 执行服务service方法和编码服务service响应.

    服务端, gRPC基础设施收到消息后, 先decode(request), 执行method(), encode(结果), return.

  - 在客户端, 客户端有一个本地对象称为存根(一些语言首选项是客户)这个存根作为服务service来执行相同的方法. 

     客户端可以调用这些在本地对象(存根)里的方法, 在合适的 protocol buffer 消息类型里为了调用而包装参数 - gRPC会照料发送给服务器的请求(s)和返回的服务器的 protocol buffer 响应(s)

- 同步与异步

  同步RPC调用阻塞, 直到一个响应从服务器到达客户端, 这个服务器被近似的抽象为一个过程调用, 而这就是RPC渴望要做的.

  另一方面, 网络在本质上是异步的, 在很多场景下是很有用的可以开始RPCs没有阻塞当前线程.

  gRPC编程API在大多数语言中有同步和异步特性. 你可以找到更多在每个语言的教程和参考文档(完整的参考文档很快)。

- RPC 生命周期

  在本节中, 你会仔细看看会发生什么当gRPC gRPC服务器客户端调用的方法. 完整的实现细节,请参阅特定于语言的页面.

- 一元RPC: 

  首先考虑最简单的RPC类型: 客户端发送一个单一请求, 然后得到一个单一响应.

  1. 一旦客户端调用一个存根方法, 服务器会被通知, 这个通知是对于这个调用RPC是被客户端的元数据调用的, 方法名, 和如果适用还有 deadline.

  2. 服务器要么立即发送回他自己最初的元数据(最初元数据是任何响应之前必须被发送的), 要么等待客户端的请求消息. 那个先发生, 是应用决定的.

  3. 一旦服务器收到客户端的请求的消息, 服务器就会干所有必要的事情, 创建并安装元件给一个响应. 

     这个响应然后被返回(如果成功)给客户端, 连同状态细节一起返回(状态码, 选项状态消息)并带上可选的追踪元数据.

  4. 如果响应状态是OK的, 那么客户端得到响应, 这就完成了在客户端调用.

- 服务器流式RPC:

  类似于一元RPC server-streaming RPC, 除了服务器返回一个流的消息在响应客户的请求. 

  发送所有的信息后, 服务器的状态细节(状态代码和可选的状态消息)和可选的尾随元数据被发送到客户机.

  这样就完成了服务器端的处理. 一旦客户端拥有了所有服务器消息, 它就完成了工作.

- 客户端流式RPC:

  客户端流RPC类似于一元RPC, 不同之处是客户端向服务器发送消息流而不是一单个条消息.

  服务器响应一条单个消息(以及它的状态详细信息和可选的尾随元数据)，通常情况下，但不一定是在它收到所有客户端消息之后。

- 双向流式RPC:

  在双向流RPC中，客户端发起最初的调用来调用方法, 然后服务端收到客户端的元数据, 方法名, 和deadline.

  服务器可以选择发送回它的初始元数据，或者等待客户端开始流媒体消息。

  客户机和服务器端流处理是特定于应用程序的。由于这两个流是独立的，客户端和服务器可以以任何顺序读写消息。

  例如,一个服务器可以等到它已经收到了客户的所有信息之前写它的消息, 或者是服务器和客户端可以玩“乒乓球”——服务器收到一个请求, 然后发回一个响应, 然后根据响应客户端发送另一个请求,等等。

- Deadline/超时

  gRPC允许客户端指定多长时间是客户端愿意等待RPC完成的时间, DEADLINE_EXCEEDED (由客户端指定).

  在服务器端，服务器可以查询特定RPC是否超时，或者完成RPC还剩下多少时间.

  指定deadline或超时是特定于语言的: 一些语言api按照超时(持续时间)来工作，而一些语言api按照deadline(固定的时间点)来工作，可能有也可能没有默认的deadline。

- RPC终止

  (客户端和服务端独立判断是否调用成功了, 所以两端结论可能不一致)

  在gRPC中，客户端和服务器对调用的成功是进行独立的本地判断，他们的结论可能不匹配。

  这意味着，例如，您可以有一个RPC，它在服务器端成功完成(“我已经发送了我的所有响应!”)，但在客户端失败(“响应在我的deadline之后到达!”)。

  服务器也可能在客户机发送所有请求之前决定完成。

- 取消一个RPC

  客户机或服务器可以在任何时候取消RPC。

  取消操作会立即终止RPC，因此不会再做任何工作。

  警告: 取消之前所做的更改不会回滚。

- 元数据

  元数据是给 gRPC 服务本身使用的, 和开发者的应用业务一般无关.

  元数据是一个实际RPC调用相关的信息, 比如认证信息, 是由键-值对列表的形式组成, 其中键是字符串, 值通常是字符串, 但也可以是二进制数据.

  元数据对于gRPC是可见的, 通过元数据可以把客户端和被调用的服务器端联系起来, 反之亦然. 对元数据的访问依赖于语言. 

- 通道

  gRPC通道提供到指定主机和端口上的gRPC服务器的连接.

  它在创建客户端存根时使用.

  客户端可以指定通道参数来修改gRPC的默认行为，比如打开或关闭消息压缩.

  通道有状态，包括连接状态和空闲状态. 

  gRPC如何处理关闭通道的问题取决于语言。一些语言还允许查询通道状态。

  

### FAQ

- 我可以在浏览器中使用它吗？
  gRPC-Web 项目已正式发布。

- 我可以将gRPC与我最喜欢的数据格式（JSON，Protobuf，Thrift，XML）一起使用吗？
  是的。gRPC 旨在可扩展以支持多种内容类型。初始版本包含对 Protobuf 的支持，以及对其他内容类型（如 FlatBuffers 和 Thrift）的外部支持，具有不同的成熟度。

- 是否可以在服务网格中使用 gRPC？
  是的。gRPC 应用程序可以像任何其他应用程序一样部署在服务网格中。gRPC 还支持 xDS API，支持在没有 sidecar 代理的服务网格中部署 gRPC 应用程序。此处列出了 gRPC 中支持的无代理服务网格功能。

- gRPC 如何帮助开发移动应用程序？
  gRPC 和 Protobuf 提供了一种简单的方法来精确定义服务，并为 iOS、Android 和提供后端的服务器自动生成可靠的客户端库。客户端可以利用高级流和连接功能，这些功能有助于节省带宽，通过更少的 TCP 连接执行更多操作，并节省 CPU 使用率和电池寿命。

- 为什么 gRPC 比 HTTP/2 上的任何二进制 blob 都好？
  这在很大程度上就是gRPC在网络上的内容。但是，gRPC 也是一组库，它们将跨平台一致地提供更高级别的功能，而常见的 HTTP 库通常不会这样做。此类功能的示例包括：
  - 在应用层与流量控制交互
  - 级联呼叫取消
  - 负载平衡和故障转移

- 为什么gRPC比REST更好/更差？
  gRPC 在很大程度上遵循 HTTP/2 上的 HTTP 语义，但我们明确允许全双工流式处理。我们偏离了典型的 REST 约定，因为我们在调用调度期间出于性能原因使用静态路径，因为从路径、查询参数和有效负载正文解析调用参数会增加延迟和复杂性。我们还正式确定了一组错误，我们认为这些错误比HTTP状态代码更直接地适用于API用例。



## Installation(错误, 废弃)

- import

	```golang
	import "google.golang.org/grpc"
	```

- go get

  ```shell
  $ go get -u google.golang.org/grpc
  ```

  > 从中国访问 grpc-go, [I/O Timeout Errors](https://github.com/grpc/grpc-go#FAQ)



## Compiler Installation, protoc

```go
// version3 版本的 protoc 编译器安装, 有两种方式:
1. 通过操作系统的包管理器安装(linux 用 apt, 有点怪,放弃)
$ apt install -y protobuf-compiler
$ protoc --version  # Ensure compiler version is 3+

2. 安装预编译好的二进制文件(任何操作系统都支持),
https://developers.google.com/protocol-buffers/docs/downloads#release-packages  最新发行
https://github.com/protocolbuffers/protobuf/releases

2.1. 手动下载
https://github.com/google/protobuf/releases, (protoc-<version>-<os><arch>.zip)
不要被干扰, protoc 并不区分语言, 而是和操作系统本身有关. 

$ PB_REL="https://github.com/protocolbuffers/protobuf/releases"
$ curl -LO $PB_REL/download/v3.15.8/protoc-3.15.8-linux-x86_64.zip

$ wget https://github.com/protocolbuffers/protobuf/releases/download/v3.19.4/protoc-3.19.4-linux-x86_64.zip

2.2. unzip 到 $HOME/.local, 或者 自定义目录下
$ unzip protoc-3.15.8-linux-x86_64.zip -d $HOME/.local
$ unzip protoc-3.19.4-linux-x86_64.zip -d ./protoc-3.19.4		// unzip 必须指定 -d 解压释放的目录, 否则都放当前了

2.3. 将 protoc 可执行文件放入操作系统环境变量 PATH, vim ~/.bashrc
$ export PATH="$PATH:$HOME/.local/bin"

# protoc
export PATH="$PATH:/home/chenchen/tmp/protoc-3.19.4/bin"

2.4. 查看版本
[chenchen@grpc01 protoc-3.19.4]$ ./bin/protoc --version
libprotoc 3.19.4

//参考: https://grpc.io/docs/protoc-installation/
```



## Go plugins for the protocol compiler

```go
// 安装 Go 编译器插件
// 1. 因为 go install 会将 compiler 安装到 GOBIN 下面. protoc-gen-go 和 protoc-gen-go-grpc
$ go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
$ go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest

$ go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.26
$ go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.1

// 2. 更新操作系统环境变量, 为了让 编译器 protoc 能找到插件, vim ~/.bashrc
export PATH=$PATH:/home/chenchen/tmp/go1172/go/bin
export GOPATH=/home/chenchen/tmp/go1172/gopath
export GOROOT=/home/chenchen/tmp/go1172/go
export GOBIN=/home/chenchen/tmp/go1172/gobin

# upx
export PATH=$PATH:/home/chenchen/tmp/upx-3.96-amd64_linux

# protoc
export PATH="$PATH:/home/chenchen/tmp/protoc-3.19.4/bin"

# Go plugins
export PATH="$PATH:$(go env GOBIN)"

// 3. 查看版本
[chenchen@grpc01 bin]$ protoc-gen-go --version
protoc-gen-go v1.27.1

// 参考: https://grpc.io/docs/languages/go/quickstart/
```



## 说明

```shell
# protoc
protoc 是 PB 编译器, 与 go 无关, 用来处理 .proto 文件

# go 插件
protoc-gen-go
protoc-gen-go-grpc
```



## See Also

https://grpc.io/docs/protoc-installation/

https://grpc.io/docs/languages/go/quickstart/









