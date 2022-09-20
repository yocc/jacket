# go type

----

[toc]





## Overview

### 类型转换

https://blog.csdn.net/kevin_tech/article/details/125013359

#### 显式

```go
// type_name(expression)

b := int16(a)
s2 := []byte(s1)
s3 := string(s2)

仅能用于将结构体类型转换接口类型, 而不能将接口类型转为结构体类型.
s2 := People(s1)
```

#### 隐式

```go
隐式转换, 是编译器所为, 开发者并不会感觉到发生了变化.
```



#### 类型断言





#### 重新构造对象





#### 类型动态转换/查询

只有对 `接口对象` 才能执行 `类型动态转换/查询`

```go
// 上面代码是将 `data` 转换成 *VerifyCodePhoneRequest 类型
_data := data.(*VerifyCodePhoneRequest)

// 只有对 接口对象 才能执行 类型动态转换/查询. 因此, packet.TransportLayer() 返回的是一个 接口对象.
tcp := packet.TransportLayer().(*layers.TCP)
```





## FAQ & troubleshooting





## See Also

