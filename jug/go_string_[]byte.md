# go string []byte 区别

----

[toc]





## Overview

### difference

```go
1. 
var s string
s = "qqqq"			// 分配存储 "qqqq" 的内存空间, s 结构体里的 str 指针指向这块内存
s = "bbbb"			// 重新给 "bbbb" 分配内存空间, s 结构体里的 str 指针指向这块内存
// string 类型的变量 s 再次赋值时是再次新建了一块新内存放 "bbbb", 同时修改了指针指向的位置

// []byte 类型是直接在内存空间上改变值, 如下:
s := []byte{1} // 分配存储 1 数组的内存空间, s 结构体的 array 指针指向这个数组
s = []byte{2}  // 将 array 的内容改为 2

string 操作低效的根本原因: 
因为 string 的指针指向的内容是不可以更改的, 所以每更改一次字符串, 就得重新分配一次内存, 之前分配空间的还得由 gc (Garbage Collection) 回收, 这是导致string 操作低效的根本原因

2. 
string 可以直接比较
[]byte 不能直接比较, 所以 []byte 不可以当 map 的 key 值

3. 
string 可以是 map 的 key 值
[]byte 不行

4.
string 应该说字符串的值不能被更改, 但可以被替换
type stringStruct struct {
    str unsafe.Pointer
    len int
}
str 指针指向的是一个字符常量的地址, 这个地址里面的内容是不可以被改变的, 因为它是只读的, 但是这个指针可以指向不同的地址

我们来对比一下string、[]byte类型重新赋值的区别：
```



### convert

```go
// s 由 string 类型转换成 []byte 类型
var s string
[]byte(s)
//
string 和 []byte 的相互转换
将 string 转为 []byte, 语法 []byte(string) 源码如下:

go 源码 src/runtime/slice.go
func stringtoslicebyte(buf *tmpBuf, s string) []byte {
    var b []byte
    if buf != nil && len(s) <= len(buf) {
        *buf = tmpBuf{}
        b = buf[:len(s)]
    } else {
        b = rawbyteslice(len(s))
    }
    copy(b, s)
    return b
}
 
func rawstring(size int) (s string, b []byte) {
    p := mallocgc(uintptr(size), nil, false)
 
    stringStructOf(&s).str = p
    stringStructOf(&s).len = size
 
    *(*slice)(unsafe.Pointer(&b)) = slice{p, size, size}
 
    return
}
可以看到 b 是新分配的, 然后再将 s 复制给 b, 至于为啥 copy 函数可以直接把 string 复制给 []byte, 那是因为 go 源码单独实现了一个 slicestringcopy 函数来实现, 具体可以看 src/runtime/slice.go

将 []byte 转为string, 语法 string([]byte) 源码如下:
go 源码 src/runtime/slice.go
func slicebytetostring(buf *tmpBuf, b []byte) string {
    l := len(b)
    if l == 0 {
        // Turns out to be a relatively common case.
        // Consider that you want to parse out data between parens in "foo()bar",
        // you find the indices and convert the subslice to string.
        return ""
    }
    if raceenabled && l > 0 {
        racereadrangepc(unsafe.Pointer(&b[0]),
            uintptr(l),
            getcallerpc(unsafe.Pointer(&buf)),
            funcPC(slicebytetostring))
    }
    if msanenabled && l > 0 {
        msanread(unsafe.Pointer(&b[0]), uintptr(l))
    }
    s, c := rawstringtmp(buf, l)
    copy(c, b)
    return s
}
 
func rawstringtmp(buf *tmpBuf, l int) (s string, b []byte) {
    if buf != nil && l <= len(buf) {
        b = buf[:l]
        s = slicebytetostringtmp(b)
    } else {
        s, b = rawstring(l)
    }
    return
}
依然可以看到 s 是新分配的, 然后再将 b 复制给 s.
正因为 string 和 []byte 相互转换都会有新的内存分配, 才导致其代价不小, 但读者千万不要误会, 对于现在的机器来说这些代价其实不值一提.

但如果想要频繁 string 和 []byte 相互转换(仅假设), 又不会有新的内存分配, 能有办法吗? 答案是有的.
package string_slicebyte_test
 
import (
    "log"
    "reflect"
    "testing"
    "unsafe"
)
 
func stringtoslicebyte(s string) []byte {
    sh := (*reflect.StringHeader)(unsafe.Pointer(&s))
    bh := reflect.SliceHeader{
        Data: sh.Data,
        Len:  sh.Len,
        Cap:  sh.Len,
    }
    return *(*[]byte)(unsafe.Pointer(&bh))
}
 
func slicebytetostring(b []byte) string {
    bh := (*reflect.SliceHeader)(unsafe.Pointer(&b))
    sh := reflect.StringHeader{
        Data: bh.Data,
        Len:  bh.Len,
    }
    return *(*string)(unsafe.Pointer(&sh))
}
 
func TestStringSliceByte(t *testing.T) {
    s1 := "abc"
    b1 := []byte("def")
    copy(b1, s1)
    log.Println(s1, b1)
 
    s := "hello"
    b2 := stringtoslicebyte(s)
    log.Println(b2)
    // b2[0] = byte(99) unexpected fault address
 
    b3 := []byte("test")
    s3 := slicebytetostring(b3)
    log.Println(s3)
}
答案虽然有, 但强烈推荐不要使用这种方法来转换类型, 因为如果通过 stringtoslicebyte 将 string 转为 []byte 的时候, 共用的是同一块内存, 原先的 string 内存区域是只读的, 一但更改将会导致整个进程 down 掉, 而且这个错误是 runtime 没法恢复的.
```



### when

```go
1. 需要操作字符串中的粒度为某个字符时, 用 []byte
2. string 可以是空字符串, 但不能为 nil 值, 所以如果你想要通过返回 nil 表达额外的含义, 就用 []byte
3. []byte 切片这么灵活, 想要用切片的特性就用 []byte
4. 需要大量字符串处理的时候用 []byte, 性能好很多
5. 最后脱离场景谈性能都是耍流氓, 需要根据实际场景来抉择
```



### type string

> type string
> string is the set of all strings of 8-bit bytes, conventionally but not necessarily representing UTF-8-encoded text. A string may be empty, but not nil. Values of string type are immutable.
>
> from Standard library builtin 

字符串时一系列8位字节的集合. 

```go
在 go 的源码中 src/runtime/string.go, string 的定义如下:
type stringStruct struct {
    str unsafe.Pointer
    len int
}

实例化 stringStruct 这个结构体:
func gostringnocopy(str *byte) string {
    ss := stringStruct{str: unsafe.Pointer(str), len: findnull(str)}
    s := *(*string)(unsafe.Pointer(&ss))
    return s
}
其实就是 byte 数组, 而且要注意 string 其实就是个 struct


```



### []byte

byte 是 uint8 的别名.

```go
而 slice 结构在 go 的源码中 src/runtime/slice.go 定义:
type slice struct {
    array unsafe.Pointer
    len   int
    cap   int
}
array 是数组的指针, len 表示长度, cap 表示容量.
除了 cap, 其他看起来和 string 的结构很像, 但其实他们差别真的很大.
```





## FAQ & troubleshooting





## See Also

https://blog.csdn.net/weixin_30237281/article/details/96134962

https://blog.csdn.net/weixin_30237281/article/details/96134962

https://blog.csdn.net/qq_34788903/article/details/102891966?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-102891966-blog-96134962.pc_relevant_multi_platform_whitelistv3&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-102891966-blog-96134962.pc_relevant_multi_platform_whitelistv3&utm_relevant_index=1





