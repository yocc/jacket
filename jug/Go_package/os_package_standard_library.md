# os_package_standard_library

## 概述
包 os 为操作系统功能提供了一个独立于平台的界面。设计是 Unix 样的，虽然错误处理是 Go 样：失败的呼叫返回类型错误值，而不是错误号码。通常，在错误中会获得更多信息。例如，如果需要文件名的呼叫失败（如 Open 或 Stat），则错误将包括打印时出现的故障文件名称，并且属于类型 *PathEror，该类型可能会解包以获得更多信息。

os 接口旨在在整个操作系统中均匀。一般不可用的功能显示在系统特定的包系统呼叫中。

下面是一个简单的示例，打开一个文件并阅读其中的一些内容。

```go
file, err := os.Open("file.go") // For read access.
if err != nil {
	log.Fatal(err)
}
```

如果打开失败，错误字符串将不言自明，如

```go
open file.go: no such file or directory
```

然后，文件的数据可以读入字节。读写从参数切片的长度中取出字名计数。

```go
data := make([]byte, 100)
count, err := file.Read(data)
if err != nil {
	log.Fatal(err)
}
fmt.Printf("read %d bytes: %q\n", count, data[:count])
```

注意：文件上同时操作的最大数量可能受操作系统或系统的限制。这个数字应该很高，但超过它可能会降低性能或导致其他问题。

