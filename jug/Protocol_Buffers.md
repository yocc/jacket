# [Protocol Buffers](https://developers.google.com/protocol-buffers)

---

[toc]



## Overview

- Protocol buffers  提供一种语言中立, 平台中立, 可扩展的机制, 用于以向前兼容和向后兼容的方式序列化结构化数据.

  它类似于JSON, 只是它更小, 更快. 并且它生成本地语言绑定.

- 重点:

  .proto 文件, 用来描述 *messages* 和 _service_

  protoc, proto 编译器, 生成的代码 = protoc(.proto file, 开发语言), 生成的代码的内容是一个 class, class 中包含可以简单访问每个字段的*访问器*, 还包含了用于序列化和解析整个结构的*方法*

  data = data + 不同语言的运行时库 + 序列化格式

  data 写入 文件 或者 网络



## PB 如何工作的?

![img](https://developers.google.com/protocol-buffers/docs/images/protocol-buffers-concepts.png)

PB 工作流:

1. 创建 .proto 文件, 此文件定义了要传输的数据的结构

2. protoc 编译器 + .proto 文件 => 生成开发语言源文件

3. 根据业务需求调整 生成开发语言源文件 相关代码, 最后打包编译成独立模块

4. 利用这个独立模块来序列化/反序列化数据

   

## 什么时候 PB 不合适？

PB 不适合所有数据. 特别细节:

- PB 倾向于假定整个消息可以一次性加载到内存, 并且不大于对象图. 对于超过几兆字节的数据, 请考虑使用其他解决方案;

  在处理较大的数据时, 由于序列化机制的问题, 这可能会导致内存使用量出现惊人的峰值.

- 当序列化 PB 时, 相同的数据可以具有许多不同的二进制序列化. 如果不完全解析这两条消息, 就无法比较它们是否相等.

- 消息不压缩. 虽然消息可以像任何其他文件一样 zip 或 gzip, 但特殊用途的压缩算法(如 JPEG 和 PNG 使用的算法)将为适当的类型的数据生成小得多的文件.

- 对于许多涉及浮点数的大型多维数组的科学和工程用途而言, PB 消息在大小和速度方面都不够高效. 对于这些应用程序, [FITS](https://en.wikipedia.org/wiki/FITS)和类似格式的开销较小.

- PB 在科学计算中流行的非面向对象语言(如Fortran和IDL)中没有得到很好的支持.

- PB 消息本身并不自描述其数据, 但它们具有完全反射的架构, 可用于实现自描述. 也就是说, 如果不访问其相应的 .proto 文件, 则无法完全解释一个.

- PB 不是任何组织的正式标准. 这使得它们不适合在具有法律或其他要求的环境中使用, 以在标准之上构建.





## See Also

https://developers.google.com/protocol-buffers/docs/overview

https://developers.google.com/protocol-buffers/docs/proto3























